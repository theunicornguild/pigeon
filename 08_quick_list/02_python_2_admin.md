**Go to the admin.py file located at `main/admin.py` and do the following:**

We defined a class method in the `GroupAdmin` class prior to this, we will need to edit that function and add a new url to the formatted html,

So edit the return value to look like this

```python
return format_html('<a class="button" href="{}">Distributor</a>&nbsp;''<a class="button" href="{}">Quick Add/Delete</a>&nbsp;',reverse('admin:distributor', args=[obj.id]),reverse('admin:recipientsList', args=[obj.id]))
```

You will notice we added a button called `Quick Add/Delete` which will send us to a new url we still haven’t created and it's called `recipientsList`

Go and find `MyAdminSite` and in the `get_urls` class method we will also add a new custom_url to the list, add the following to the list:

```python
path('recipients/groups/<int:query>', self.admin_view(views.recipientsList), name="recipientsList"),
```

`Don’t forget the comma at the end there :D`

And we should be done with this file for now!
