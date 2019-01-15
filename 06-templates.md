---
YamlDesc: CONTENT-ARTICLE
Title: python django templates
MetaDescription: python django templates, template inheritance, code blocks, if else, loops, example code, tutorials
MetaKeywords: python django templates, template inheritance, code blocks, if else, loops, example code, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-templates
---

# Django Templates
* Django supports Templates,
* A template is a text file, that can be structured around a specific HTML, 
  CSV, XML.. file,
* Templates support variables, code blocks[functions], conditional statements, 
  loops and filters (can be equated to builtin functions).
* Template is a convenient way to generate HTML dynamically. 
* Since most elements of a give set of releated HTML pages is common,
  template contains the static parts of the desired HTML, with substitutable 
  variables.

## Creating Django HTML Templates

### STEP 1. Edit Project level settings.py file
* Add Template Directory to `TEMPLATES` Dictionary in the projects `settings.py`
  `'DIRS': ['F:\\DjangoLabs\\training\\tinitiate\\template_test\\templates'],` 
* Add Static Files Directory, that will be used to host static libraries like 
  jQuery or Anguular or custom CSS and JS files
* ![django template settings](django-template-settings.png "django template settings")

### STEP 2. Create New App to Demonstrate Using HTML Templates
* 1. Create a new app using `python manage.py startapp template_test`
* 2. Create a HTML Template File `simple-template.html` and save it in the folder 
     mentioned in the settings.py `TEMPLATES` dictionary `DIRS` element
```
<!-- File: simple-template.html -->
<html>
  <head>
    <title>{{ l_title }}</title>
  </head>
  <body>
    {% if course_name == "PYTHON" %}
        <h1>{{ course_name }}</h1>
        <ul>
        {% for topic in course_list %}
            <li>{{ topic }}</li>
        {% endfor %}
        </ul>
    {% elif course_name == "JAVA" %}
        <h1>{{ course_name }}</h1>
        <ul>
        {% for topic in course_list %}
            <li>{{ topic }}</li>
        {% endfor %}
        </ul>
    {% else %}
        <h1>{{ course_name }}</h1>
        <ul>
        {% for topic in course_list %}
            <li>{{ topic }}</li>
        {% endfor %}
        </ul>
    {%endif%}
  </body>
</html>
```
* 3. The Template Language supports Loops and Conditional Statements, All 
     python Variables such as simple variables, tuples, dictionaries 
     are supported.

### STEP 3. Edit the `views.py` of the App `template_test`
```
# File: views.py of the New App "template_test"

from django.shortcuts import render
from django.template import loader
from django.http import HttpResponse

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
* Here we see the the `context` being passed as a Dictionary to the Template.
* The **Dictionary** elements will be used inside the Template directly, as 
  though the dictionary is part of the template file.

### STEP 4. Edit the urls.py of project and urls.py of the APP
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
 * localhost:8000/tt/st_java
 * localhost:8000/tt/st_python
