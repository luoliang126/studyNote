# django笔记

1. ##### django的安装

   ```python
   到官网上下载安装包：https://www.djangoproject.com/download/（必须先安装python）
   菜鸟教程安装步骤：https://www.runoob.com/django/django-install.html
   进入 Django 目录，执行 python setup.py install
   然后开始安装，Django 将要被安装到 Python 的 Lib下site-packages。
   然后是配置环境变量，将这几个目录添加到系统环境变量中： 
   C:\Python33\Lib\site-packages\django
   C:\Python33\Scripts
   验证是否安装成功？
   执行cmd操作
   >>> import django
   >>> django.get_version()
   如果有django的版本即安装成功！
   ```

2. ##### django的基本操作

   ```python
   2.1创建一个项目
   django-admin startproject '项目目录名称'
   2.2、启动项目（在项目根目录下）
   python manage.py runserver // 默认使用8000端口
   python manage.py runserver 8080 // 修改访问端口为8080
   注：一般可以通过localhost访问，如果要通过本机ip访问，需要修改settings中的配置
   ALLOWED_HOSTS = [192.168.1.6] //本机ip
   2.3、创建一个应用
   python manage.py startapp sale 
   // sale为应用名称(注意该sale目录与根目录并行，但是他们是上下级的关系)
   2.4、在每个应用中的views中创建路由
   2.4.1、sale下的 views.py文件中
           from django.http import HttpResponse // 引入django的HttpResponse
           def index(request): // 定义一个index函数
               return HttpResponse("Hello, world. You're at the polls index.")
   2.4.2、在项目的根目录下找到urls.py，这里配置路由
           from sale.views import index; 
       	// 引入刚才sale下的views文件下的 index 函数
           path('sale/index/', index),  
           // 配置路由地址 'sale/index/'，并指向刚才引入的index函数。
       这样一个sale下的index路由就创建成功了
   2.4.3、python manage.py runserver  //打开python服务
       在地址栏输入http://127.0.0.1:8000/sale/index/ 
   	就可以看到index函数返回的 "Hello, world. You're at the polls index."
   
   上面的方法创建几个、或者几十个小型项目路由地址还可以，但是大型项目这样创建的话，根目录下的urls.py文件就会显得很臃肿，解决办法就是路由拆分组合。
   2.4.4、路由拆分思路：每一个应用（如sale应用）,在该应用下创建一个urls.py专门用来处理这个应用下的路由地址和函数方法处理该路由，再将该文件暴露出去，在根目录下的urls.py文件中引入sale应用中的urls.py文件，那么sale应用的所有路由，就全部添加到根目录下的urls.py中，达到分层的效果，当然也可以继续嵌套（推荐两级路由就够用了！）
   在sale目录下创建二级路由文件urls.py
       from django.urls import path
       from sale.views import index,index1,index2  
       //从sale中引入所有该应用的路由，并挂在urls.py中，一起暴露出去
       urlpatterns = [
           path('index/', index),
           path('index1/', index1),
           path('index2/', index2),
           ......
       ]
   在根目录中的urls.py(相当于一级路由)中引入该二级路由以及该二级路由下的所有子路由（因为该应用下的路由，都挂在在该文件的二级路由下了）
   from django.urls import path,include 
   //注意这时的根urls.py中需要引入include(用于引入所有的二级路由)
       urlpatterns = [
           ......
           path('sale/', include('sale.urls')), 
       ]
   ```

3. ##### 数据库的操作sqlite3

   ```python
   3.1、数据库的创建（使用python自带的sqllite3）
       执行初始化数据库:python manage.py migrate 
       // 会在根目录下创建一个db.sqlite3的数据库文件
       使用navicat可视化数据库视图，查看该数据库。
       创建连接，创建数据库名称myProgramme，用户名：luoliang，密码：123456
       注意：执行python manage.py migrate后，创建的应用都会有apps.py和models.py文件，每一个应用都有	  单独对应的数据库
   3.2、数据库的操作ORM（Object relational mapping）对象映射
   3.2.1、在创建的每个应用下都有apps.py和models.py文件，并手动添加到根目录的settings.py文件中去
       INSTALLED_APPS = [
           ......
           'common.apps.CommonConfig' // common应用下的apps.py文件中的CommonConfig数据库模型
       ]
       其中apps.py相当于配置文件(配置当前应用的数据模型名称)
       from django.apps import AppConfig
       class LogincontentConfig(AppConfig):
           name = 'loginContent'
   
       models.py为数据模型
       from django.db import models
       创建一个数据模型User，映射数据库的一张User表
       class User(models.Model):
           # 用户名称
           username = models.CharField(max_length=20)
           # 联系电话
           mobile = models.CharField(max_length=11)
           # 密码
           password = models.CharField(max_length=18)
   
   3.2.2、当数据模型models变更时,例如User模型发生变更（添加字段或者其他操作），需要通知数据库中User表字段相应变更。
   python manage.py makemigrations loginContent // 检查common应用中是否有数据模型变更
   // 如果有多个数据模型变更python manage.py makemigrations loginContent loginContent1 ......
   
   假如：之前有一张表有以下字段username,mobile,password。并且这张表已经存储有数据。这个时候我们要再添加字段时，必须再添加的新字段中给一个默认值，如果不给就会报错'缺省值'。
       from django.db import models
       class User(models.Model):
           username = models.CharField(max_length=20)
           mobile = models.CharField(max_length=11)
           password = models.CharField(max_length=18)
           qq = models.CharField(max_length=20,null=True,blank=True)
   这里的qq字段就是后面添加的，且初始值为null,可以没有该字段。
   
   3.2.3、执行python manage.py makemigrations loginContent后，只是在loginContent应用中的migrations目录下创建了一个临时的修改数据模型字段，并未真正提交到数据库修改。还需要执行一段命令
   python manage.py migrate   // 这个时候我们再到navicat中刷新以下表，就会发现多了一个loginContent_user的表了，或者是更新了该表
   
   3.2.4、用户信息表，django已经帮我们创建好了（当然你也可以再重新创建一张用户表）
   创建超级管理员账号 python manage.py createsuperuser，然后按照提示输入用户名、密码（至少8个字符）、邮箱等
   django有一套自己的管理数据界面，运行python manage.py runserver   然后访问http://127.0.0.1:8000/admin
   访问输入刚才我们创建的超级管理员账号和密码登陆。这是发现只有一个系统自带的Auth表，没有我们创建的User表？？？
   所以需要将User这张表手动添加到Admin这个管理下，在loginContent下的models.py数据模型中添加：
       from django.db import models
       class User(models.Model):
           # 用户名称
           username = models.CharField(max_length=20)
           # 联系电话
           mobile = models.CharField(max_length=11)
           # 密码
           password = models.CharField(max_length=18)
       # 将User表添加注册到admin管理界面，就可以实现admin管理这张表
       from django.contrib import admin
       admin.site.register(User)
   注：admin界面可以通过视图操作数据库，是一套不错的后端管理界面，但是它样式固定，不方便修改，且不能按照一定的业务逻辑修改界面。所以我们需要开发一套自己的前端界面，既可以达到视图的优化，又能够操作数据！
   ```

4. ##### 数据库的读写操作

   ```python
   
   ```

5. ##### python链接mongo数据库--pymongo

6. ##### get/post请求，以及返回json对象

   ```python
   from django.shortcuts import render
   from django.http import HttpResponse
   import json;
   def index(request):
       # 接收请求参数（get和post方式）
       username = request.GET.get('username', '')
       print('用户名是' + username)
       
       username = request.POST.get('username', '')
       print('用户名是' + username)
   
       # 返回一个字符串
       # return HttpResponse("Hello, world. You're at the polls index.")
   
       # 返回一个json对象
       data = {
           'patient_name': '张三',
           'age': '25',
           'patient_id': '19000347',
           '诊断': '上呼吸道感染',
       }
       return HttpResponse(json.dumps(data,ensure_ascii=False),content_type="application/json,charset=utf-8")
   ```

7. 虚位以待！