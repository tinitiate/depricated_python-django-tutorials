---
YamlDesc: CONTENT-ARTICLE
Title: python django restful web services tutorials
MetaDescription: python django restful web services tutorials
MetaKeywords: python, django, restful, web services, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-rest-webservices
---

# Django Restful Web Services
* **RESTful** follows simple design principles, All transactions must be 
  independent, Dont have dependencies among requests between client 
  and server, in the same session.
* **STATELESS**
* All `TRASACTIONS Must be stateless`, Meaning the server treats each request
   as an independent transaction that is unrelated to any previous request.
* **Have a URI Directory structure**
* Make the URI Directory structure visible to the client
* This means the access to a resource or service, must be in the format of URL:
  `[PROTOCOL]://[HOSTNAME]/[DIRECTORY-PATH]/[FILE-NAME]`
  Example 1. `[HTTP]://[TINITIATE.COM]/[COURSES]/[GET-COURSE-LIST.HTML]`
  Example 2. `[FTP]://[TINITIATE.COM]/[COURSES]/[GET-COURSE-LIST.HTML]`
* **Use HTTP Methods: PUT, GET, POST and DELETE**
* RESTful Communication between Client and Server happen through HTTP
  methods; PUT, GET, POST and DELETE.
* GET: `Read data or resource from server`
* PUT: `Creates Resource on server`
* DELETE: `Removes Resource from server`
* POST: `Updates or Changes or Adds data to server`
* **Supported Responses Formats Plain Text,XML and JSON**
* RESTful Responses are given in Plain Text, XML and JSON formats
* **Python Django Rest Framework** is the module that creates REST webservices
  * Install the djangorestframework module, `pip install djangorestframework`


## Demonstration of Django RESTful Web Service
* In this project we demonstrate creating a Web Service with a table
  * This will enable the client to perform CRUD operation on the SQLlite DB 
    table Created
  * Insert data, Select all/one data rows, update data, delete data


### STEP 1. Create Django APP To Demonstrate Django RestFul webservices
* Navigate to the Project Folder `tinitiate` and in the path that has the 
  `manage.py` file.
* Create an **APP** named `app_webservices`
* Using the following command.
```
python manage.py startapp app_webservices
```

### STEP 2. Change App Settings for Project
* **PROJECTs settings.py**
  * Locate the **PROJECTs** `settings.py` file, In this case its located at
    `tinitiate/tinitiate/settings.py` file.
  * In the `settings.py` file add the app name **app_webservices** to the 
    **INSTALLED_APPS**  list.
  * In the `settings.py` file add the template folder location of the 
    APP **app_webservices** to the **TEMPLATES/DIRS** section of the file.
* **PROJECTs urls.py**
  * Locate the **PROJECTs** folder and in the `urls.py` file, And add the 
    App Name **app_webservices** with the following

### STEP 3. Create Model (APP/models.py file) to represent the DB Table
* The model is the representation of the DB table, here we create a table from 
  the `models.py` file using the migration commands.
```
from django.db import models

class Projects(models.Model):
    # Primary Key
    proj_id = models.IntegerField(primary_key=True)
    # Member Columns
    proj_name = models.CharField(max_length=128,default=None, blank=True, null=True)
    proj_date = models.DateTimeField(default=None, blank=True, null=True)
    proj_cost = models.DecimalField(max_digits=20,decimal_places=4,default=None, blank=True, null=True)
```

### STEP 4. Create Model and Migrate Changes to SQLite DB
* This command is used to create the table from specifications in the 
  models.py file.
```
python manage.py migrate
```

### STEP 5. Create Serializer (APP/serializers.py file) for the Model
* Serializers allow complex data such as querysets and model instances to be 
  converted to native Python datatypes that can then be easily rendered into 
  JSON, XML or other content types. 
* Serializers also provide deserialization, allowing parsed data to be 
  converted back into complex types, after first validating the incoming data.
* This is a One-to-one mapping to the Model
```
from rest_framework import serializers
from .models import Projects

class ProjectsSerializer(serializers.ModelSerializer):

    class Meta:
        model = Projects
        fields = ('proj_id', 'proj_name', 'proj_date', 'proj_cost')
```

### STEP 6. Create The View (APP/views.py file)
* The `views.py` is the file that maps form the URLs endpoints to the exact
  python function
```
from django.shortcuts import render
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from django.http import HttpResponse 
from app_webservices.models import Projects
from app_webservices.serializers import ProjectsSerializer 


@api_view(['GET', 'POST'])
def ws_test(request):

    if request.method == "GET":
        return Response({"data": "GET Example"}, status=status.HTTP_200_OK)

    elif request.method == "POST":
        return Response({"data": "POST Example."}, status=status.HTTP_200_OK)

    else:
        data = {"Error": {"status": 400,
                          "message": "Request Error"}}

        return Response(data, status=status.HTTP_400_BAD_REQUEST)


@api_view(['GET', 'POST'])
def all_projects(request):

    """
    Request Type: GET
    =================
    1. List All Projects rows
    2. Request URL: localhost:800/app_webservices/all_projects
    
    Request Type: POST
    ==================
    1. Insert ONE Project row
    2. Request URL: localhost:800/app_webservices/all_projects
    3. Sample Data:
    {"proj_id":1,"proj_name":"Data Base","proj_date":"2016-05-18T03:02:00.776594Z","proj_cost":10000.22}
    """

    if request.method == 'GET':
        projects = Projects.objects.all()
        serializer = ProjectsSerializer(projects,context={'request': request} ,many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = ProjectsSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)

        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)


@api_view(['GET', 'PUT', 'DELETE'])
def single_project(request, in_proj_id):
    
    """
    Request Type: GET
    =================
    1. List All Projects rows
    2. Request URL: localhost:800/app_webservices/single_project/1
    
    Request Type: PUT
    =================
    1. Insert ONE Project row
    2. Request URL: localhost:800/app_webservices/single_project/1
    3. Sample PUT:
    {"proj_name":"Data Base and Python"}
    
    Request Type: DELETE
    ====================
    1. Insert ONE Project row
    2. Request URL: localhost:800/app_webservices/single_project/1
    """

    try:
        project = Projects.objects.get(proj_id=in_proj_id)
    except Projects.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'GET':
        serializer = ProjectsSerializer(project,context={'request': request})
        return Response(serializer.data)

    elif request.method == 'PUT':
        serializer = ProjectsSerializer(project, data=request.data,context={'request': request})
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        project.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

### STEP 7. Create APP the URLs (APP/urls.py file)
* This is the APP level `urls.py` here we map the URL endpoint to the 
  `views.py` function.
```
from django.urls import path
from . import views

urlpatterns = [
    path('ws_test/', views.ws_test, name='ws_test'),
    path('all_projects/', views.all_projects, name='all_projects'),
    path('single_project/<int:in_proj_id>', views.single_project, name='single_project'),
]
```

### STEP 8. Run Project and Test URLS in Browser
* Navigate to the project root folder and locate the `manage.py` file
  and run the project using the command
```
python manage.py runserver
```
* **TESTING**
* Using a Web Services testing tool such as PostMan, CURL, test with 
  the following
