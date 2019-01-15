---
YamlDesc: CONTENT-ARTICLE
Title: python django apps
MetaDescription: python django apps, views example code, tutorials
MetaKeywords: python django apps, views example code, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-apps-views
---

# Django Views
* The VIEW is the webpage itself ina Django project, Its a Web response 
  Retun object of a python function 
* The web response (HttpResponse Oject) renders to the HTML.
* Below is a demonstration of creating differnent types of views.


## STEP 1. Create Django APP To Demonstrate Views
* Navigate to the Project Folder `tinitiate` and in the path that has the 
  `manage.py` file.
* Create an **APP** named `app_views`
* Using the following command.
```
python manage.py startapp app_views
```


## STEP 2. Check APP Folder and Files
* Locate the **PROJECTs** `settings.py` file, In this case its located in the 
  `tinitiate/tinitiate/settings.py` file, Append the **APP** names to the 
  list **INSTALLED_APPS**  we created in **STEP 3**
![django app folder structure](django-app-folder-structure.png "django app folder structure")


## STEP 3. Add content to the views.py file
* Django supports the **Model View Template (MVT)** pattern of web pages
  * The MVT is defined as the **Model** The data (Usually from a DataBase or
    User Input) that needs to be displayed in the web page.
* Here we demonstrate 
  * View with no parameter
  * View with Integer parameter
  * View with String parameter
  * View to determine the parameter as EVEN or ODD number
```
from django.shortcuts import render
from django.http import HttpResponse

def view_no_param(request):
    return HttpResponse("Welcome to Tinitiate Django Test Home Page Version 1.0")

def view_integer_param(request, in_data):
    response = "Integer Data Provided: " + str(in_data)
    return HttpResponse(response)

def view_string_param(request, in_data):
    response = "String Data Provided: " + in_data
    return HttpResponse(response)

def even_or_odd(request, in_number):
    
    s_data = str(in_number)

    if (in_number%2) == 0:
        response = s_data + " is a even number"
    else:
        response = s_data + " is a odd number"

    return HttpResponse(response)
```


## STEP 4. APP folder urls.py Configuration
* Create an new file `urls.py` in the APP folder
* ![django app create urls](django-app-create-urls-py.png "django app create urls")
* Add the following code
```
from django.urls import path
from . import views

urlpatterns = [
    # For URL: localhost:8000/view_no_param and view function: view_no_param
    path('view_no_param/', views.view_no_param, name='view_no_param'),

    # For URL: localhost:8000/view_integer_param and view function: view_integer_param
    # Usage Example URL: localhost:8000/view_integer_param/1
    path('view_integer_param/<int:in_data>', views.view_integer_param, name='view_integer_param'),

    # For URL: localhost:8000/view_string_param and view function: view_string_param
    # Usage Example URL: localhost:8000/view_string_param/Hello
    path('view_string_param/<str:in_data>', views.view_string_param, name='view_string_param'),
    
    # For URL: localhost:8000/even_or_odd and view function: even_or_odd
    # Usage Example URL: localhost:8000/even_or_odd/1
    path('even_or_odd/<int:in_number>', views.even_or_odd, name='even_or_odd'),
]
```


## STEP 5. PROJECT folder urls.py Configuration
* Create an empty file `urls.py`
* Add the following code
```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('app_views/', include('app_view.urls')),
    path('app_ti/', include('app_ti.urls')),
    path('admin/', admin.site.urls),
]
```

### Understanding the routing views.py and urls.py
* In the `views.py` we created functions to handle different URLs
* The `return HttpResponse` in the functions accepts a string and returns 
  HTML, which is browser readable.
* In the `urls.py` the URL pattern is associated to the view.<function-name>


## STEP 6. Run Project and Test URLS in Browser
* At commandline start the project, using the command:
```
python manage.py runserver
```
* Open a browser to test the URLs defined so far
 * localhost:8000/app_views/view_no_param
 * localhost:8000/app_views/view_integer_param/1
 * localhost:8000/app_views/view_string_param/hello
 * localhost:8000/app_views/even_or_odd/4
 * localhost:8000/app_views/even_or_odd/5
