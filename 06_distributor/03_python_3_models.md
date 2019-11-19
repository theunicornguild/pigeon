Now we need to edit the previous model that we created `Group`

**Go to `main/models.py` and add the following imports:**

```python
from main.functions import sendSms,sendEmail
```

Then go to the `Group` model and add the following, ( IT MUST BE INDENTED INTO THE GROUP MODEL ) sorry for the caps but I had to do it:

```python
   def sms(self, message):
       for recipient in self.members.all():
           sendSms(recipient,message)

   def email(self, subject,message):
       for recipient in self.members.all():
           sendEmail(recipient,subject,message)
```

Here we have two model methods, both do kinda the same thing they loop through all members of a group and either send them an SMS message or an Email with the functions we created previously

Just a little extra thing that we can create is a `property` which is a calculated field in a model, in this case we want to have access to how many members inside each group at all times, and to do that lets indent this portion too to the Group model

```python
   @property
   def counter(self):
       return len(self.members.all())
```
