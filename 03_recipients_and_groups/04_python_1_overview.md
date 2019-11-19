After we finish we can now create a superuser, then run the server and check if everything is working perfectly

**Type the following in your command line where `manage.py` lives:**

```bash
python manage.py createsuperuser
<Then fill the information to create your own user>
python manage.py runserver
```

Then navigate to http://127.0.0.1:8000/admin and you can login and notice we have our two models there to either add/view/edit/delete them

## Trello

Seems like we finished a mile stone so we can move all of the cards in the `Doing` section in Trello to `Review` check that everything is working and if everything is functional move it to `Done`

## Git

Create a new checkpoint

```bash
git add .
git commit -m “Finished Models”
git push
```
