# Django to-do app
## 📑 Overview
This project create simple to-do app using django python

Interface of the to-do app:

![alt text](https://github.com/TrietTran1701/Django-To-do/blob/main/img/Untitled.png)


## 🔎 Project details
### Step by step implementation

#### Step 1: Setup environment
1. Create new django project, open cmd and cd to folder
2. Type django-admin startproject my_todolist
3. To start project as a local host site, x runserver   |   to make migrations, x migrate
4. Create an admin user with x createsuperuser
5. Make application called tasks x startapp tasks
6. In my_todolist/settings.py, add 'tasks', under INSTALLED_APPS


#### Step 2: Creating Url Route
1. Make test function in tasks/views.py:
```
from django.http import HttpResponse
def index(request):
	return HttpResponse('Hello world')
```
2. Create urls.py file under tasks directory, add:
```
from django.urls import path
from . import views
```
3. Have first path in urls.py call the views.index (which is the function index from views.py) when homepage is called (denoted with ' '):
```
urlpatterns = [path(' ', views.index)]
```
4. Let base url file under my_todolist/urls.py know about this path, add:

5. Under urlpatterns list, add:
```
path(' ', include('tasks.urls'))
```

#### Step 3: Creating Model (Task)
1. Under tasks/models.py, code in:
```
class Task(models.Model):
	title = models.CharField(max_length=200)
	complete = models.BooleanField(default=False)
	created = models.DateTimeField(auto_now_add=True)

	def __str__(self):
		return self.title
```
2. Prepare the database for migration with: 
```
 makemigrations
 ```
3. Migrate the model Task into database as a database table
4. Register to admin interface with model in tasks/admin.py:
```
from .models import *
admin.site.register(Task)
```

#### Step 4: Creating Model (Form)
1. Create forms.py under 'tasks' directory, this is where all forms classes are stored
2. Import forms.py into tasks/views.py:
```
from .forms import *
Under views.index function:
form = TaskForm()
```
Pass form model into template by adding a key: 'form': form under ‘context’ dictionary
3. Pass in template tag under list.html file and set up input field:
(Before for loop but after header, no indents)
```
<form>
	{{ form }}
	<input type="submit" name="Create Task">
</form>
```
3. Delete checkbox field by specifying which field to access.

#### Step 5: Creating item:
1. Set form method, whenever form is submitted, to be sent back to views.index function under views.py. Under list.html, change <form> to <form method="POST" action="/"> #action is base url path
2. Under views.py, add:
```
from django.shortcuts import redirect
```
3. Under views.index function, add (between TaskForm() and context init):
	```
    if request.method == 'POST':
		form = TaskForm(request.POST) #pass post method
		if form.is_valid():
			form.save()	#saves to the database
		return redirect('/') #send back to same url path
    ```
4. Add CSRF Token
5. Runserver and try it out! user typed string is submitted to same view as POST method, then that method puts it into the form and saves it
