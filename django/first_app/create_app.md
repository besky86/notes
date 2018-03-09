# 创建项目
```
django-admin startproject blog
```
创建好应用之后，会生成对应的文件

```
.
├── LICENSE
├── README.md
└── blog
    ├── blog
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── db.sqlite3
    └── manage.py
```

* 外部 blog/文件夹，项目文件夹
* manage.py 和Django项目交互的命令行配置
* 内部 blog/文件夹 项目实际存放Python包的目录
* blog/settings.py Django项目的配置
* blog/urls.py 链接路径url,
* wsgi.py blog项目的兼容WSGI的web服务器

# 启动项目
```
python manage.py runserver
```
也可指定端口：
```
python manage.py runserver 8080
python manage.py runserver 0:8080
```

# 创建main
```
python manage.py startapp mainpage
```

目录结构为：
```
├── blog
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   ├── settings.cpython-36.pyc
│   │   ├── urls.cpython-36.pyc
│   │   └── wsgi.cpython-36.pyc
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── db.sqlite3
├── mainpage
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   ├── urls.cpython-36.pyc
│   │   └── views.cpython-36.pyc
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
└── manage.py
```

1. 我们在mainpage/views.py文件中创建一个视图：
```
def index(request):
    return HttpResponse("mainpage")
```

2. 然后在mainpage/urls.py中创建对应的uri:
```
from django.urls import path

from . import views

urlpatterns = [
        path("", views.index, name='index'),
]
```

3. 在blog/urls.py中包含对应的mainpage中的urls：
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('mainpage.urls')),
    path('admin/', admin.site.urls),
]
```

# 如何添加urls:
基本是使用django.urls中的path函数, 他有两个必须参数： route和view,可选参数 kwargs和name,参数说明如下：
* router, 请求的uri.处理请求时，Django从urlpatterns中的第一个path开始匹配，直到匹配到为止
* view，视图函数
* kwargs: 传递到view的参数
* name: path命名
在blog/urls中可以看到以下注释：

```
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
```
从上面的注释可以看出： view参数可以是一个返回HttpResponse的函数，也可以是继承了View.as_vew()的返回值
