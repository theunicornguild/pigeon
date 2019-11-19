Great! Everything works so far, But we don’t want to see the name of the group when we trigger the Distributor action, instead we want to have the ability to send them an SMS or an email

**So go back to your code editor and open up `main/views.py`:**

Start off by importing some stuff that we will need for this function, at the top of the page fix the imports to look like this and we will explain anything new

```python
from django.shortcuts import render, redirect
from main.models import Group
from django.core.files.storage import FileSystemStorage
```

> Imports:

- We are importing both `render` and `redirect` to control the flow of the request, either we redirect the user to a different page is something is wrong or we render a HTML template if we have some information to show him
- Importing the model Group to do some queries
- The final bit might be a little bit complicated, But you should understand it easily, If you are a Django veteran you should know how to upload files yea? Well you might remember that whenever we actually uploaded files we used a field in the models either an `ImageField` or a `FileField`.. But in this case we don’t really want to link this html template to any group it is a one time thing that we don’t need saved, And so we imported the `FileSystemStorage` to allow us to create files on the server without the need to create a `FileField/ImageField` in the existing modelset

OK! So that was some explaining but don’t worry the rest is a piece of cake

Let’s start off by removing the old `distributor` view and creating this new one and we can go ahead and explain it:

```python
def distributor(request,query):
   group = Group.objects.filter(id=query)
   if not group.exists():
       return redirect('/')
   else:
       group = group.first()
   if request.method == "POST":
       requestType = request.POST.get('type')
       if requestType == "sms":
           message = request.POST.get('message')
           group.sms(message)
       elif requestType == "email":
           template = request.FILES['template']
           fs = FileSystemStorage()
           filename = fs.save(template.name, template)

           subject = request.POST.get('subject')
           group.email(subject,str(fs.path(filename)))
           fs.delete(filename)
   context = {
       'group': group,
   }

   return render(request,'admin/distributor.html',context)
```

> The flow of the view:

- The view will receive a request with a get parameter `query`
- It will check if the `query` holds an actual `Group` ID and if it doesn’t it will redirect the user to the home page
- It will save the group it found in a parameter called `group`
- It then will check if the request method was `POST` or not
- If it was a `POST` request then there is a hidden post request parameter which is `type` and this type will determine if we were going to send an SMS or an Email, This will happen in the Template section which is up next so don’t worry
- If it was an `SMS` type it will grab a `message` and use the model method we built previously to send it
- If it was an `Email` type then it will grab a file from the request and save it as the variable `template`
- After that we will save the template using the `FileSystemStorage` to a variable called `filename`
- We will grab the subject for the email and then activate the model method for sending SMS messages
- After we finish sending all the emails we will delete that file as its no longer needed
- We define a context dictionary that will populate the `group` variable in the HTML template
- Then we render that HTML template

Well we can’t really test so far sadly, so we have to create the template first before we can check if our work actually works :D, Let’s cover that in the next chapter!
