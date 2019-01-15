---
YamlDesc: CONTENT-ARTICLE
Title: python django templates
MetaDescription: python django templates, template inheritance, code blocks, if else, loops, example code, tutorials
MetaKeywords: python django templates, template inheritance, code blocks, if else, loops, example code, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-template-inheritance
---

# Django Template Inheritance
* Template Inheritance is a concept where Templates can be divided into 
  multiple files, Where **Template Blocks** or template functions, those can be 
  called multiple times are placed in one file and the base HTML in another.
* Below are the steps to demonstrate the Template Inheritance
* **FOLDER PATH REFERENCE**
* TINITIATE
*     -  /tinitiate
*        - /__pycache__
*        - __init__.py
*        - **settings.py**
*        - **urls.py**
*        - wsgi.py       
*     -   manage.py
*
*     -  /rootpages
*        - /__pycache__
*        - /migrations
*        - __init__.py
*        - admin.py
*        - apps.py
*        - models.py
*        - tests.py
*        - **urls.py**
*        - views.py
*     -  /template_test
*        - /__pycache__
*        - /migrations
*        - /templates
*          - simple-template.html
*          - template-blocks.html
*          - blocks-caller.html
*        - __init__.py
*        - admin.py
*        - apps.py
*        - models.py
*        - tests.py
*        - **urls.py**
*        - views.py


## STEP 1. Django HTML Templates Inheritance
* **Base HTML Template**, Place this in the Templates Folder
* This is the File that Makes Calls to the Template File
```
<!-- File: blocks-caller.html -->
<html>
  <body>
    {% block blue_block %} {% endblock %}
    <br><br>
    {% block green_block %}{% endblock %}
    <br><br>
    {% block red_block %} {% endblock %}
  </body>
</html>
```
* **Blocks Caller**, Place this in the Templates Folder 
* This is the Template File that has the **Blocks**, and is inherited
```
{% extends 'blocks-caller.html' %}

{% block blue_block %}
<p style="background-color:blue; color:white;">{{ l_blue_data }}</p>
{% endblock %}

{% block green_block %}
<p style="background-color:green; color:white;">{{ l_green_data }}</p>
{% endblock %}

{% block red_block %}
<p style="background-color:red; color:white;">{{ l_red_data }}</p>
{% endblock %}
```


### STEP 2. Edit the views.py of the App template_test
* Add the **Function** to handle the Template
```
from django.shortcuts import render
from django.template import loader
from django.http import HttpResponse


# Template Blocks
def template_blocks(request):

    # Create Template Object
    template = loader.get_template('template-blocks.html')

    # Data to be passed to the template
    context = {
        'l_blue_data': 'This is BLUE Data',
        'l_green_data': 'This is GREEN Data',
        'l_red_data': 'This is RED data',
    }

    return HttpResponse(template.render(context, request))
# END

# Python Template
def simple_template_python(request):

    # Create Template Object
    template = loader.get_template('simple-template.html')


    # Data to be passed to template
    context = {
        'course_list': ['basic','advanced','web development'],
        'l_title': 'Welcome to Python Training',
        'course_name':'PYTHON',
    }

    return HttpResponse(template.render(context, request))
# END

    
# Java Template
def simple_template_java(request):

    # Create Template Object
    template = loader.get_template('simple-template.html')


    # Data to be passed to template
    context = {
        'course_list': ['core','advanced','spring'],
        'l_title': 'Welcome to Java Training',
        'course_name':'JAVA',
    }

    return HttpResponse(template.render(context, request))
# END
```


### STEP 3. Edit the urls.py of project and urls.py of the APP
* Add the APP details to the projects `urls.py`, See "tt" path
```
# File: Projects urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('rootpages.urls')),
    path('tt/', include('template_test.urls')),
    path('admin/', admin.site.urls),
]
```

* Edit the APPs `urls.py`
```
from django.urls import path
from . import views

urlpatterns = [
    # For URL: localhost:8000
    path('st_python', views.simple_template_python, name='st_python'),
    path('st_java', views.simple_template_java, name='st_java'),
    path('tblocks', views.template_blocks, name='tblocks'),
]
```


### STEP 5.Runserver and test Templates
* At commandline start the project, using the command:
```
python manage.py runserver
```
* Open a browser to test the URLs defined so far
 * localhost:8000/tt/template_blocks
