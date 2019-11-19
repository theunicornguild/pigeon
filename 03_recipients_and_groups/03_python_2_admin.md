This will be the unfriendliest admin.py you might ever interact with, but for now we will just register the current models and get things set, later on most of our work will be within the `admin.py` file to highly customize how we interact with those two models and add more functionalities

**Open up this file in your code editor `main/admin.py` and type the following:**

First import the models

```python
from django.contrib import admin
from main.models import Group,Recipient
```

And then let’s register the models

```python
admin.site.register(Recipient)
admin.site.register(Group)
```
