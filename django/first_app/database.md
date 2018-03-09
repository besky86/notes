# 启用数据库

打开blog/settings.py文件,可以看到如下代码:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
在代码中有'default'的配置：
* ENGINE: 数据库驱动
* NAME: 数据库名称, sqlite3数据库是使用其绝对路径.
其实，还有更多的参数：
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```

# 对数据库进行升级：
:
