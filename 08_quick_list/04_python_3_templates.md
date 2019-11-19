As you might have noticed in the `recipientsList` view we were using a template called `recipients.html` and that doesn’t exist so far, so we need to start creating that

Go to `main/templates/admin/` folder and create a new file `recipients.html`

It will follow the same basics we had in the previous HTML template we created as we will be extending from the base of the Django admin interface, then creating a header by using the `branding` block and finally putting the HTML code for our view in the `content` block

**Type the following in the newly created `recipients.html` file:**

```html
{% extends "admin/base_site.html" %}
{% block branding %}
<h1 id="site-name">Pigeon Headquarters</h1>
{% endblock %}
{% block content %}
<h1>{{group.name}}</h1>
<div id="toolbar">
 <form id="changelist-search" method="get">
   <div>
     <label for="searchbar"><img src="/static/admin/img/search.svg" alt="Search"></label>
     <input type="text" size="40" name="search" value="" id="searchbar" autofocus="">
     <input type="submit" value="Search">
   </div>
 </form>
 <div class="js-inline-admin-formset inline-group" id="Group_members-group" data-inline-type="tabular"
   data-inline-formset="{&quot;name&quot;: &quot;#Group_members&quot;, &quot;options&quot;: {&quot;prefix&quot;: &quot;Group_members&quot;, &quot;addText&quot;: &quot;Add another Group-recipient relationship&quot;, &quot;deleteText&quot;: &quot;Remove&quot;}}">
   <div class="tabular inline-related last-related">
     <input type="hidden" name="Group_members-TOTAL_FORMS" value="10" id="id_Group_members-TOTAL_FORMS" autocomplete="off"><input
       type="hidden" name="Group_members-INITIAL_FORMS" value="7" id="id_Group_members-INITIAL_FORMS"><input type="hidden"
       name="Group_members-MIN_NUM_FORMS" value="0" id="id_Group_members-MIN_NUM_FORMS"><input type="hidden" name="Group_members-MAX_NUM_FORMS"
       value="1000" id="id_Group_members-MAX_NUM_FORMS" autocomplete="off">
     <fieldset class="module ">
       <h2>Recipients</h2>
   </div>
   <table>
     <thead>
       <tr>
         <th class="original"></th>
         <th>ID</th>
         <th class="column-recipient required">Recipient
         </th>
         <th>Action</th>
       </tr>
     </thead>
     <tbody>
       {% for recipient in recipients %}
       <tr class="form-row row1 has_original dynamic-Group_members">
         <td class="original">
         </td>
         <td>
           {{ recipient.id }}
         </td>
         <td class="field-recipient">
           <strong>{{recipient.name}}</strong>
         </td>
         <td class="delete">
           {% if group in recipient.groups.all %}
           <form action="?page={{recipients.number}}&query={{group.id}}&search={{search}}" method='POST'>
             {%csrf_token%}
             <input type="hidden" name="recipient" value="{{recipient.id}}">
             <input type="hidden" name="type" value="delete">
             <input type="submit" value="Delete" style="background: #ba2121">
           </form>
           {% else %}
           <form action="?page={{recipients.number}}&query={{group.id}}&search={{search}}" method='POST'>
             {%csrf_token%}
             <input type="hidden" name="recipient" value="{{recipient.id}}">
             <input type="hidden" name="type" value="add">
             <input type="submit" class="addlink" value="Add">
           </form>
           {%endif%}
         </td>
       </tr>
       {% endfor %}
     </tbody>
   </table>
   </fieldset>
 </div>
 <div class="pagination">
   <span class="step-links">
     {% if recipients.has_previous %}
     <a href="?page=1&query={{group.id}}&search={{search}}">&laquo; first</a>
     <a href="?page={{ recipients.previous_page_number }}&query={{group.id}}&search={{search}}">previous</a>
     {% endif %}
     <span class="current">
       Page {{ recipients.number }} of {{ recipients.paginator.num_pages }}.
     </span>
     {% if recipients.has_next %}
     <a href="?page={{ recipients.next_page_number }}&query={{group.id}}&search={{search}}">next</a>
     <a href="?page={{ recipients.paginator.num_pages }}&query={{group.id}}&search={{search}}">last &raquo;</a>
     {% endif %}
   </span>
 </div>
</div>
{% endblock %}

```

> We will start explaining new things that are happening in the `content` block only:

- We’ve added a for-loop to go through each `recipient` in the `recipients` found at line 36
- Then we started displaying the information of each recipient
- We also added an if statement that checks if the `recipient` is a member of this current group found at line 47
- If the person is already in the group it will show him a form with a button to Delete the user from the group
- If the person isn’t in the group it will show him a form with a button to Add the user to the group
- We then close the for-loop and added a section for pagination found at line 69 until line 83
- This section will check if there is a previous or next page and then create a url for each page depending on the number of recipients divided the the limit you decided `(we picked 25 in the view)`
- More information about this pagination system can be found here https://docs.djangoproject.com/en/2.1/topics/pagination/

And this will be the end of this chapter
