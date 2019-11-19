In order to create the distributor template we need a `templates` folder

Go to the folder `main` in your project and inside of it create a folder called `templates`

After that create another folder inside of the newly created `templates` and call it `admin`

The final path should look like this `main/templates/admin/`

Then create a new file inside of it and it should be called `distributor.html`

**The final path for it should look like this too `main/templates/admin/distributor.html`**

And insert the following code:

```HTML
{% extends "admin/base_site.html" %}
{% block branding %}
<h1 id="site-name">Pigeon Headquarters</h1>
{% endblock %}
{% block content %}
<h1>{{group.name}}</h1>
<div class="js-inline-admin-formset inline-group" data-inline-type="tabular">
   <div class="tabular inline-related last-related">
       <fieldset class="module ">
           <h2>Distributor - {{group.members.count}} recipients</h2>
           <table>
               <thead>
                   <tr>
                       <th class="original"></th>
                       <th>Method</th>
                       <th>Data</th>
                       <th>Action</th>
                   </tr>
               </thead>
               <tbody>
                   <tr class="form-row row1 has_original dynamic-Group_members">
                       <form action="?&query={{group.id}}" method='POST'>
                           <td class="original">
                           </td>
                           <td>
                               SMS
                           </td>
                           <td class="delete">
                               {%csrf_token%}
                               <input type="hidden" name="type" value="sms">
                               <textarea name="message" rows="4" cols="50" value=""></textarea>
                           </td>
                           <td>
                               <input type="submit" value="Send">
                           </td>
                       </form>
                   </tr>
               </tbody>
               <tbody>
                   <tr class="form-row row1 has_original dynamic-Group_members">
                       <form action="?&query={{group.id}}" method='POST' enctype="multipart/form-data">
                           <td class="original">
                           </td>
                           <td>
                               Email
                           </td>
                           <td class="delete">
                               {%csrf_token%}
                               <input type="hidden" name="type" value="email">
                               <input type="text" name="subject" placeholder="Subject">
                               <input type="file" placeholder="Template" name="template" />
                           </td>
                           <td>
                               <input type="submit" value="Send">
                           </td>
                       </form>
                   </tr>
               </tbody>
           </table>
       </fieldset>
   </div>
</div>
{% endblock %}
```

Ok this is a big file but keep in mind most of it is for styling purposes, all the class names are from the Django admin interface css files, but for most customizing you can find a lot of information here too https://docs.djangoproject.com/en/2.1/ref/contrib/admin/#templates-which-may-be-overridden-per-app-or-model

> Ok let’s start and i’ll try and make these into points so its easier to get back to them:

- `{% extends “admin/base_site.html” %}` this comes from the main base file that the Django admin has, We actually have no control over it but it's what gives the django admin page the looks that it has and we want to use that to our advantage
- `branding` block is where we have our page Header, sadly even though we added it in the normal admin sites because this is a completely new page we have to do this one manually
- `content` block is where all our HTML code lives, forms and showing data etc will all be here
- Inside the `content` block we are showing information about the `Group` we are about to send an SMS to like `group.name` or `group.members.count`
- We also have two forms, One that will handle sending an SMS and the other one will handle sending an Email
- Both forms have a `csrf_token` for security reasons `( read about it here https://docs.djangoproject.com/en/2.1/ref/csrf/ )`
- Also in the form that handles uploading templates we added a portion which is `enctype="multipart/form-data"` which allows us to expect files being sent from this form
- We finish it all with a endblock for the content section
