We still need to create a view for this new url and call it `recipientsList` and to do that we need some imports first, and we will get to explaining them

**Type the following at the top of the views.py file located at `main/views.py`**

```python
from django.core.paginator import Paginator
from django.db.models import Q
```

> Imports:

- `Paginator` : which is a system given to us by Django by default to handle having more than one page in a view, so instead of showing 100 values at the same time in a page we can limit that search limit to be 20 or 5 or whatever we want
- `Q` : this little thing will help us in advanced filters while doing some queries to the database, say for example you want to search for anyone with the name `x` or the phone number `x` at the same time, this is where `Q` shines, it can help you do that with ease

Now am going to start off by giving you the full view because its kinda big then we will start cutting it down to smaller points so you can understand it easier

**So type the following in your `views.py` file**

```python
def recipientsList(request,query):
   search = request.GET.get('search')
   group = Group.objects.filter(id=query)
   if not group.exists():
       group = Group.objects.first()
   else:
       group = group.first()
   if request.method == "POST":
       requestType = request.POST.get('type')
       if requestType == "add":
           recipientId = request.POST.get('recipient')
           recipient = Recipient.objects.get(id=recipientId)
           group.members.add(recipient)
       elif requestType == "delete":
           recipientId = request.POST.get('recipient')
           recipient = Recipient.objects.get(id=recipientId)
           group.members.remove(recipient)
   if search:
       queryset = Recipient.objects.filter(Q(name__contains=search) | Q(email__contains=search) | Q(number__contains=search))
   else:
       queryset = Recipient.objects.all()
   queryset.order_by('pk')
   paginator = Paginator(queryset, 25)
   page = request.GET.get('page')
   recipients = paginator.get_page(page)
   context = {
       'group': group,
       'recipients': recipients,
   }
   if search:
       context['search'] = search
   return render(request,'admin/recipients.html',context)
```

Wow that is huge isn’t it, well emm not really let's break it down

> Explanation:

- First we defined the view, it accepts two things a `request` and a `query` which is the ID of the group we would like to quickly add or remove users from
- Then we defined something called `search` which will also get an optional `GET` parameter from the request
- We use the `query` to find the group we would like to interact with
- If the group doesn’t exist it will just grab the first group it finds in the database
- Then it will check if the request method was `POST` and if it was it will grab something called `type` we pointed out before in the `distributor` view as a way to differentiate between two functionalities
- After that it check if the type was `add`, and if it was it will take a `POST` parameter from the request and use it to find a specific recipient
- It will then add that recipient to the group we specified in the `query`
- If it wasn’t an `add` type and it was a `delete` type it will do the opposite, it will take the recipient and remove him from the `Group`
- After that we finish the `POST` portion of the code
- Now we check if the variable `search` exists and if it does we query the Recipient list differently
- We use `Q` to filter through the Recipient objects for a match in either the name, email or the phone number
- If search wasn’t defined we just grab all the Recipients from the database
- We then use a function given to us by Django which is `order_by` to specify how do we want to order the list of Recipients we just got, I chose by `pk` which is the primary key, and in this case it’s the ID of the recipient
- We jump now to the `Paginator` and for it to work we give it the queryset that we created (the list of Recipients we generated either by filtering or all of them) and a limit number, in this case its `25` so we can only see 25 recipients at a time
- We also check if there is a `page` get parameter and if it does exist we tell the `Paginator` to grab that page, so think about it like this if we have 100 recipients and it's the 2nd page its gonna show us recipients 25-50 instead of 1-25 etc…
- After that we bundle everything into a dictionary called `context` and send it to the HTML template using the `render` function

This might take a while to set in but try reading each point and looking at the code to grasp it more

With that we are finished with the views and we can jump in to the template
