# django
Django tutorial projects

# ----- Article Database -----
# We'll use the Django Object Relational Mapper
# which can be used to work with multiple DBs.
# 1. Setup the site
mkdir tut3
cd tut3
pipenv install django==3.0
pipenv shell
django-admin startproject project_3 .
python manage.py startapp articles
# 2. Tell Django about our posts app in settings
INSTALLED_APPS = [
 'django.contrib.admin',
 'django.contrib.auth',
 'django.contrib.contenttypes',
 'django.contrib.sessions',
 'django.contrib.messages',
 'django.contrib.staticfiles',
 'articles.apps.ArticlesConfig',
]
# 3. Create a SQLite3 DB with migrate
# As the DB model is changed we run migrate to
# update our project
python manage.py migrate
# 4. Create a DB model that stores posts. Django
# turns the model into DB tables. The models.py
# file is located in our app folder
from django.db import models
# Define the type of content you'll store being
# TextField
class Article(models.Model):
 # CharFields are used for small strings
 title = models.CharField(max_length=200)
 # TextFields are used for big strings
 text = models.TextField()
# 5. Activate our model by making a migrations file
# which tracks all changes and then we build the DB
# with the migrate command
Ctrl+C to stop server
python manage.py makemigrations articles
python manage.py migrate
# 6. Create a superuser for logging into Djangos
# admin interface
python manage.py createsuperuser
# 7. Start server and open admin in the browser
python manage.py runserver
http://127.0.0.1:8000/admin
# 8. Update posts/admin.py for our app to display in
# the admin interface
from django.contrib import admin
# Tell Django to display our articles app and the DB
# model on the admin interface
from .models import Article
admin.site.register(Article)
# 9. Now you can enter articles in the browser.
from django.db import models
class Article(models.Model):
 title = models.CharField(max_length=200)
 text = models.TextField()
# 10. Now will display our articles on a webpage by
# creating a homepage view in views.py
# Used to generate a list of objects
from django.views.generic import ListView
# Import the Article model from models.py
from .models import Article
# Define our homepage class which is a subclass
# of ListView, assign the model and then the URL
# template
class HomePageView(ListView):
 model = Article
 template_name = 'home.html'
 # Define the name used to refer to the list
 # of articles
 context_object_name = 'all_articles_list'

# 11. Create the folder articles/templates with
# mkdir templates and add home.html to the
# templates folder
# Refer to all_articles_list and as we
# cycle through the articles we grab them using text
# which is set up in the model class
<h1>Articles</h1>
{% for article in all_articles_list %}
 <h3>{{ article.title }}</h3>
 <p>{{ article.text }}</p>
{% endfor %}
# 12. Update mb_project/settings.py to point to the
# templates folder
TEMPLATES = [
 {
 'BACKEND': 'django.template.backends.django.DjangoTemplates',
 'DIRS': [os.path.join(BASE_DIR, 'templates')],
# 13. Add the URL to our homepage to urls.py
from django.contrib import admin
from django.urls import path, include
urlpatterns = [
 path('admin/', admin.site.urls),
 path('', include('articles.urls')),
]
# 14. Define articles/urls.py
from django.urls import path
from .views import HomePageView
urlpatterns = [
path('', HomePageView.as_view(), name='home')
]
# 15. Run the server and verify results at http://127.0.0.1:8000/
python manage.py runserver
# Articles
Don't Mess with a Drop Bear
Drop bears are commonly said to be unusually large, vicious, carnivorous marsupials related to
koalas that inhabit treetops and attack their prey by dropping onto their heads from above.
Koala drop bears are about the size of a very large dog, have coarse orange fur with dark
mottling, have powerful forearms for climbing and attacking prey, and bite using broad
powerful premolars rather than canines. They weigh 120 kilograms (260 lb) and have a length
of 130 centimetres (51 in).
Jackalopes are Real Frightening
There are two versions of the jackalope. The first is taxidermy version created by Douglas
Herrick and his brother. It is created by grafting deer antlers onto a jackrabbit carcass. Stuffed
and mounted, jackalopes are found in many bars and other places in the United States.
The second version, upon which the Wyoming taxidermists were building, is very frightening
indeed! The cute Jackalope was inspired by sightings of rabbits infected with the Shope
papilloma virus.
Shope papilloma virus infects rabbits, causing keratinous carcinomas, typically on or near the
animal’s head. These tumors can become large enough that they interfere with the host’s ability
to eat, eventually causing starvation
