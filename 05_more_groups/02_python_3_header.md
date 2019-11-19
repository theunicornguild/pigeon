We will do a fun and simple thing here ! we will change the admin site’s header to be something else

**To do that we need to go to `main/admin.py` and import the following**

```python
from django.contrib.admin import AdminSite
```

And this will give us the ability to create our own admin site!

Above the `admin.site.register` portion of the code type the following:

```python
class MyAdminSite(AdminSite):
   site_header = 'Pigeon Headquarters'
```

And then define something new underneath this class and call it `admin_site` like the following

```python
admin_site = MyAdminSite(name='admin')
```

And this will create the admin site with the name `admin` which is the default so it will change the header for all other admin pages too

Since we did that now we can register previous models to this new admin site instead

So replace the old admin site registers with these `( The difference is minimal instead of having admin.site we will have admin_site as we are using the newly registered admin site )`

It will look like this

```python
admin_site = MyAdminSite(name='admin')
admin.site.register(Recipient,RecipientAdmin)
admin_site.register(Group)
```

And finally because we are over-riding the admin page we need to change the `urls.py` in the main project folder.

**So go to `pigeon/urls.py` and do the following:**

Import the admin_site you created

```python
from main.admin import admin_site
```

Change the urlpatterns to be like this:

```python
urlpatterns = [
   path('admin/', admin_site.urls),
]
```

This way we are using the urls from the admin_site we generated and not the normal one
And so we should be done with this portion !

NOTE: if you still want access to other models for example like `User` that you normally get from Django you have to register it to this admin site on your own, more information about this can be found here https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#customizing-the-adminsite-class
