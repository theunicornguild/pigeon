start importing the stuff we are gonna need

**Go to `main/admin.py` and open it up with your favourite code editor**

Type the following

```python
from import_export import resources
from import_export.admin import ImportExportModelAdmin
```

And now we can create a `resource` that we would be importing and exporting from and define some interesting stuff while doing so

Type the following in the admin.py file

```python
class RecipientResources(resources.ModelResource):
   class Meta:
       model = Recipient
       skip_unchanged = True
       report_skipped = True
```

And this will inherit from `resources.ModelResouce` and we don’t really need to know what that does but if your interested I linked the documentation in the Introduction for this chapter

In the `class Meta` we defined three things, the model that we want to be using in this case `Recipient`
And then two things which are `skip_unchanged` and `report_skipped` and those will skip recipients that already exist and will tell you how many are skipped in the importing portion

After that we will write a second class, so type the following underneath it

```python
class RecipientAdmin(ImportExportModelAdmin):
   resource_class = RecipientResources
   search_fields = ('name','email','number')
```

And this will help us in customizing the model in the admin page itself so we can define some attributes that we can search for recipients with, in this case we are searching for people with their name/email/phone number

After we are done with that we need to edit the previous registration of the recipient model

So edit the following

```python
admin.site.register(Recipient)
```

And make it look like this

```python
admin.site.register(Recipient,RecipientAdmin)
```

So now it won’t be using the default configuration for any model in the admin page but it will be using a customized version that we just created and allows for us to search for recipients with fields

`( Keep in mind all the admin.site.register portions need to be at the bottom of the page for it to take affect or else it will give you an error )`
