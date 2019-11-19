Let’s start this new chapter with creating a new file in the `main` folder and naming it `functions.py`

**Open up `main/functions.py` with your prefered code editor and do the following:**

Import the following at the top of the file

```python
from django.template.loader import render_to_string
from django.utils.html import strip_tags
from django.core.mail import send_mail
from django.conf import settings
import nexmo
```

> Let’s explain the imports:

- Render_to_string: will involve reading an html template and sending it as an email to recipients
- Strip_tags: will create a text version of that html template for devices that don’t use HTML
- Send_email: will handle sending the email with its subject/body/recipient’s email
- Settings: will use that to get some stuff that we will define in the `settings.py` file later on
- Nexmo: is the client that we will use to handle sending SMS messages using their Python SDK ( which is easier than using an API )

So now we are familiar with all these imports we can start coding the functions, let’s begin with the SMS function,

Type the following:

```python
def sendSms(recipient,message):
   client = nexmo.Client(key=settings.NEXMO_API_KEY, secret=settings.NEXMO_API_SECRET)
   client.send_message({
       'from': 'Pigeon',
       'to': str(recipient.number),
       'text': message
       })
```

We defined a function that will take two parameters,

- Recipient object
- String which is the message

Then we define the client using `nexmo.Client` and some API keys that we will get around to later

After that its a simple function which is `send_message` that takes a dictionary with 3 fields,

- From : which is the name/number that will be sending the message
- To: the number that we will send an SMS to ( keep in mind we have to turn it into a string hence the usage of `str`
- Text: which is the message that we want to send

Pretty simple yeah?

Now we need to switch over to the email part

Type the following

```python
def sendEmail(recipient,subject,message):
   context = {
       'recipient': recipient
   }
   html_content = render_to_string(message, context=context)
   text_content = strip_tags(html_content)
   send_mail(subject, text_content, settings.EMAIL_HOST_USER , [recipient.email,], fail_silently=True, html_message=html_content)
```

This function will also take a recipient object, a message and a subject (title) for the email
Although the message in this case is not a string but a HTML template that we will upload !

We will define a context dictionary just like we do in a normal view, then html_content will populate the HTML template with the context, in this case the recipient’s information

Then we get a text_content version of that HTML template with no html strips, `think <body> <div> etc`

After that we use the `send_email` function to send the email, we provide it will all the information that it needs but we will still need to give it a `EMAIL_HOST_USER` which is something that we will write later on in the `settings.py` file to finalize this whole thing, Also keep in mind we have `fail_silently` set as True so it won’t give us errors etc when an email is failed for whatever reason and just to make things more smoothly
