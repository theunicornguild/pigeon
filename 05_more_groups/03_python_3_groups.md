In this chapter we will handle the `many-to-many` relationships between groups and recipients so we might get introduced to new stuff don’t worry about it and enjoy the ride

**Let’s go to the admin.py file at `main/admin.py` in our code editor**

And start by making a new class called `MembershipInline` and that class will handle how a recipient is a member of a specific group `( Membership can be anything I just liked the word because it can link recipients and groups easier )`

Type the following

```python
class MembershipInline(admin.TabularInline):
   model = Group.members.through
   raw_id_fields = ('recipient',)
   search_fields = ('recipient__name',)
```

And here we say this model will handle the relationship between groups and recipients
We also defined a search field, in this case it's gonna be the recipient’s name
A new thing here is the `raw_id_fields` and if you plan on having a BIG project in the future that relies on the admin page even if your just gonna go to the admin page to edit stuff it's very important to understand it

`raw_id_fields` will transfer a field in the admin page form from a `Select / options` HTML version to an input field… this might seem weird why would we even do that its stupid we want to see a list of options to pick from.. Yeah?

`Example of the Select / options for those confused` https://www.w3schools.com/tags/tryit.asp?filename=tryhtml_select

Well imagine if the `Select / options` list had 1 million entry, your browser and the server itself would crash because it won’t be able to handle querying all that information from the database and displaying it to you on the browser, so we replace it with specific ID to remove that huge amount of data querying

After we’ve done that we can now define the `ModelAdmin` to handle displaying groups,
So after finishing the previous class lets type the following underneath it

```python
class GroupAdmin(admin.ModelAdmin):
   search_fields = ('name',)
   inlines = [MembershipInline]
   list_per_page = 20
   exclude = ('members',)
   list_display = (
       'name',
   )
```

> In this ModelAdmin we defined a few things and let’s take our time to explain them

- Search_fields : will handle how we search for an object in the admin page
- Inlines : will show group members in a separate portion to the one you are used to and make it more user-friendly to interact with it
- list_per_page : will limit the amount we see per page to 20 only (built in pagination)
- exclude : will remove the following field which is members (recipients) from the groups details page because we already replaced it with the `inlines` portion
- list_display : will show us the field that we specify in this case `name` in the list page

Might seem confusing and a lot of information all at once, you can always go back to the Django documentation which should have further information about each field https://docs.djangoproject.com/en/2.2/ref/contrib/admin/
