**Go to `pigeon/settings.py`**
And at the bottom of the file you should see the following:

```python
NEXMO_API_KEY = "API_KEY"
NEXMO_API_SECRET = "API_SECRET"
EMAIL_USE_TLS = True
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_HOST_USER = 'MY_EMAIL@gmail.com'
EMAIL_HOST_PASSWORD = 'PASSWORD'
EMAIL_PORT = 587
```

For this section I’ll just need you to either make a new gmail email or any other that offers an SMTP protocol for connection, `most email services offer that` and we gave you the default settings for gmail just switch around the `MY_EMAIL@gmail.com` portion with yours and the `PASSWORD` portion with yours too

You should be set with the Email stuff for now, but for the SMS to work you need to register an account at https://www.nexmo.com/

After that you should get a `API_Key` and `API_Secret` from them, fill the information into their section at `NEXMO_API_KEY & NEXMO_API_SECRET`

And you are set
