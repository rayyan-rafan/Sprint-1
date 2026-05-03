# 🚀 Django Blog Project – Sprint 1

## 📌 Overview
This project is part of a capstone Sprint 1, focused on building a foundational Django blog application with MySQL integration and CI testing.

### Key Objectives:
- Set up a Django blog application
- Integrate MySQL as the database
- Implement blog features (model, views, templates)
- Write unit tests
- Configure CI pipeline using GitHub Actions

---

## 🧱 Tech Stack

- Backend: Django (Python 3.10)
- Database: MySQL
- CI/CD: GitHub Actions
- Testing: Django Test Framework
- Environment: Ubuntu / EC2

---

## 🎯 Sprint 1 Goals

- Create basic blog structure  
- Integrate MySQL database  
- Implement blog model and admin  
- Build views and templates  
- Write unit tests  
- Configure CI pipeline  

---

## ⚙️ Project Setup

### 1. Clone Repository
git clone https://github.com/rayyan-rafan/Sprint-1.git  
cd Sprint-1  

---

### 2. Create Virtual Environment
python3 -m venv venv  
source venv/bin/activate  

---

### 3. Install Dependencies
pip install -r requirements.txt  

---

## 🐬 MySQL Setup

### Install MySQL
sudo apt-get update  
sudo apt-get install mysql-server  
sudo systemctl start mysql  

---

### Login to MySQL
sudo mysql -u root -p  

---

### Create Database and User
CREATE DATABASE blog_db;  
CREATE USER 'blog_user'@'localhost' IDENTIFIED BY 'your_password';  
GRANT ALL PRIVILEGES ON blog_db.* TO 'blog_user'@'localhost';  
FLUSH PRIVILEGES;  

exit;  

---

### Install MySQL Client for Django
sudo apt-get install libmysqlclient-dev python3-dev  
pip install mysqlclient  

---

## ⚙️ Django Configuration

Update blog_project/settings.py:

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'blog_db',
        'USER': 'blog_user',
        'PASSWORD': 'your_password',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}

---

## 🔄 Database Migration

python3 manage.py makemigrations  
python3 manage.py migrate  

---

## 🧩 Application Features

### Blog Model

from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

---

### Admin Panel

python3 manage.py createsuperuser  

Access:
http://127.0.0.1:8000/admin  

blog/admin.py:

from django.contrib import admin
from .models import Post

admin.site.register(Post)

---

### Views

from django.shortcuts import render
from .models import Post

def post_list(request):
    posts = Post.objects.all()
    return render(request, 'blog/post_list.html', {'posts': posts})

---

### Template

Create: blog/templates/blog/post_list.html

<!DOCTYPE html>
<html>
<head>
    <title>Blog</title>
</head>
<body>
    <h1>Blog Posts</h1>
    <ul>
        {% for post in posts %}
            <li>{{ post.title }} - {{ post.created_at }}</li>
        {% endfor %}
    </ul>
</body>
</html>

---

### URLs

blog/urls.py:

from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]

blog_project/urls.py:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]

---

## ▶️ Run Application

python3 manage.py runserver  

Open:
http://127.0.0.1:8000/  

---

## 🧪 Sample Data

python3 manage.py shell  

from blog.models import Post  
Post.objects.create(title="First Post", content="Content of first post")  
Post.objects.create(title="Second Post", content="Content of second post")  

---

## 🧪 Testing

python3 manage.py test  

Example:

from django.test import TestCase
from .models import Post

class PostModelTest(TestCase):

    def setUp(self):
        Post.objects.create(title="Test Post", content="Test content")

    def test_post_title(self):
        post = Post.objects.get(id=1)
        self.assertEqual(post.title, "Test Post")

---

## 🔄 CI Pipeline (GitHub Actions)

Create:
.github/workflows/django.yml  

name: Django CI

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      mysql:
        image: mysql:8.0
        env:
          MYSQL_ROOT_PASSWORD: your_password
          MYSQL_DATABASE: blog_db
        ports:
          - 3307:3306

    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.12'
      - run: pip install -r requirements.txt
      - run: python3 manage.py check
      - run: python3 manage.py test

---

## 📂 Project Structure

Sprint-1/
│── blog_project/
│── blog/
│── .github/workflows/
│── requirements.txt
│── README.md

---

## 📸 Screenshots

Include:
- Homepage
- Endpoint output
- Test results
- Docker containers (if used)

---

## ✅ Sprint 1 Checklist

- Blog app created  
- MySQL integrated  
- Views working  
- Tests added  
- CI pipeline configured  

---

## 🚀 Next Steps (Sprint 2)

- Dockerize app  
- Deploy on AWS  
- Add authentication  

---



## ⭐ Notes

- Avoid using root user in production  
- Use environment variables for secrets  
- Ensure MySQL is running before migrations  
