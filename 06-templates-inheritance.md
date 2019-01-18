---
YamlDesc: CONTENT-ARTICLE
Title: python django templates
MetaDescription: python django templates, template inheritance, code blocks, if else, loops, example code, tutorials
MetaKeywords: python django templates, template inheritance, code blocks, if else, loops, example code, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-template-inheritance
---

# Django Template Inheritance
* Template Inheritance is a concept where Templates can be divided into called
  multiple files, Where **Template Blocks** or template functions, those can be 
  multiple times are placed in one file and the base Template HTML in another.

## Demonstration for Django HTML Template Inheritance

## STEP 1. Create Django APP To Demonstrate Django HTML Templates
* Navigate to the Project Folder `tinitiate` and in the path that has the 
  `manage.py` file.
* Create an **APP** named `app_django_templates_inheritance`
* Using the following command.
```
python manage.py startapp app_django_templates_inheritance
```

### STEP 2. Edit Project level settings.py file for Template Data
* Add Template Directory details to the Projects `settings.py`
  `'DIRS': ['F:\\code\\tinitiate\\source\\python-django\\code\\tinitiate\\app_django_html_templates\\templates'],`
* Add Static Files Directory, that will be used to host static libraries like 
  jQuery or Angular or custom CSS and JS files
* ![python django template inheritance settings](python-django-template-inheritance-settings.png "python django template inheritance settings")

### STEP 3. Add APP to the Project Folder settings.py file
* Add APP to the Project Folder settings.py file
* Locate the **PROJECTs** `settings.py` file, In this case its located in the 
  `tinitiate/tinitiate/settings.py` file, Append the **APP NAME app_django_templates_inheritance**
  names to the list **INSTALLED_APPS**

## STEP 4. Create the Django HTML Templates for Inheritance
* Create file `blocks-caller.html` in the **Base HTML Template**, Place this 
  in the Templates Folder in the app folder
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
* Create file `template-blocks.html` **Blocks Caller**, Place this in the 
  Templates Folder 
* This is the Template File `template-blocks.html` that has the **Blocks**, 
  and is inherited
```
<!-- File: template-blocks.html -->
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

### STEP 5. Edit the views.py of the App template_test
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
```


### STEP 6. Edit the Projects urls.py and App urls.py
* Add the APP details to the **projects** urls.py:
* `path('app_django_templates_inheritance/', include('app_django_templates_inheritance.urls')),`
```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('app_django_templates_inheritance/', include('app_django_templates_inheritance.urls')),
    path('app_django_html_templates/', include('app_django_html_templates.urls')),
    path('multiple_views/', include('app_multiple_view_files.urls')),
    path('app_views/', include('app_views.urls')),
    path('app_ti/', include('app_ti.urls')),
    path('admin/', admin.site.urls),
]
```

* Add **APPs** urls.py with the following contents
```
from django.urls import path
from . import views

urlpatterns = [
    # For URL: localhost:8000
    path('ti_blocks', views.template_blocks, name='template_blocks'),
]
```
>
* **Folder Structure For Templates Inheritance**
  * `PROJECT`
  * /tinitiate
    * /__pycache__
    * __init__.py
    * **settings.py**
    * **urls.py**
    * wsgi.py       
  * manage.py
  * `APP`
  * /app_django_templates_inheritance
    * /__pycache__
    * /migrations
    * /templates
      * template-blocks.html
      * blocks-caller.html
    * __init__.py
    * admin.py
    * apps.py
    * models.py
    * tests.py
    * **urls.py**
    * **views.py**
>

### STEP 7.Runserver and test Templates
* At commandline start the project, using the command:
```
python manage.py runserver
```
* Open a browser to test the URLs defined so far
* localhost:8000/app_django_templates_inheritance/ti_blocks
