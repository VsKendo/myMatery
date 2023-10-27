---
title: 从0搭建Django项目 - 2. Django逆向工程
date: 2023-10-27 01:11:09
tags: [python, Django, 逆向工程]
top: true
author: VsKendo
categories: python
summary: 使用Django自带的逆向工程，完成类到数据库自动建表。
description: 使用Django自带的逆向工程，完成类到数据库自动建表。
---

# 简介

最近重新研究了Django，发现也没想象中那么简单，也没那么好用。

我们会编写一个类，然后希望数据库也对应生成这个表。而逆向工程就是自动化这个步骤。

由于现在用的都是python3，所以接下来的所有命令，可能要将`python`改为`python3`.

# Django项目逻辑

在新建项目后，是没有app的，项目里一开始只有django的管理端。此时我们还不能写任何的业务逻辑。

## 目录介绍

这个管理端的文件夹的名字与你新建Django项目时的一致。比如你使用`django-admin startproject mysite`新建项目，此时你的项目名称就是mysite。目录如下：

```bash
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

你的文件将存在第一行的`mysite`目录下，而`manage.py`就是管理这个项目的入口。项目的具体设置（比如配置文件）就在`mysite/mysite`下，这个目录就是我们说的管理端。此时可以使用`python manage.py runserver`来运行Django.

## 新建app

使用`python manager.py startapp myappname`来新建一个叫`myappname`的应用。因为Django使用的不是MVC模型，所以一个这样创建的app就是一个Service，这有点类似于微服务。比如用`python manager.py startapp account`来管理登录，而同时用`python manager.py startapp payment` 创建一个app来管理支付。

# 流程

终于到逆向工程的步骤了。总体步骤是：

1. 新建一个app。
2. 在app的`models.py`中编写要被逆向的类。
3. 使用下面的步骤完成逆向工程。

## 步骤

开始前，默认已经完成了上述的流程2：在app的`models.py`中编写要被逆向的类。

1. 和上面介绍的一样，我在Django中新建一个app叫`myappF23`。

2. 在Django的`mysite/settings.py`中

   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'myappF23.apps.Myappf23Config',
   ]
   ```

   在第8行将告诉Django：myappF23是一个app项目，而不是一个普通文件夹。

4. 运行`python manage.py makemigrations myappF23`，让逆向工程为`settings.py`里的`myappF23`这个app创建一个`migrations`文件夹。这里的`0001_initial.py`将是逆向工程会运行的代码。(这个`.py`文件中存储的显然是python代码。此时，你可以使用` python manage.py sqlmigrate myappF23 0001`来查看``0001_initial.py``将会执行什么具体的SQL语句)

5. 运行`python manage.py migrate 0001 `，这步将让Django使用这个0001创建数据库。因此你的目录下会多出`db.sqlite3`文件。

6. 在完成创建数据库后，创建超级管理员，`python manage.py createsuperuser`。

7. 可以使用`127.0.0.1:8000/admin`，使用你在第四步创建的账号密码登录。登录后，可以在网页中对数据库进行CRUD。

## 更新步骤

上述步骤适用于新建，如果更改了`models.py`，还需要重新执行一遍下述步骤才能更新数据库。

1. 运行`python manage.py makemigrations myappF23`，重新生成一个0002为前缀的py文件。如果多次编写，请在文件夹下查看最新的编号。
2. 使用`python manage.py migrate myappF23 0002  `使得更改生效。

### 更新示例

我意外删除了一个表Course，现在想重新生成。

但是如果按照上述步骤更新，会出现报错：无法找到该表。这也是我讨厌Django的地方，非常难用，完全没考虑很多情况。

解决方法：

1. 先在`models.py`中注释掉Course类

2. 运行`python manage.py makemigrations myappF23`，此时显示如下：

   ```shell
   Migrations for 'myappF23':
     myappF23/migrations/0004_delete_course.py
       - Delete model Course
   ```

3. 按理说，我们应该执行`python manage.py migrate myappF23 0004  `来运行这个编号为0004的代码从而删除Course表，但是由于我已经手动删了，所以需要虚假执行。使用`python manage.py migrate myappF23 0004 --fake `.该方法多了最后的参数，代表我们已经手动替Django操作过这个步骤了，只是让Django记录一下。

   ```shell
   Operations to perform:
     Target specific migration: 0004_delete_course, from myappF23
   Running migrations:
     Applying myappF23.0004_delete_course... FAKED
   ```

4. 取消注释Course类

5. 重新`python manage.py makemigrations myappF23`与`python manage.py migrate myappF23 0005`。成功

# 类的编写

在流程中的第二步提到，我们需要编写类，让其能被逆向，并且在Django中使用类似ORM的方式去对其CRUD。

那么，如何编写类呢？

首先，所有类必须写在 `models.py`中，并且继承`models.Model`这个类。

因此，`models.py`的第一句总是`from django.db import models`.

## 简单示例

```python
# CREATE TABLE "myappF23_student" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "first_name" varchar(100) NOT NULL, "last_name" varchar(100) NOT NULL, "email" varchar(200) NOT NULL UNIQUE, "date_of_birth" date NOT NULL);

class Student(models.Model):
    STUDENT_STATUS_CHOICES = [
        ('ER', 'Enrolled'),
        ('SP', 'Suspended'),
        ('GD', 'Graduated'), ]
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    email = models.EmailField(max_length=200, unique=True)
    date_of_birth = models.DateField()
    status = models.CharField(max_length=10, choices=STUDENT_STATUS_CHOICES, default='ER')

    def __str__(self):
        return self.first_name
```

**这样创建的类会有一个默认的id作为主键。**也可以使用`sid = models.IntegerField(primary_key=True) `或者`sid = models.AutoField(primary_key=True)`来给student类一个id。

## 类与类的关系

类与类之间有2中关系：**多对一**和**多对多**。注意，没有 **一对多或者一对一**，永远只有 **'多对'**开头的关系。

 额外创建一个类：教师类

```python
class Instructor(models.Model):
    first_name = models.CharField(max_length=128)
    last_name = models.CharField(max_length=128)
    bio = models.TextField(null=True, blank=True)

    def __str__(self):
        return self.first_name
```

其中bio字段是老师的简介：

- `blank=True`: 表示在表单验证时，该字段可以为空。这影响到表单的验证行为，允许用户在表单中不填写这个字段。
- `null=True`: 表示在数据库中，该字段可以存储为 NULL。如果这个字段不设置为可空 (`null=False`)，那么在数据库中该字段将不允许存储为 NULL 值。

 再额外创建一个类：课程类

该课程有多对一（1个课程只有1个老师）和多对多（1个课程可以有多个学生上，1个学生也可以选多个课程）关系。

```python
class Course(models.Model):
    COURSE_LEVEL_CHOICES = [
        ('BE', 'Beginner'),
        ('IN', 'Intermediate'),
        ('AD', 'Advanced'),
    ]
    title = models.CharField(max_length=200)
    description = models.TextField()
    Instructor = models.ForeignKey(Instructor, on_delete=models.CASCADE)
    students = models.ManyToManyField(Student, blank=True)
    start_date = models.DateField()
    end_date = models.DateField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    level = models.CharField(max_length=100, choices=COURSE_LEVEL_CHOICES, default='BE')

    def __str__(self):
        return self.title
```

肉眼可见地，多对一关系为一个普通的外键，其中第9行里的`on_delete=models.CASCADE`代表级联删除，表示如果删除了这个教授，那么对应的这个课程也会被删除。**记住，因为是多对一，所以这个关系会写在“多”的类里，那个“一”应该写在前面**。在本案例中，教师类写在课程类的前面，并且外键写在课程类中。

而多对多关系就无所谓定义在哪了。本案例中，因为我是先写的学生类，再写的课程类，所以我将多对多关系写在课程类中。这个关系的定义在第10行。遇到这种情况时，Django其实会额外创建一个表`course_student`来表示多对多关系。`blank=True`代表这个关联表是空也无所谓。

至此，我有了4个表：`course, course_student, student, instructor`.

## 其他

至于如何编写自增主键啥的，或者想了解更多信息的，请看文档：https://docs.djangoproject.com/en/4.2/topics/db/models/

# References

1. 官方文档：https://docs.djangoproject.com/en/4.2/intro/tutorial02/
