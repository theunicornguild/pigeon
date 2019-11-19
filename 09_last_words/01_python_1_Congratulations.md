We have created an easier way to add/remove users to and from groups and it was very similar to how the normal structure of Django works, create a View + URL + Template and everything should be set, although we did not touch the url in any `urls.py` file and it was all done in the `admin.py` side of things because we were actually adding urls to an existing application and not creating one from scratch ( the application in this instance is the admin interface )

After you check that everything is working perfectly, let's move to the Trello section

## Trello

> Move the card in the `Doing` section to the `Review` section and check if everything is working perfectly then have someone review your work before moving it to the `Done` section

## Git

Create a checkpoint

```bash
git add .
git commit -m “Finished adding & removing users in quicker manner”
git push
```
