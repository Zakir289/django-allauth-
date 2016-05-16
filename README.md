#Zakarthi

* To say about me, I am **Java, node.js developer**, But somehow I got fascinatied to pyramid authomatic framwork. After using it I came across Django-allauth framework.

* **Django-allauth** framework is the best framework I have seen for the Authentication module.
It Comes with huge features, which are more than enough for the site.
I have used PyCharm IDE to make this project, Intellij is my favourite IDE and it’s really cool, you can give a try.

* The project constains complete SignUp process, Integrations with Facebook, Linkedin, Gmail and storing the sessions in redis.

* Using MySql or other relational databases for storing the sessions is not a good idea, because for every request it needs to hit the db to get the details of the user, So, I feel using redis for storing the sessions and other customized details of the user works.


**Hints for establishing the above project**


**Step1**

       --> django-admin startproject zakarthi


      --> python manage.py startapp Auth

      --> python mange.py makemigrations

     --> python manage.py migrate

      --> python manage.py runserver 0.0.0.0:Port_number


**Step 2.** Add the below code to INSTALLED_APPS in settings.py.

```
 INSTALLED_APPS = [
 'django.contrib.sites',
   'allauth',
   'allauth.account',
   'allauth.socialaccount',
   'allauth.socialaccount.providers.facebook',
   'allauth.socialaccount.providers.google',
   'allauth.socialaccount.providers.linkedin',]
```





**Step 3.** Add Redirect_url to **setttings.py**, the url to which the site is redirected once the site is authenticated.
```
  LOGIN_REDIRECT_URL = '/landing'
  ````

**step 4.** Add **redis** to **settings.py**, so that sessions will be stored on redis. Make sure redis is running on localhost:6379


```
CACHES = {
"default": {
   "BACKEND": "django_redis.cache.RedisCache",
   "LOCATION": "redis://127.0.0.1:6379/",

   }
}
```


**step 5.** List of information that you need to collect from thrid party sources. Specify it in **Settings.py**.

``` Javascript
SOCIALACCOUNT_PROVIDERS = \
   {'facebook':
        {'METHOD': 'oauth2',
         'SCOPE': ['email', 'public_profile', 'user_friends'],
         'AUTH_PARAMS': {'auth_type': 'reauthenticate'},
         'FIELDS': [
             'id',
             'email',
             'name',
             'first_name',
             'last_name',
             'locale',
             'timezone',
             'link',
             'gender',
             'updated_time'],
         'EXCHANGE_TOKEN': True,
         'LOCALE_FUNC': lambda request: 'en_US',
         'VERIFIED_EMAIL': False,
         'VERSION': 'v2.4'},

    'linkedin':
        {'SCOPE': ['r_emailaddress'],
         'PROFILE_FIELDS': ['id',
                            'first-name',
                            'last-name',
                            'email-address',
                            'picture-url',
                            'public-profile-url']},

    'google':
        {'SCOPE': ['profile', 'email'],
         'AUTH_PARAMS': {'access_type': 'online'}}

    }

```

**step 6:** Since I don't have a mail server, which the frameworks uses for verification mails. I will be disabling it

```
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
```


**step 7: ** Now its time to add url's.

```
urlpatterns = [
   url(r'^admin/', admin.site.urls),
   url(r'^$','Authentication.views.home'),
   url(r'^accounts/', include('allauth.urls')),
   url(r'^landing/', 'Authentication.views.landing')
]


```
Define specified views for the urls. **/landing** is the redirect url which will redirect to the landing page specified.




**step 8.** Change **site_id** variable in **settings.py** by checking value from your **django_site** table. If this
value is not configured properly it will matched with other site.

**step 9.** Currently I am using **sqlite3** db, Please configure it accordingly in to your requirement(MySql...).


**Step 10.**

##### You need to configure with third party oauth providers like facebook, gmail, Linkedin.

1.  For integration with **Facebook**, Please refer [here](https://developers.facebook.com/apps/). Create a client id and
secret key and then configure it with Django admin by considering the **site_id** and **host site**.

2.  For **Linkedin**, Please refer [here](https://www.linkedin.com/secure/developer). Create the details as done above and
configure it with Djano adin.

3. For **Gmail**, Please refer [here](https://www.linkedin.com/secure/developer). Collect the details and configuring with
admin of Django. while giving redirect uri for Gmail application configuration, Please give **site_name/accounts/google/login/callback/**.
If redirect uri is not given, Django is unable to collect the details.



**step11.** Now you need to configure the details you got in the above step with **Django admin**. Because Django
framework should know on which site third party oauth should be performed.


you need to be super user if you want do changes to Django admin. Some versions of Django won't create the super
user. please use the below command for that
```
python manage.py createsuperuser
enter username, password, email id

```
**step 12:** Now go to the **url http://localhost:8000/admin/**

enter the credentials
username and password

choose Social Accounts → Social Applications → Add Social Applicaton

Now give Cliend id and secret key collected from facebook, Gmail, Linkedin and save the credentials


**step 13: ** Now you can hit **localhost:8000/account/login** to login.

