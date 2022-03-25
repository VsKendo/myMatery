---
title: 学习搭建Django项目 - 1
date: 2022-03-25 08:01:59
tags: [python, Django]
author: VsKendo
categories: python
summary: 从0开始学习Django，根据官方文档进行补充，建议和官方文档一起看。
description: 从0开始学习Django，根据官方文档进行补充，建议和官方文档一起看。
---

# 学习搭建Django项目 - 1

项目采用python3.9+Django 4.0.3，项目github：https://github.com/VsKendo/django_demo。

本教程将指引你阅读官方文档，推荐在阅读官方文档的时候同步查看本教程。

更新时间：2022年3月25日

官方文档信息：

- ​      [         Getting Help       ](https://docs.djangoproject.com/zh-hans/4.0/faq/help/)    

- ​      语言： **zh-hans**    

- ​       文档版本：         **4.0**           

## 安装Python和Django

若你还未安装Python或Django，请看官方文档【中文】：

快速下载和安装指南： https://docs.djangoproject.com/zh-hans/4.0/intro/install/

这里不再赘述。

---



# 开始

本教程需配合官方文档【中文】：https://docs.djangoproject.com/zh-hans/4.0/intro/tutorial01/ 使用，因为有些官方文档已经说得很详细了，这里不再赘述。

**查看官方文档过程中请配合本教程使用**。

**查看官方文档过程中请配合本教程使用**。

**查看官方文档过程中请配合本教程使用**。

当您**看完**官方文档“编写你的第一个 Django 应用，第 1 部分”后，建议按照本教程对项目进行修改。

## 文件说明（tree）

在你新建了Django项目后，你会得到以下文件。

```html
.  //根目录只是你项目的容器， 名称对 Django 没有影响，可以将它重命名为任何你喜欢的名称。
├── manage.py //一个让你用各种方式管理 Django 项目的命令行工具。
├── ossbe //包名（项目名），引用它内部任何东西时需要用到。
│   ├── __init__.py //一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。
│   ├── asgi.py //运行在 ASGI 兼容的 Web 服务器上的入口。
│   ├── settings.py //Django 项目的配置文件。
│   ├── urls.py //Django 项目的 URL 声明，就像你网站的“目录”。
│   └── wsgi.py //运行在 WSGI 兼容的Web服务器上的入口。
└── templates
```

---



# 注册app

请修改`settings.py`中的 `INSTALLED_APPS`，加上你的网站中的Config类。

### 注意

在教程中，会让你使用 `python manage.py startapp websitename`命令创建一个**网站**（我更愿意称之为**拦截器**），这个命令可以运行多次，即一个项目中可以用这个命令创建多个网站（拦截器）。后面的 websitename 为你给网站（拦截器）的名字。每次运行这个命令，都要在 `INSTALLED_APPS`中加上一行 `'websitename.apps.websitenameConfig'`。

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'manager.apps.ManagerConfig',
]	
```

最后一行便是我添加的，我的项目名为 ossback ，网站（拦截器）文件夹名为 manager。因此只增加一行： `'manager.apps.ManagerConfig'` 。

在往后所有的教程中，使用 `python manage.py startapp websitename`命令创建的文件夹，我都会叫它“**拦截器**”而非“**网站**”（为了避免混淆）。



# 创建templates文件夹

你要创建templates文件夹，用于存储html文件。这时候就要说到项目规范问题了。

1. 显然，我们目前开发的Django是**前后端不分离**的
2. 每个网站都有其各自使用的 `html 和静态（js、css、img）文件`，以及可能会有公用的 `html 和静态（js、css、img）文件`

终上所述，后端文件和前端文件应该单独存放，我个人推荐使用如下目录结构。

## 目录结构

1. 每个网站都有自己的 static 文件夹，但都**没有** templates 文件夹。所以，在**每个网站文件夹中**创建static文件夹，static文件夹下再创建css、js和img文件夹，用于存储静态文件。
2. 只有一个 templates 文件夹，其和 manage.py 同级
3. 在 templates 文件夹中，只能出现 html 文件。
4. 在 templates 文件夹中，子文件夹命名为 websitename1、websitename2...以及shared。用于存放网站1、网站2...和共享的 html 文件。
5. 在 templates 文件夹的同级创建一个 common_static 文件夹，用于存放共用的css、js和img文件。

目录结构大概如下：

```html
. （省略不重要的文件和文件夹）
├── website1 （网站1文件夹）
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── static （网站1私有的静态文件的文件夹）
│   │   ├── css
│   │   ├── img
│   │   └── js
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── website2 （网站2文件夹）
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── static （网站2私有的静态文件的文件夹）
│   │   ├── css
│   │   ├── img
│   │   └── js
│   ├── tests.py
│   └── views.py
├── collect_static (在部署到Nginx时，项目中所有静态文件会被统一放在这里)
├── common_static （在开发时需要，用于存放共用的静态文件）
│   ├── css
│   ├── img
│   └── js
├── manage.py
└── templates （所有前端文件位置）
    ├── website1 （网站1私有的 html 文件）
    │   └── test1.html
    └── website2 （网站2私有的 html 文件）
        └── test2.html
    └── shared （所有网站共用的 html 文件，如 footer.html 等）
        └── shared.html
```

## 修改 settings.py 

只需要修改这个文件中的一些配置，便可以正常使用了。

1. 在开头增加了 `import os`

2. 修改了 TEMPLATES 中的 DIRS（确保值为`[BASE_DIR / './templates'] ` ） 和 context_processors （增加了 `'django.template.context_processors.static',`）

   ```python
   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': [BASE_DIR / './templates']
           ,
           'APP_DIRS': True,
           'OPTIONS': {
               'context_processors': [
                   'django.template.context_processors.debug',
                   'django.template.context_processors.request',
                   'django.template.context_processors.static',
                   'django.contrib.auth.context_processors.auth',
                   'django.contrib.messages.context_processors.messages',
               ],
           },
       },
   ]
   ```

3. 修改了语言、时区

   ```python
   LANGUAGE_CODE = 'zh-hans'
   TIME_ZONE = 'Asia/Shanghai'
   ```

4. 修改 `template_processors`配置，原本只有 `STATIC_URL` ，现在变为：

   ```python
   # STATIC SETTINGS
   STATIC_URL = '/static/'
   # BASE_DIR 是项目的绝对地址
   STATIC_ROOT = os.path.join(BASE_DIR, 'collect_static')
   # 以下不是必须的  各个app共用的文件可以放在这
   STATICFILES_DIRS = (
       os.path.join(BASE_DIR, 'common_static'),
   )
   ```

现在，关于 Django 显示页面的基本配置已经完成，接下来就是写 html 文件了。



# 在views中返回html文件

根据官方文档，我们知道，以下`views.py`中的代码将返回一串文字

```python
def index(request):
    return HttpResponse("Hello, world. You're at the index.")
```

为了返回html页面，我们可以在它下面新增函数，如：

```python
def test(request):
    return render(request, "test.html")
```

其中的html文件在template文件夹下，内容为：

```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>欢迎来到测试文件页面</title>
</head>
<body>
<h1>测试html页面能否被正确引用</h1>
<img src="{% static 'img/avatar.png' %}" alt="无">
<img src="{{ STATIC_URL }}img/avatar.png" alt="无">
</body>
</html>
```

最后，修改`urls.py`：

```python
urlpatterns = [
    path('index/', views.index, name='index'),
    path('', views.index, name='index'),
    path('test/', views.test, name='test'),
]
```

## html文件引用静态文件

在 html 文件头添加 `{% load static %}`，在需要引用的位置使用 `{% static '路径' %}`

如，引用 static 文件夹下的 img 文件夹中的 img1.png：

```html
<img src="{% static 'img/avatar.png' %}" alt="无">
<img src="{{ STATIC_URL }}img/avatar.png" alt="无">
```

> 其实，这种方法之所以能找到static文件夹，是因为 settings.py 中 STATIC_URL 的值为 '/static/'

以上两种方式都可以使用。

## 访问我们刚创建的页面

url：`http://localhost:8000/websitename/test`

其中 websitename 是你自己创建的拦截器的名字。



# 创建一个新的网站（拦截器）

根据单一职责原则，我们可能要使用指令 `python manage.py startapp websitename` 创建多个拦截器从而响应用户不同的请求。每次创建一个新的拦截器，你都应该至少进行以下步骤：

1. 使用指令 `python manage.py startapp websitename` 
2. 给 websitename 文件夹创建 static 文件夹。
3. 新建 urls.py，并修改其中的内容
4. 在项目的 urls.py 里引用你刚新建的 urls.py
5. 在 templates 下新建 websitename 文件夹
6. 修改 settings.py 中的 `INSTALLED_APPS`，注册新的拦截器

---



# References

1. 【官方中文文档】编写你的第一个 Django 应用：https://docs.djangoproject.com/zh-hans/4.0/intro/tutorial01/
1. 【该文章有一处错误】Django STATIC_URL 的理解：https://blog.csdn.net/qq_24551305/article/details/84865025
1. django中如何使用template_processors避免频繁的函数传递给html，使用template上下文的方法传递方法：https://blog.csdn.net/qq_37463791/article/details/103607884
