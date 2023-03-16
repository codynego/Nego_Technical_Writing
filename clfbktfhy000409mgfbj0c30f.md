---
title: "A step by step guide to building Blog ApI with Django Rest Framework Part 1"
seoTitle: "a step by step guide to building blog api with django rest framework"
seoDescription: "In this post, i will be guiding you through the porcess of creating a blogapi with django rest framework"
datePublished: Thu Mar 16 2023 20:41:14 GMT+0000 (Coordinated Universal Time)
cuid: clfbktfhy000409mgfbj0c30f
slug: a-step-by-step-guide-to-building-blog-api-with-django-rest-framework
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/BI465ksrlWs/upload/527f1d8073b75348f1a255ca5d3dd885.jpeg
tags: django, rest-api, django-rest-framework, 2articles1week, blog-api

---

In this post, I will cover how you can build a blog API in DRF (Django Rest Framework). This is a step-by-step guide that will help you get started in building your (probably) first API project.

I will assume you already have python installed on your computer and that you know at least basic python, if not you can check out other tutorials on how to do that.

so lets get started:

First open your VSCode Editor or your ubuntu terminal but for this project i will be using my ubuntu terminal but the process remains almost the same with your vscode

create a project project and change into it

```python
mkdir BlogApi
cd BlogApi
```

Next is to setup a virtual environment so as to isolate your project from other projects.

And you can do that using this command

```python
python -m venv <virtual evironment name>
```

you can replace &lt;virtual evironment name&gt; with any name of your choice but for this tutorial, we will be using "env" as our vuitual environment name

```python
python -m venv env
```

Activate your virtual environment

```python
source .env/Scripts/activate
or
./env/Scripts/activate
```

you will notice your terminal changed to something like this

`(env) PS C:\Users\HP\BlogAPI`

depending on what you are using.

Now lets move to the next step, which is installing Django.

In your current working directory,

type this command to install django

```python
pip install django
```

wait for fews seconds and voila...Django finally installed

if you have come this far without experiencing any error, congratulation and if you did, that's also awesome

take a deep breath, recheck where you messed up, google your errors as you try to fix them. Bux and Developers are the most closest partners &lt;smile&gt;.

Now let's proceed..

currently this is how your directory should look like

```python
BlogApi/
└── env/
    ├── Scripts/
    ├── include/
    ├── lib/
    └── pyvenv.cfg
```

now lets create our django project.. in the same directory, type

```python
django-admin startproject blogapi .
```

and our django app

```python
python manage.py startapp blog
```

by now your directory should be like this

```python
BlogApi/
├── env/
│   ├── bin/
│   ├── include/
│   ├── lib/
│   └── pyvenv.cfg
├── manage.py
├── blogapi/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── blog/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    └── views.py
```

if all is as it is, then lets proceed

When this is done, you will have to add your app to your main project. Here is how to do that

1. Open the [`settings.py`](http://settings.py) file located in your project folder `BlogApi/blogapi/settings.py` using your code editor.
    
2. Find the `INSTALLED_APPS` list in the file, which should already have some default apps installed like `django.contrib.admin`, `django.contrib.auth`, etc.
    
3. Add the name of your app `'blog'` at the end of the list, like this:
    

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```

Now let's define our Blog model in the [`models.py`](http://models.py) file:

```python
from django.db import models

class Category(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name
class Post(models.Model):
    title    = models.CharField(max_length=100)
    content  = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    category = models.ForeignKey(Category, on_delete=PROTECT)

    def __str__(self):
        return self.title
```

This creates a `Post` model with four fields: `title`, `content`, `created_at`, and `updated_at`. The `created_at` field will be automatically set to the current date and time when a new post is created, and the `updated_at` field will be automatically updated to the current date and time whenever a post is updated.

after creating your models, you have to run the this command to make the neccesary migrations

```python
python manage.py makemigrations
python manage.py migrate
```

Next, let's create a serializer for our `Post` model. But before we do that, we first need to install `django rest framework`

```python
pip install djangorestframework
```

then add it in to your list of installed app in your `settings.py` file

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
    'djangorestframework',
]
```

Now when that is done, create a `serializer.py` file in your app directory.

In the [`serializers.py`](http://serializers.py) file in the `blog` app, add the following code:

```python
from rest_framework import serializers
from .models import Post, Category

class CategorySerializer(serializers.ModelSerializer):
    class Meta:
        model = Category
        fields = ('id', 'name')

class PostSerializer(serializers.ModelSerializer):
    Category = CategorySerializer()
    class Meta:
        model = Post
        fields = ('id', 'title', 'content', 'created_at', 'updated_at', 'category')
```

This creates a `PostSerializer` that will serialize our `Post` model into JSON format. The serializer includes all the fields in the `Post` model same thing also applies to the category but if you noticed we added a category field to the `PostSerializer` this is so that we can include the category the post belongs to when we call the API.

Now we need to create views that will handle the HTTP requests for our API. In the [`views.py`](http://views.py) file in the `blog` app, add the following code:

```python
from rest_framework import generics
from .models import Post
from .serializers import PostSerializer

class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

The first class `PostList` will handle the get and post request which means that you can retrieve all posts and create a new post in the database. While the second class `PostDetail` will handle the get, put, patch and delete of a specific post .

Here's a brief overview of what these methods mean:

* `GET`: Used to retrieve a resource or a list of resources from a server. It is a safe method that should not modify any resources on the server.
    
* `PUT`: Used to update a resource on the server. It replaces the entire resource with the new data provided in the request.
    
* `PATCH`: Used to update a resource on the server. It is similar to PUT, but only updates the fields that are provided in the request, leaving the rest of the fields unchanged.
    
* `DELETE`: Used to delete a resource from the server.
    

now that you understand what each of these methods means, lets proceed to the next step which is configuring our urls.

If you check your directory `Blog`, you will notice that `urls.py` file isn't present by default, so we are going to create one..

when created,In the [`urls.py`](http://urls.py) file in the `blog` app, add the following code:

```python
from django.urls import path
from blog.views import PostList, PostDetail

urlpatterns = [
    path('posts/', PostList.as_view(), name='post-list'),
    path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),
]
```

This creates two URLs: `/posts/` for the `PostList` view, and `/posts/<int:pk>/` for the `PostDetail` view

Also the `urls.py` in the `blogapi` project directory, you need to map your app urls to your project urls.

In the [`urls.py`](http://urls.py) file, import the `include` function from the `django.urls` module

```python
from django.urls import path, include
```

Then define a URL pattern that includes the URL patterns of your app. You can do this by adding the following line of code to the urlpatterns list:

```python
urlpatterns = [
    path('admin/', admin.site.urls)),
    path('blog/', include('blog.urls')),
   # other URL patterns go here
]
```

Hooray!!!

If you made it this far, it means you have finally created your own BlogApi project, you can now start the development server and access your API by running

```python
python manage.py runserver
```

and access your api using

```python
 http://localhost:8000/posts/
or
 http://localhost:8000/posts/<postid>
```

So thats it, congratulation !!!, In my next post, i will share with you how to add Filtering, Searching and Ordering feature to your API project.