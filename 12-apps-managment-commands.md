---
YamlDesc: CONTENT-ARTICLE
Title: django custom management commands
MetaDescription: python django ,custom management commands code, tutorials
MetaKeywords: python django ,custom management commands code, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-custom-management-commands
---

# Django Custom Management Commands
* Django Custom Management Commands are applied through projects `manage.py`
  commands like startproject, runserver..
* Django management commands are designed as interfaces to execute simple to 
  complex tasks using the command `pytho manage.py at the terminal
* Management commands are useful to communicate with the Django application 
  with a command line terminal 
* They provide an interface to execute website cron jobs.

## STEP 1. Create Django APP To Demonstrate Sessions
* Navigate to the Project Folder `tinitiate` and in the path that has the 
  `manage.py` file.
* Create an **APP** named `app_management_commands`
* Using the following command.
```
python manage.py startapp app_management_commands
```

## STEP 2. Project Folder settings.py changes
* Locate the **PROJECTs** `settings.py` file, In this case its located in the 
  `tinitiate/tinitiate/settings.py` file, Append the **APP app_management_commands** name to the 
  list **INSTALLED_APPS**  we created in **STEP 1**
![django app folder structure](django-app-folder-structure.png "django app folder structure")

## STEP 3. Projects folder urls.py Configuration
* Create an new file `urls.py` in the APP folder
* ![python django app folder structure urls](python-django-app-folder-structure-urls.png "python django app folder structure urls")
* Add the following code
```
urlpatterns = [
    path('', include('app_home.urls')),
    path('app_urls/', include('app_urls.urls')),
    path('app_django_templates_inheritance/', include('app_django_templates_inheritance.urls')),
    path('app_django_html_templates/', include('app_django_html_templates.urls')),
    path('multiple_views/', include('app_multiple_view_files.urls')),
    path('app_views/', include('app_views.urls')),
    path('app_ti/', include('app_ti.urls')),
    path('app_response_types/', include('app_response_types.urls')),
    path('app_forms/', include('app_forms.urls')),
    path('app_webservices/', include('app_webservices.urls')),
    path('app_cookies/', include('app_cookies.urls')),
    path('app_sessions/', include('app_sessions.urls')),
    path('app_management_commands/', include('app_management_commands')),
    path('admin/', admin.site.urls),
]
```

## STEP 4. APP folder urls.py Configuration
* Create an new file `urls.py` in the APP folder
* ![python django app folder structure urls](python-django-app-folder-structure-urls.png "python django app folder structure urls")
* Add the following code
```
from django.urls import path
from . import views

urlpatterns = [
    path('test/', views.test, name='test'),
    path('limited_offer/', views.limited_offer, name='limited_offer'),
]
```

## STEP 5. Add content to the APPs folders views.py file
* The `views.py` demonstrates the following
  * A Test page
  * An offers page, Here the offer avaiability is determined by the setting 
    in the config file `app.config`
```
from django.shortcuts import render
from configparser import ConfigParser
from django.http import HttpResponse
from django.conf import settings

# Create your views here.
def test(request):
    return HttpResponse("Management Commands")


def limited_offer(request):
    
    # Path of the APP Config File
    l_config_path = settings.BASE_DIR + "\\app_management_commands\\" + "app.config"

    # Read the config file
    config = ConfigParser()
    data = open(l_config_path,'r')
    config.read_file(data)

    # Check the Offer status and render the page as per the offer settings
    offer_status = config.get("site", "offers")

    if str(offer_status) == "1":
        return HttpResponse("This is Limited Offer")
    else:
        return HttpResponse("No offers now")
```

## STEP 6. Add config file for APPs folders views.py file
* Here we use an `app.config` file to have the offer settings
  * 1: enable offers
  * 0: disable offers
* Below is the initial setting
* This is changed by the management command, NOT manually
```
[site]
offers = 1
```

## STEP 7. Create the Management Commands
* Create the management and management/commands folder in the APP folder
* Management Commands are APP specific, here we need to Create the `management`
  and `management/commands` folders
* In our app `app_management_commands`, we create `management` and 
  `management/commands` folders
* Below is the folder structure for the Management Commands  
  * tinitiate/                              <-- Django Project Folder
  *  |-- app_management_commands/           <-- Django App Folder
  *  |    |-- management/
  *  |    |    +-- commands/
  *  |    |         +-- current_time.py  <-- Print the current time
  *  |    |         +-- offer_control.py <-- Enable / Disable a page from the view
  *  |    |-- migrations/
  *  |    |    +-- __init__.py
  *  |    |-- __init__.py
  *  |    |-- admin.py
  *  |    |-- apps.py
  *  |    |-- tests.py
  *  |    +-- views.py
  *  |-- tinitiate/
  *  |    |-- __init__.py
  *  |    |-- settings.py
  *  |    |-- urls.py
  *  |    |-- wsgi.py
  *  +-- manage.py
>
* Add the first management-command `current_time.py`
* Management Command has a `Command Class` and the method `handle`
```
from django.core.management.base import BaseCommand
from django.utils import timezone
from django.conf import settings # correct way

class Command(BaseCommand):
    help = 'Displays current time'

    def handle(self, *args, **kwargs):
        time = timezone.now().strftime('%X')
        self.stdout.write("Current Time is %s" % time)
```
>
* Add the next management-command `offer_control.py`
* This Management Command has a `Command Class` and the method `handle` and 
  arguments that are passed to the handle.
* Here we update the app.config file based on the argument of the 
  management command, using the `ConfigParser`.
```
from django.core.management.base import BaseCommand
from django.utils import timezone
from django.conf import settings
from configparser import ConfigParser

class Command(BaseCommand):

    help = 'Use this to control Offer Link'

    # Create Arguments Handler
    def add_arguments(self, parser):
        parser.add_argument('-enable', action='store_true', help='Enables the Offers Link')
        parser.add_argument('-disable', action='store_true', help='Disables the Offers Link')

    # Handle the Custom Management Command
    def handle(self, *args, **kwargs):
    
        # Read arguments
        l_enable = kwargs['enable']
        l_disable = kwargs['disable']
        
        # Path of the APP Config File
        l_config_path = settings.BASE_DIR + "\\app_management_commands\\" + "app.config"
        
        # Read the config file
        config = ConfigParser()
        data = open(l_config_path,'r')
        config.read_file(data)

        # Based on the Argument of the Management Command 
        # update the config file.
        if l_enable:
            config["site"]["offers"] = "1"
        elif l_disable:
            config["site"]["offers"] = "0"

        with open(l_config_path, 'w') as configfile:
            config.write(configfile)
```
>


## STEP 8. Run Project and Test URLS in Browser
* At commandline start the project, using the command:
```
python manage.py runserver
```
* Run a test to see the APP working
* Login using the URL:  `http://localhost:8000/app_management_commands/test`
* Execute the Custom Management Commands at the command line in a seperate window
```
python manage.py current_time
```
* Offers page  `http://localhost:8000/app_management_commands/limited_offer` 
  will show offers present
* Execute the Custom Management Commands to disable the offer at the commandline.
```
python manage.py offer_control -disable
```
* Offers page  `http://localhost:8000/app_management_commands/limited_offer` 
  will show offers not available
* Enable the offers by running the following at the commandline
```
python manage.py offer_control -enable
```
* Offers page  `http://localhost:8000/app_management_commands/limited_offer`
  will again show offers.
