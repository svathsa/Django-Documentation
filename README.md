# Django-Documentation
<br/>
<h3> What is Django? </h3><br/>
Django is essentially a tool to help support your web applications. Django is a Python Web Framework that allows users to run local servers, set up a database, build APIs etc. using Python as the functioning programming language. The use of Python and the million other libraries it comes with, makes Django an attractive option for web development. <br/>

<h3> Creating and running a Django server </h3>
<ul>
<li> First, in the Command line, cd into the directory where you want to set up your server.</li>
<li> Go ahead and enter the following command: <br />
        
    django-admin startproject name_of_project
   <br/>
  This creates a project directory and initializes it with some files essential to run the server. 
</li>
<li>
  To run the server, go ahead and cd into the newly created project folder. Then  enter the following command <br/>
   
      python manage.py runserver
 
 <br />
       Go over to to localhost:8000 in the address bar of your browser to check if the server has been set up successsfully and is running.
 </li>
 </ul>

<h3>Creating and running Apps</h3>
<ul>
  <li> Any main part of a website/project is called an app. It is important to remember that each app must have a single function. So the functionality of an app must be explained by a single sentence.</li>
  <li> To create a new app:
  
     python manage.py startapp app_name
  </li>
  <li>The newly created folder named 'app_name' consists of the of the following built in files:
    <ul>
      <li> migrations: which is essentially a tool to hook-up your app to a database. </li>
      <li> __init__.py: tells django that the directory is a python package. </li>
      <li> admins.py: used to administer the database that comes built-in with django. The admin page can be used to create and modify users, create new objects, modify exeisting objects, etc.</li>
      <li> apps.py: mainly describes the settings of the app. </li>
      <li> models.py: could be thought of as a blueprint for your database. It helps in creating templates to organize the storage of data.</li>
      <li> views.py: are the functions in your database. Most of the time, they take in a web page and return a webpage. So views take a request from the user and return a response.</li>
    </ul>
  </li>
  <li>To allow django to recognize your newly created app, go over to settings.py and add the following to the INSTALLED_APPS array:<br>
         
    'app_name.apps.App_nameConfig'
   Here app_name denotes the name of your app.
  </li>
</ul>
<h3>Creating Views and Models</h3>
<ul>
  <li>Views are functions that take a request from the user and returns an http response. </li>
  <li>The urlspatterns array in urls.py file of your app is always looked up by django when a request ending with that string is made. The action taken is defined by what comes after the comma (usually the name of a view).</li>
  <li>urls.py: Each app consists of a urls.py file which describes the functionality in an array for various url paths. These urls.py files from all the apps is imported by the urls.py file of the mysite folder.</li>
  <li> '' describes the path to the home page of the app, views.index - specifies a particular view (i.e., a particular function in views.py) to be executed at the homepage of the app and name specifies the name of the view.</li>
  <li> To integrate the urls.py of our app we create another path in our main website’s urls.py and call it:
      
    path(‘appname/’, include(‘appname.urls’))
    </li>
   <li> In our views.py file we import:
        
    from django.http import HttpResponse
       
  to allow us to create a view that returns an http response.

  </li>
    <li>In our view, we could either return a http response or return an http redirect.
      <ul>
        <li>To return an http response: return HttpResponse(html) #html is a string that consists of some markup</li>
        <li>To redirect: return redirect(url)</li>
      </ul>
    </li>
    <li>Models are classes that are used to represent data in Django.</li>
    <li>The models.py file is used to describe the representation of data. Each class in models.py defines a table and each variable in that class defines a column.</li>
    <li>Every class created in models.py inherits from models.Model
      
    class class_name(models.Model)

   </li>
   <li>To create fields in your model we use:
      
        column_name = models.CharField(max_length=500) #we can also use IntegerField(), FloatField(), etc.
   </li>
   <li>To create a foreign key that links one model to another we use:
       
        models.ForeignKey(class_name, on_delete = models.CASCADE)
   </li>
   <li>To incorporate changes made to the models.py in the database associated with the django server, we type the following in the             terminal:
        
     python manage.py makemigrations music
     python manage.py migrate
  
   </li>
  </ul>
  <h3>Manipulating the Database:</h3>
  <ul>
    <li>Let us use the example of a music app that uses two models: Album and Song. </li>
    <li> First, we open a python shell which will allow us to manipulate data in the database.
 
    python manage.py shell
   </li>
   <li> Then we import the tables to the shell:
    
    from music.models import Album, Song
  
   </li>
   <li> To create a new album object:
      
     a =Album(artist=”Taylor Swift”, albumn_title=”Red”, genre=”pop”, album_logo = “url")

   <br/>
      Essentially, this adds a row to the album table.

   </li>
   <li> Some other functions:
     <ul>
      <li>a.save() - then saves the data</li>
      <li>a.artist - prints artist name</li>
      <li>a.id/ a.pk - prints the primary key value</li>
     </ul>
    </li>
    <li>To select a particular album object:
      Album.objects.filter(id=1) - used to print the objects of album with id =1
      Album.objects.filter(artist__startswith = ‘Taylor’) prints those objects whose name starts with Taylor
    </li>
  </ul>
  <h3>Django's admin interface</h3>
  <ul>
    <li>To create an account in the django admin interface:
      
      python manage.py createsuperuser

   </li>
   <li>To register a model in django's admin interface we include the following statement in the admin.py file:
     
     admin.site.register(class_name)
   </li>
   <li>To access the admin interface, we go to the url 'localhost8000:admin/'</li>
   <li>Once logged in, django provides us with a GUI for manipulating data in our database.</li>
  </ul>
  <h3>Templates</h3>
  <ul>
    <li>Creating templates is a way to separate html and django backend.</li>
    <li>We create a directory under music called templates. Under templates we create another folder with the same name as the parent directory of templates (in this case music). Under music we create index.html.</li>
    <li>We include the following statement to load the newly created template index.html in our index view:
      
     template = loader.get_template('music/index.html')
   </li>
   <li>We then use a dictionary, usually called ‘context’, to pass in information to our template from our backend. For example, we could pass our models into our html templates</li>
   <li>In the index function we have the following return statement:
       
     return HttpResponse(template.render(context, request))

 </li>
  <li>Writing python code in our html templates:
       <ul>
         <li>Python code is enclosed within {%  ….  %}</li>
         <li>Python variables in the html code are enclosed within {{ … }}</li>
       </ul>
    </li>
  </ul>
  <h3>Creating a base template</h3>
  <ul>
    <li>We create an html page in our templates/music folder, say ‘base.html,’ and we include all the code that is to be repeated in all the web pages of the website.</li>
    <li>In the body tag of base.html, we append the variable piece of code by inserting the following bit of code: <br />
             {% block body%} <br />
             {% endblock %}
    </li>
    <li>In our other html pages we include the following bit of code at the top: <br />
                {% extends 'music/base.html' %}
    </li>
    <li>Then we enclose the entire html page with the same bit of code as our base.html:
                       {% block block_name %} <br />
                       {% endblock %}
    </li>
    <li>We can create multiple blocks. We just give it another name (here the name of the block is body).</li>
  </ul>
  <h3>Static files</h3>
  <ul>
    <li>In order to store css files and images we can create a directory to store static files in our main app directory called “static”.</li>
    <li>To load static files into your index.html, we use
          {% load staticfiles %} <br />
          </li>
  </ul>
  <h3>Generic Views</h3>
  <ul>
    <li>Generic views are based on the idea that most websites can be generalized in a particular way. They all consist of a list in the homepage and clicking on an item in the list leads to a page which consists of details of the list.</li>
    <li>We include the following import statement in views.py:
        
       from django.views import generic
       
   </li>
   <li>We use classes while defining generic views (unlike conventional views where we use functions).</li>
   <li>In views.py, we define two classes IndexView and DetailsView that inherit the generic.ListView and generic.DetailView classes respectively.</li>
    <li>In the classes we define template_name = ‘the template that we want to load’</li>
    <li>In urls.py we mask the class as a function to call by the following. For instance:
     
        url(r'^$', views.IndexView.as_view(), name='index')
     
   </li>
   <li>Default name whenever you return a list of objects from the listview generic function is ‘object_list’</li>

  </ul>
  <h3>Creating a REST API in Django</h3>
  <ul>
    <li>REST APIs are used to create a common format for devices to access the data from your website. Typically the standard format is JSON, XML or YAML. </li>
    <li>To handle APIs we install the rest framework:
       
      pip install django restframework

  </li>
  <li>Add ‘rest_framework’ to the INSTALLED_APPS array in settings.py</li>
  <li>To create REST APIs we create a serializer class - A serializer class is one that converts our model objects into JSON format.</li>
    <li>Whenever the user sends a request to the API, we want to return a JSON response (unlike a conventional http response).</li>
    <li>To do this we serialize the data. Serializing is essentially storing data in a specialized format that can be universally stored and accessed.</li>
    <li>Create a serializers.py file in the app directory.</li>
    <li>In serializers.py:
     
          from rest_framework import serializers
          from .models import model_name

          class model_nameSerializer(serializers.ModelSerializer):
	          class Meta:
		          model = model_name
		          fields = (field_names)
      
   </li>
   <li>In views.py:
      
        from django.shortcuts import get_object_or_404 #function that returns the object if present or returns a 404 not found error
        from rest_framework.views import APIView #way to make normal views return API data.
        from rest_framework.response import Response #allows us to return  the JSON response
        from rest_framework import status #allows us to return  the JSON response
        from .models import model_name
        from .serializers import model_nameSerializer

        class model_nameList(APIView):

        def get(self, request): #defines behavior for a get req. made to the API
          models = model_name.objects.all()
          serializer = model_nameSerializer(models, many = True)
          return Response(serializer.data)

   </li>
   <li>In urls.py:
     
      from rest_framework.urlpatterns import format_suffix_patterns
      from app_name import views

      urlpatterns = [...] #define standard url patterns. Don’t forget to use as_view() function
      urlpatterns = format_suffix_patterns(urlpatterns)
   </li>
  </ul>
