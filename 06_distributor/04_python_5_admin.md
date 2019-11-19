After the changes we made to the model in the last chapter let’s move to the admin.py file

**Go to `main/admin.py`:**

And define a function called `distributor` which will act as an `Action button` on the list page for `Groups`

It should look like this

```python
def distributor(modeladmin, request, queryset):
   query = queryset.first().id
   return redirect('admin:distributor', query=query)
distributor.short_description = "Select one group to send messages"
```

This function will take 3 things by default as per the Django documention which you can read here https://docs.djangoproject.com/en/2.2/ref/contrib/admin/actions/#adding-actions-to-the-modeladmin

We will mainly use the queryset given to this function and take the first one (which is the only one we need), because by default you can pick more than one group to do an action to and we want to focus mainly on sending bulk emails/SMS to one group instead of more than one at a time

After we get the ID for the group that we want we redirect the user to a view that isn’t created yet (will be soon) with the ID of the group that we want to send stuff to

We can also add a short description for that action to make it clear what does it do when you hover over it

After we’ve finished that we can now switch back to the `GroupAdmin` class and make some edits,

First we will add this new line in the class

```python
actions = [distributor]
```

Which is the action we just created, And also we will edit the `list_display` that we had previously to look like this `( Notice we also added the counter property we defined in the model here )`

```python
   list_display = (
       'name',
       'counter',
       'group_actions',
   )
```

And after that we want to define a method function inside that class that looks like this

```python
   def group_actions(self, obj):
       return format_html('<a class="button" href="{}">Distributor</a>&nbsp;',reverse('admin:distributor', args=[obj.id]))

```

And this will display an HTML code as an action in the list page of Groups, it should look very cool after you test it out ! :D just give us a moment to fix more stuff before testing though

The overall `GroupAdmin` class should look like this at the end

```python
class GroupAdmin(admin.ModelAdmin):
   search_fields = ('name',)
   actions = [distributor]
   inlines = [MembershipInline]
   list_per_page = 20
   exclude = ('members',)
   list_display = (
       'name',
       'counter',
       'group_actions',
   )
   def group_actions(self, obj):
       return format_html('<a class="button" href="{}">Distributor</a>&nbsp;',reverse('admin:distributor', args=[obj.id]))
```

Last but not least we need to define a URL for our newly creation distributor function

So go to `MyAdminSite` class and add a class method function named `get_urls` that should look like this:

```python
   def get_urls(self):
       urls = super(MyAdminSite, self).get_urls()
       custom_urls = [
           path('groups/distributor/<int:query>', self.admin_view(views.distributor), name="distributor"),
       ]
       return urls + custom_urls
```

This will add more urls to the admin site, and this one will be for the `distributor` function in this case

OK! Enough is enough we’ve added so many stuff and didn’t import so it's probably gonna break now :(

To prevent that go up to the top of the page and let’s import the following

```python
from main import views
from django.urls import path
from django.shortcuts import  redirect
from django.utils.html import format_html
from django.urls import reverse

```

But there is something missing… we still don’t have a `views.distributor` function and it will still break

To make sure everything is working before we go on any further.

**Go to `main/views.py` and type the following as a test code for now**

```python
from django.shortcuts import render, redirect
from django.http import HttpResponse
from main.models import Group

def distributor(request,query):
   group = Group.objects.filter(id=query)
   if not group.exists():
       return redirect('/')
   else:
       return HttpResponse(group.first().name)
```

This will test if we do actually get a Group with the specific ID that should be in the query parameter, if so then it will send us an HttpResponse with the name of that group, Again we are doing this just to make sure that we are getting the correct ID and that the view actually works at this point of time
