# Deploy a Django App to Heroku
Text version of tutorial from [Pretty Printed - Deploy a Django App to Heroku](https://youtu.be/GMbVzl_aLxM)

## What you need to have
1. Heroku Account
2. Heroku command line client
3. Private git repo
## Deploy your Django project
### Login Heroku
Type in the command line
```
heroku login
```
wait for the browser windows pop up and login. 
### Create a repo for heroku account
Go inside the directory with manage.py file and init repo
```
git init
```
Add everything to the repo with
```
git add .
git commit -m "first commit"
```
### Create a heroku app
```
heroku create "<your app name>"
```
Also you can you exacly the same on heroku.com\
Add the remote for you app with
```
heroku git:remote -a <your app name>
```
### Install Gunicorn
The default django server is only for development. You can't use that when you deploy it. App server we gonna use is Gunicorn. For the beginning install it on you local machine using
```
pip install gunicorn
```
After installing run your gunicorn server
```
gunicorn <your django project name>.wsgi
```
Follow the server link and make sure the server and the app work correctly
### Set up heroku Procfile and run heroku local
Create a new file in your repo root
```
touch Procfile
```
Procfile should contain the following command
```
web: gunicorn <your django project name>.wsgi
```
Run the heroku local server
```
heroku local
```
Make sure everything works on your local machine before deploying your project on heroku
### Create requirements.txt file
```
pip freeze > requirements.txt
```
### Commit and push evetything to heroku
```
git add .
git commit -m "added procfile and requirements"
git push heroku master
```
### Set up static directory in your Django project

Go to settings.py file and add after ```STATIC_URL = ...``` following:

```python
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```
Commit and push everything again
```
git add .
git commit -m "added static root"
git push heroku master
```
### Add url and local host in ALLOWED_HOSTS
Edit ALLOWED_HOSTS list in settings.py file
```python
ALLOWED_HOSTS = ['<heroku app url>','127.0.0.1']
```

```
git add .
git commit -m "added allowed hosts"
git push heroku master
```
So now you gonna see your django app working on heroku

## Using heroku Postgres database

### Install the library interface
```
pip install psycopg2
pip freeze > requirements.txt
```
### Modify the configuration for the database
Go to your Heroku profile -> your heroku app -> Settings -> Config Vars -> Reveal Config Vars. Copy DATABASE_URL\
It has the following structure: `postgres://username:password@host:port/db_name`\
Add then corresponding values in settings.py
```python
DATABASES = {
    'default' : {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'db_name',
        'HOST': 'host',
        'PORT': 'port',
        'USER': 'username',

    }
}
```

 Migrate database
```
python manage.py migrate
```
```
git add .
git commit -m "added postgre database"
git push heroku master
```
## Handle static files
To handle static files we gonna use whitenoise library
```
pip install whitenoise
pip freeze > requirements.txt
```
Go to MIDDLEWARE list in your settings.py file and add
```python
MIDDLEWARE = [
    ...
    'whitenoise.middleware.WhiteNoiseMiddleware'
]
```
```
git add .
git commit -m "added whitenoise middleware"
git push heroku master
```

Source: [Pretty Printed - Deploy a Django App to Heroku](https://youtu.be/GMbVzl_aLxM)
