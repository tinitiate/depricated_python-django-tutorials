---
YamlDesc: CONTENT-ARTICLE
Title: python django apps urls
MetaDescription: python django apps urls, url patterns example code, tutorials
MetaKeywords: python django apps urls, , url patterns example code, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-apps-urls
---


# Django APPs
* In Django every **PROJECT** is made up of **APPs**,  They are the independent 
  control mechanisms to handle pages and sub folders of the web project.

  
## STEP 1. Create Django APP
* Navigate to the Project Folder and in the path that has the `manage.py` file
* Create an **APP** named `rootpages`, which will manage the `Root Level Pages`
* Using the following command.
```
python manage.py startapp rootpages
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
    * **VIEWS**
    * The VIEW is the webpage itself, Here the HttpResponse renders to the HTML.
    * Here we demonstrate A simple view creates a Text Response, `homepage`
    * Also a demonstration of a VIEW with parameter
```
from django.http import HttpResponse

def homepage(request):
    return HttpResponse("Welcome to Tinitiate Django Test Home Page Version 1.0")

def param_test(request, in_data):
    response = "Data Provided: " + str(in_data)
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
    # For URL: localhost:8000 and view function: homepage
    path('', views.homepage, name='homepage'),
    # For URL: localhost:8000/param_test and view function: param_test
    # Usage Example URL: localhost:8000/param_test/1
    path('param_test/<int:in_data>', views.param_test, name='param_test'),
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
    path('', include('rootpages.urls')),
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
 * localhost:8000
 * localhost:8000/param_test/1
 * localhost:8000/even_or_odd/4
 * localhost:8000/even_or_odd/5
