# Django - Python Web Framework Guide

> Build powerful web applications with Django

## 📋 Table of Contents

- [What is Django?](#what-is-django)
- [MTV Architecture](#mtv-architecture)
- [Models](#models)
- [Views](#views)
- [Templates](#templates)
- [URLs](#urls)
- [Django ORM](#django-orm)
- [Interview Questions](#interview-questions)

---

## What is Django?

**Django** is a high-level Python web framework that encourages rapid development and clean, pragmatic design.

### Features

✅ Built-in admin interface
✅ ORM (Object-Relational Mapping)
✅ Authentication system
✅ Form handling
✅ Security features
✅ Scalable

---

## MTV Architecture

**Model-Template-View** (similar to MVC)

- **Model**: Database schema (data layer)
- **Template**: HTML presentation (presentation layer)
- **View**: Business logic (logic layer)

---

## Models

```python
from django.db import models

class User(models.Model):
    username = models.CharField(max_length=50, unique=True)
    email = models.EmailField()
    age = models.IntegerField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.username

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['-created_at']
```

---

## Views

```python
from django.shortcuts import render, get_object_or_404
from django.http import JsonResponse
from .models import Post

# Function-based view
def post_list(request):
    posts = Post.objects.all()
    return render(request, 'posts/list.html', {'posts': posts})

# Class-based view
from django.views.generic import ListView

class PostListView(ListView):
    model = Post
    template_name = 'posts/list.html'
    context_object_name = 'posts'
```

---

## Templates

```django
<!-- templates/posts/list.html -->
{% extends 'base.html' %}

{% block content %}
<h1>Posts</h1>
{% for post in posts %}
    <article>
        <h2>{{ post.title }}</h2>
        <p>{{ post.content }}</p>
        <small>By {{ post.author.username }} on {{ post.created_at }}</small>
    </article>
{% empty %}
    <p>No posts yet.</p>
{% endfor %}
{% endblock %}
```

---

## URLs

```python
# urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
    path('post/create/', views.post_create, name='post_create'),
]
```

---

## Django ORM

```python
# Create
user = User.objects.create(username='alice', email='alice@example.com', age=25)

# Read
users = User.objects.all()
user = User.objects.get(id=1)
user = User.objects.filter(age__gte=18)
user = User.objects.first()

# Update
user = User.objects.get(id=1)
user.age = 26
user.save()

# Delete
user = User.objects.get(id=1)
user.delete()

# Queries
User.objects.filter(age__gt=18)  # Greater than
User.objects.filter(username__icontains='ali')  # Case-insensitive contains
User.objects.exclude(age__lt=18)  # Exclude
User.objects.order_by('-created_at')  # Order by (descending)
```

---

## Interview Questions

### 1. What is Django?
**Answer**: High-level Python web framework that follows MTV (Model-Template-View) architecture. Includes ORM, admin interface, authentication.

### 2. Django vs Flask?
**Answer**:
- Django: Full-featured, batteries included, opinionated
- Flask: Lightweight, minimal, flexible

### 3. What is Django ORM?
**Answer**: Object-Relational Mapping system that lets you interact with database using Python code instead of SQL.

### 4. Explain Django middleware
**Answer**: Hooks into Django's request/response processing. Used for authentication, CORS, security, etc.

### 5. What are signals in Django?
**Answer**: Notification system that allows decoupled applications to get notified when actions occur (e.g., pre_save, post_save).

---

**Keywords**: Django tutorial, Django Python, Django models, Django ORM, Django views, MTV architecture, Django interview questions, Python web framework

---

*Note: This is a starter guide. More comprehensive content coming soon!*
