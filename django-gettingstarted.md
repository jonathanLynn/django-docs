# Getting Started with Django

This guide will walk you through the process of creating a new Django project, creating an app within the project, and setting up the URLs for the app.

## Creating a Django Project

First, let's create a new Django project. Open your terminal and run the following command:

`python -m django startproject myproject`

## Creating an App

Next, let's create a new app within our project. Navigate to your project directory and run the following command:

```
cd myproject
python manage.py startapp myapp
```

This will create a new app named myapp.

## Updating settings.py

After creating the app, we need to add it to the INSTALLED_APPS list in our project's settings.py file. Open settings.py and add the following line to INSTALLED_APPS:

'myapp',

## Creating a urls.py File in the App Directory

Next, let's create a urls.py file in our app directory. This file will contain the URLs for our app. In the myapp directory, create a new file named urls.py and add the following code:

```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

In this code, we've created a URL pattern for the home page of our app. The views.home argument is the view function that will be called when a user navigates to the home page.

## Updating the Project urls.py File

Finally, let's update our project's urls.py file to include the URLs from our app. Open the urls.py file in the myproject directory and add the following code:

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('myapp/', include('myapp.urls')),
]
```

In this code, we've added a new URL pattern that includes the URLs from our app. The include('myapp.urls') function tells Django to look for URL patterns in myapp/urls.py whenever a user navigates to a URL that starts with myapp/.

That's it! You've now created a Django project, an app, and set up the URLs for your app.