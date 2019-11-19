Let’s start by creating a recipient model, although if you want to use this application on an existing project that holds data already you might just need to edit the (user/recipient/customers/whatever you call it) model and make it similar to the one we have to get everything working

So let’s open the project in whatever code editor you use, I normally use VS Code as I find it superior to the rest, (JUST MY OPINION)

**Navigate to `main/models.py` and open it up**

Let’s start by creating the model, It should have 3 main points, a Name for the person who will be receiving the email/sms , a Phone number and an Email address, All of them are gonna be character fields but some recipients will only have either a phone number or email address so we need to make those two fields optional

```python
class Recipient(models.Model):
   name = models.CharField(max_length=250)
   number = models.CharField(max_length=250,null=True,blank=True)
   email = models.CharField(max_length=250,null=True,blank=True)

   def __str__(self):
       return self.name
```

And after that we needs groups, and groups will have two main things, a name and group members so let’s type the following

```python
class Group(models.Model):
   name = models.CharField(max_length=250)
   members = models.ManyToManyField(Recipient,related_name="groups")
   def __str__(self):
       return self.name

```

Although in your own project you might have some sort of groups built in already to handle your own users changes will be minimal to this model later on as you will notice and can be applied easily to fit your needs

Don’t forget to makemigrations and migrate after applying any changes to models (we won’t be making any further changes to models that affect the structure anyways so this will be the only and final time we migrate)

Type the following in your main project directory where you can see `manage.py` live

```shell
python manage.py makemigrations
python manage.py migrate
```

And you should be set
