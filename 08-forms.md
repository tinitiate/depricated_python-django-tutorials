---
YamlDesc: CONTENT-ARTICLE
Title: python django forms tutorials
MetaDescription: python django forms tutorials, example code, tutorials
MetaKeywords: python django forms tutorials, example code, tutorials
Author: Venkata Bhattaram / tinitiate.com
ContentName: django-forms
---

# Django Forms
* Forms are integral part of any web application, they are the controls that 
  accept inputs from the UI and even shows the responses back to the UI.
* HTML Forms work with GET and POST HTTP requests. The FORM `values` are passed
  to the Django VIEW through POST/GET request, they are passed as a Dictionary.

## Demonstration of Django Forms

### STEP 1. Create Django APP To Demonstrate Django Forms
* Navigate to the Project Folder `tinitiate` and in the path that has the 
  `manage.py` file.
* Create an **APP** named `app_forms`
* Using the following command.
```
python manage.py startapp app_forms
```

### STEP 2. Change App Settings for Project
* **PROJECTs settings.py**
  * Locate the **PROJECTs** `settings.py` file, In this case its located at
    `tinitiate/tinitiate/settings.py` file.
  * In the `settings.py` file add the app name **app_forms** to the 
    **INSTALLED_APPS**  list.
  * In the `settings.py` file add the template folder location of the 
    APP **app_forms** to the **TEMPLATES/DIRS** section of the file.
* **PROJECTs urls.py**
  * Locate the **PROJECTs** folder and in the `urls.py` file, And add the 
    App Name **app_forms** with the following

    
### STEP 3. Create forms.py file in the APP folder
* The `forms.py` file has the form data, Each form usually is its function,
* Here we demonstrate Python Django Forms with HTML form elements, TextBox, 
  Radio button, Check Box, Drop Down Box.
```
from django import forms

class TestForm(forms.Form):
    # HTMl INPUT Box
    userid = forms.CharField()
    passwd = forms.CharField()

    form_choices = (
        (1, 'Food'),
        (2, 'Drinks'),
        (3, 'Movies'),
        (4, 'Rest'),
        (5, 'Sports')
    )

    # HTML Drop Down Box
    DropDownBox = forms.ChoiceField(choices=form_choices)

    # HTML Radio Buttons
    RadioButtons = forms.ChoiceField(choices=form_choices, widget=forms.RadioSelect)

    # HTML Multi Select Text Box
    MultiSelect = forms.MultipleChoiceField(choices=form_choices)

    # HTML Check Box
    CheckBox = forms.MultipleChoiceField(choices=form_choices, widget=forms.CheckboxSelectMultiple)
```

### STEP 4. HTML Template for FORM rendering `post_inputs.html` file
* This is the `post_inputs.html` file that will be rendered, with the form data.
* This form will have provisions to input user data and choices.
```
<form method="post" action="">

    {% csrf_token %}

    <b>User name:</b><br>
    {{form.userid.as_text}}<br>
    <b>User password:</b><br>
    {{form.passwd.as_text}}<br><br><br>
    <hr>
    
    <b>Drop Down Box:</b><br>
    {{form.DropDownBox}}<br><br><br>
    <hr>

    <b>Radio Button:</b>
    {{form.RadioButtons}}<br><br>
    <hr>

    <b>Multi Select Box:</b><br><br>
    {{form.MultiSelect}}<br><br>
    <hr>

    <b>Checkbox:</b><br>
    {{form.CheckBox}}<br><br><br>

    <button type="submit">Submit</button>
</form>
```

### STEP 5. HTML Template for output.html file FORM Submit rendering after POST action
* This is the `output.html` file which will be rendered after the form submission.
```
<html>
    <body>
        <h1>Django Forms from POST method</h1>
        
        <h2>Text Boxes Data</h2>
        <p>User Name: {{ user_id }}</p>
        <p>Password: {{ pass_wd }}</p>
        <br><br>
        
        <h2>Drop Down Data</h2>
        <p>{{ DropDown_data }}</p>
        
        <br><br>
        <h2>Radio Option Data</h2>
        <p>{{ Radio_Selected }}</p>

        <br><br>
        <h2>Multi Select Text Box</h2>
        {% for choice in MultiList_Selected %}
            <p>{{ choice }}</p>
        {% endfor %}
        
        <br><br>
        <h2>Check Box</h2>
        {% for choice in CheckBox_Selected %}
            <p>{{ choice }}</p>
        {% endfor %}

    </body>
</html>
```

### STEP 6. Create views.py file in the APP folder
* Here we create a `views.py` file with various demonstrating reading the Form 
  data post validation after the POST action
```
from django.shortcuts import render, HttpResponse
from django.template import loader
from .forms import TestForm

def simple_form(request):
    
    if request.method == 'POST':

        # Create Template Object
        template = loader.get_template('output.html')
        testForm = TestForm(request.POST)

        if testForm.is_valid():

            # Get the Choice Position of the selected values from post
            drop_down_data = testForm.cleaned_data.get('DropDownBox')
            radio_data = testForm.cleaned_data['RadioButtons']
            multi_data_list = testForm.cleaned_data['MultiSelect']
            checkbox_data_list = testForm.cleaned_data['CheckBox']

            # Single Selected Choice
            dd_var = TestForm.form_choices[int(drop_down_data)-1][1]
            rd_var = TestForm.form_choices[int(radio_data)-1][1]

            # Multi Selected Choice
            md_list = []
            for i in multi_data_list:
                md_list.append(TestForm.form_choices[int(i)-1][1])

            # Check Box Selected Choice    
            cd_list = []
            for i in checkbox_data_list:
                cd_list.append(TestForm.form_choices[int(i)-1][1])
            
            # UserID and Password Text Box values
            userid_txt = testForm['userid'].value
            passwd_txt = testForm['passwd'].value
                
                
        # Data (context) to be passed to template
        context = {
            'user_id': userid_txt,
            'pass_wd': passwd_txt,
            'DropDown_data' : dd_var,
            'Radio_Selected' : rd_var,
            'MultiList_Selected' : md_list,
            'CheckBox_Selected' : cd_list,
        }

        # Use the "context" to render the HTML Template to display values
        return HttpResponse(template.render(context, request))

    else:
        form = TestForm()
        return render(request,'post_inputs.html',{'form':form})
```

### STEP 7. Run Project and Test URLS in Browser
* At commandline start the project, using the command:
```
python manage.py runserver
```
* **TESTING**
* Open a browser to test the URLs defined so far
  * For Home Page Use: `http://localhost:8000/app_forms/form_post/`
  * Fill form data and press submit to see the data in the rendered output.html
