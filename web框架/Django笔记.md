













































































































































# 第一章 Django基础

## 1. **web开发基础术语**

### **1.1**  两种架构

**CS架构(Client/Server：客户端-服务端架构)**

```
举例：QQ、OutLook

CS的客户端是胖客户端

优点：
CS能充分发挥客户端PC的处理能力,很多工作可以在客户端处理后再提交给服务器，用户体验好。

缺点：
对于不同操作系统要相应开发不同的版本，并且对于计算机电脑配置要求也较高
```

**BS架构(Browser/Server:浏览器-服务端架构,特殊的CS)**

```
举例： 网页版邮箱

BS是特殊的CS，此时浏览器充当了客户端，BS的客户端是瘦客户端

优点：

分布性强、维护方便、开发简单且总体拥有成本低   

缺点：

数据安全性问题、对服务器要求过高、数据传输速度慢、软件的个性化特点明显降低，难以实现传统模式下的特殊功能要求。
```



### **1.2**  两种开发模式

**MVC开发模式：**

```
M即Model: 是应用程序中用于处理数据逻辑的部分

V即View: 视图是指用户看到并与之交互的界面

C即Controller（控制器） - 控制器作用于模型和视图上。它控制数据流向模型对象，并在数据变化时更新视图，它使视图与模型分离开
```

 **MTV开发模式：**

```
M:Model，数据模型：这是一个抽象层，用来构建和操作web应用中的数据

T:Template，模板层，负责显示数据

V:View,视图：用于封装负责处理用户请求及返回响应的逻辑

Django 里更关注的是模型（Model）、模板(Template)和视图(Views)，所以Django也被称为MTV框架。
 MVC与MTV在本质上是一样的，都是为了分工明确、“低耦合”
```

 **Django也被称为MTV框架，Django中两种开发模式的具体含义：**

```
MVC:

M ：数据存取部分，由django数据库层处理 

V： 显示数据的界面

C：根据用户输入委派视图的部分，由 Django 框架根据 URLconf 设置，对给定 URL 调用适当的 Python 函数(视图函数)。

MTV:

M ：代表模型（Model），即数据存取层。 该层处理与数据相关的所有事务。例如：如何存取、如何验证有效

T ：代表模板(Template)，即表现层。 该层处理与表现相关的决定： 如何在页面或其他类型文档中进行显示。

V ：代表视图（View），即业务逻辑层。 该层包含存取模型及调取恰当模板的相关逻辑。 可以把它看作模型与模板之间的桥梁。

在Django中，Model还是Model层，操作数据；MVC中的View用来显示数据，对应于MTV的Template；MVC中的Controller是控制层，对应于Django中的两部分：第一部分URLConf,这部分是Django配置的路由，第二部分是由视图函数构成的。
```



## **2.Django简介**

### **2.1 Django是什么？**

一个可以使Web开发工作愉快并且高效的Web开发框架

### **2.2 Django的历史**

Django诞生于2003年的秋天,是由堪斯（Kansas）州 Lawrence 城中的一个 网络开发小组编写，目的是用于快速开发能满足需求的新闻类站点。

Django是一个很“接地气”的框架，它来自于真实世界中的代码，而不是科研项目，具有广泛的开源社区的支持。是Python Web中的“第一开发框架~~~”

### **2.3 Django的优势**

​		强大的后台功能

​		优雅的网址设计

​		可插拔的 App 理念

​		开发效率很高

### **2.4安装Django**

​		pip install django==2.0.6

​		pip install django    则安装最新版

## **3.**  Hello Django

### **3.1**  运行流程

```python
1. 根据项目配置文件settings.py中的ROOT_URLCONF找到总路由模块的路径。

2. 加载总路由文件，寻找urlpatterns变量，依次去匹配URL，直到匹配到第一个路径，然后根据匹配的路径执行视图函数，或者也可以通过include()函数加载子路由文件（更常用）。

3. 子路由文件中取寻找urlpatterns变量，匹配路径，寻找对应的视图函数。
```

### **3.2**  Hello,Django跑起来

### **3.3**  启动开发服务器

```python
几种常见的启动方式:

1. 绑定本地端口号为8000

python manage.py runserver

1. 绑定本地端口号为8888

python manage.py runserver 8888

1. 绑定(监听)某个主机端口号为8888

python manage.py runserver ip地址:8888

注意：如果想要任何主机都能问，则settings.py中 

ALLOWED_HOSTS = ['*',]
```

 

### **3.4命令行创建项目与应用**

```python
命令行创建项目：

django-admin.py startproject 项目名称

命令行创建应用：

python manage.py startapp app名称
```

 

## **4. 路由的配置**

### **4.1  通过path()函数配置(通过路由匹配路径)**

### **4.2  通过re_path()函数配置（通过正则匹配路径）**

通过re_path()函数配置（通过正则匹配路径）

### **4.3 URL动态传参**

```python
1.通过在path()函数中的第一个url参数,捕获客户端传递的参数，也就是匹配URL中添加"<参数名>"动态捕获参数。

2.如果通过re_path()函数捕获参数，则通过命名分组的方式捕获参数，(?P<参数名称>正则表达式)视图函数中的形式参数名与URL动态捕获的参数名一致
```

###  **4.4  设置额外参数**

```python
以字典形式传递参数到视图函数,字典的key与视图函数的形式参数名一致

path('路由',视图函数名,{key:value})

re_path('路由',视图函数名,{key:value})
```

​	

### **4.5  路径转换器**

```python
URL捕获到参数后，常用的转换器：

str 匹配除路径分隔符之外的任何非空字符串,并将参数转换为字符串，这个是默认转换器

int 匹配零或任何正整数。返回一个int

uuid：匹配格式化的uuid，转换为UUID类型的对象，如 075194d3-6885-417e-a8a8-6c931e272f00

注：uuid是全局唯一标识符（univeral unique identifier），通常用32位的一个字符串的形式来表现。有时也称guid(global unique identifier)。python中自带了uuid模块来进行uuid的生成和管理工作

path，匹配任何非空字符串，包含路径分隔符（/）

slug 可理解为注释、后缀或附属，常作为URL的解释性字符。可匹配任何ASCII字符以及连接符和下划线，能使URL更加清晰易懂，比如网页的标题是"13岁的孩子"，其URL地址可以设置为"13-sui-de-hai-zi"。
```

 

### **4.6 django1.x的url()函数也可以配置路由**

```python
url()函数是Django1.x的用法

url(正则表达式,include()函数)

或者

url(正则表达式,视图函数)
```



## **5.  视图函数**

### **5.1  视图函数的第一个位置参数为请求对象，可以接收URL传递过来的参数**

### **5.2 视图函数加载模板**

```python
1.视图函数通过返回render(request,'模板路径')函数加载模板

2.视图函数也可以通过render()函数向模板传递字典,模板接收到数据后可以通过{{ 模板变量名 }}显示数据，模板变量名必须与字典中的key一致。
```



## **6.  获取请求信息**

### **6.1  请求信息存放在视图函数的参数request中**

### **6.2  request的常用属性**

​		

```python
COOKIES

获取客户端Cookie信息

FILES

字典对象，包含所有的上传文件

GET

获取GET请求的请求参数

POST

获取POST请求中请求体中的请求参数

method

获取该请求的请求方式

path

获取请求路径

user

获取当前请求的用户信息

META

获取客户端的请求头信息，eg: request.META.get("REMOTE_ADDR")获取客户端的IP地址
```



## **7.JsonResponse**

### **7.1这个类是HttpResponse的子类**

### **7.2它的默认Content-Type 被设置为： application/json**

第一个参数，data应该是一个字典类型，当 safe 这个参数被设置为：False ,那data可以填入任何能被转换为JSON格式的对象，比如list, tuple, set。 默认的safe 参数是 True. 

# 第二章	Django基础二

## 1.模板

### 1.1模板概念

### 1.1.1Django通过模板动态地生成HTML

（1）模板的加载位置(在settings.py中TEMPLATES设置)：
  		DIRS决定模板目录的全局位置
		APP_DIRS决定是否在应用的目录中寻找模板
（2）模板引擎：
		Django框架常使用Django模板引擎

## 1.2  模板变量 

（1）语法{{ 模板变量名 }}显示模板变量
（2）点语法访问复杂对象

```
	字典
	对象(属性、无参方法,除了self参数)
	列表(Django模板引擎不支持负数索引)
```

（3）locals()函数返回一个字典，字典中包含的是当前局部变量的内容

## 1.3  模板标签

### if标签：

```python
1.简单if标签

     {% if 布尔值 %}

          布尔值为True时，显示的内容

     {% else %}

            布尔值为False时，显示的内容

     {% endif %}
注意：可以在if后添加and、or、not，逻辑判断符号。判断是否相等，使用"=="符号。

2.多分支if标签：

      {% if score >= 90 %}

           优秀

        {% elif score >= 80 %}

           良好

        {% elif score >= 60 %}

           一般

        {% else %}

           不及格

        {% endif %}
```

### for标签

```python
{% for 循环变量 in 循环序列 %}

           {{ 循环变量 }}

        {% empty %}

           如果循环序列为空，执行此处

{% endfor %}



注意：使用for标签循环时，有一个forloop模板变量，用来记录当前循环进度的。

forloop的常用属性：counter、first、last。

      如果要进行反向循环，则在for标签的循环序列后加上reversed。



用法举例：

forloop.last 是一个布尔值；在最后一次执行循环时被置为True。一个常见的用法是在一系列的链接之间

放置管道符（|）



{% for link in links %}

{{ link }}

{% if not forloop.last %} | {% endif %}

{% endfor %}



另一个常见的用途是为列表的每个单词的加上逗号

{% for p in places %}

   {{ p }}

   {% if not forloop.last %}, {% endif %}

{% endfor %}
```

### include标签

```python
include标签常用来包含那些固定不变的模板

{% include '包含的模板地址' %}
```

### 模板继承标签

```python
父模板（基模板）提前定义若干个block块标签，子模板可以继承、重写、添加父模板block块中的内容。

       子模板通过

  {% extends '父模板位置' %}继承父模板。

       父模板使用

          {% block 块名称 %} 

          {% endblock %}

      进行“挖坑”，子模板“填坑”。

       子模板如果想要在父模板的某个块中添加内容，则先要使用

{{ block.super }}获取父模板中该块的内容。
```



## 1.4  模板过滤器

作用：在变量被显示前修改它的值
使用方法：模板变量后面添加管道符|，管道符后是过滤器名称

### 1.4.1常用模板过滤器

```python
常用模板过滤器：

1. length 获取模板变量对应值的长度

2.first 获取第一个元素

3.upper 变为大写

4.lower 变为小写

5.truncatewords:"单词数" 截取指定数量的单词，该过滤器需要参数  （会出现三个点）

6.truncatechars:"字符数" （会出现三个点）

7.date 显示日期，该过滤器需要参数，eg:    date:"Y-m-d H:i:s"



8.addslashes 在单引号、双引号、反斜线前添加斜线

9.add:"添加的数字"  在模板变量上添加指定的数据

10.capfirst 使首字母大写

11.safe 不会对特殊字符转义

   eg:  传递过来的字符串是：

         info = "<h3>这是传递的info信息</h3>"

         则，在模板中如果使用safe过滤器：

         {{ info|safe }}

        显示结果，会将h3当做3号标题显示。
```

### 1.4.2自定义过滤器

```python
自定义过滤器的步骤：
1.在app目录下建立templatetags包(包含__init__文件)，并在此包下建立自己的过滤器模块(例如：myfilter.py)

2.在自定义的过滤器模块中编写：

from django import template

register = template.Library()

并使用装饰器@register.filter装饰一个函数,这个函数就是过滤器的具体调用，第一个参数是传递过来的模板变量；如果需要对该过滤器设置参数，还需要传递第二个参数

3.在模板中加载过滤器模块

  {% load myfilter %}
 
```

## 2.url反向解析

### 2.1 URL反向解析作用：

反向解析是动态获取URL的一种方式，使用反向解析可以避免在模板和视图函数中硬编码路径，易于扩展和维护

### 2.2  模板中使用反向解析的步骤

（1）在include()函数中设置命名空间，解析第一部分URL
			include('子路由模块路径',namespace="在此设置命名空间")
（2）在path()函数的name关键字参数中指定名称，解析第二部分URL
			path('路由地址/',视图函数名,name="在此设置name")
（3）在子路由(include包含的urls.py)中设置app_name
			app_name = "最好设置为应用名"
（4）在模板中使用反向解析
			{% url 'namespace名称:name名称'  参数1 参数2 ... %}

### 2.3视图函数中使用反向解析

使用reverse()函数
			不传参： reverse("namespace:name")
			传参：reverse("namespace:name",args=(参数值,))或reverse("namespace:name",kwargs=字典)

## 3.两种跳转方式

### 3.1服务端跳转（转发）

（1）将客户端发送的请求在后台服务器进行传递转发。请求参数可以在后台传递，浏览器的URL地址不变
（2）实现服务端跳转：返回HttpResponse；返回render()

### 3.2  客户端跳转（重定向）

（1）客户端向服务端发送两次不同的请求，浏览器的URL地址改变
（2）实现客户端跳转
			HttpResponseRedirect('重定向的URL')
			redirect()与reverse()结合使用

# 第三章 Django基础三

## 1.  静态资源

### 1.1 使用步骤

（1）在模版中添加{% load static %}标签，加载static模块
（2）使用{% static "地址" %}动态生成静态资源URL

```
注意：{% static "地址"%}中的地址为：从第一部分路由对应的目录开始拼接的地址
```

### 1.2  重要配置

（1）STATIC_URL:静态资源的虚拟路径的第一部分(前缀路由),会启动“静态资源探测器”
（2）STATICFILES_DIRS

```
“静态资源探测器”会首先去查找STATICFILES_DIRS配置里设置的目录下的静态资源；然后会去查找每个app下的static子目录下的静态资源
eg:
STATICFILES_DIRS = [  
os.path.join(BASE_DIR,'staticresource'),
]
```

（3）STATIC_ROOT

```python
可以通过"python manage.py collectstatic"命令将所有应用的静态资源收集到STATIC_ROOT指向的目录中
eg:  STATIC_ROOT = os.path.join(BASE_DIR,'目录名')
```

##  2. Model类

### 2.1  ORM思想:Object Relationship Mapping,对象关系映射

### 2.2  实现方式：继承Model类(django.db.models.Model)

（1）模型类对应于数据库表，类属性对应于表字段

```python
from django.db import models

class Student(models.Model):   

    name = models.CharField(max_length=20)  

    sex = models.CharField(max_length=10)

    score = models.FloatField()

    class Meta:

        db_table = "students"   #   指定模型类对应的表名称
```

（2）创建好模型后，迁移（同步）到数据库

```
   1. 制作迁移计划

   python manage.py makemigrations 

   2. 执行迁移任务
   python manage.py migrate [应用名]
```

（3）如果缺少mysqlclient包（Django使用它连接mysql），则迁移失败，可以通过轮子文件安装该包

### 2.3  数据库配置信息（在settings.py中配置）

```python
DATABASES = {

    'default': {

        'ENGINE': 'django.db.backends.mysql',

        'NAME': 'mydb',

        'HOST':'localhost',

        'PORT':3306,

        'USER':'root',

        'PASSWORD':'123456',
    }

}
```

## 3.  操作Model类

### 3.1 进入shell环境：python manage.py shell

### 3.2  添加记录

方式一：通过模型类的对象管理器插入记录
模型类.objects.create(类属性=值)

```
演示Student模型：

添加记录：

方式一：通过模型类的对象管理器添加

stu1 = Student.objects.create(name='令狐冲',sex='男',score=92.5)


```

方式二：实例化模型对象，并save()

```
实例化模型对象，并save()

stu4 = Student(name="小龙女",sex='女',score=89)

stu4.save()
```

### 3.3 修改记录、删除记录

```
修改记录：

调用模型对象的save()方法，前提是该模型对象在数据库中已经存在关联的记录

QuerySet的update()方法可以更新多条记录：

>>> from django.db.models import Q

>>> q1 = Q(price__gte=60)

>>> q2 = Q(price__lte=40)

>>> books = Book.objects.filter(q1|q2)

>>>books.update(price=F("price")+100)

删除记录：

调用模型对象的delete()方法

删除表中全部数据

以学生模型为例：

Student.objects.all().delete()

删除多条记录：

Student.objects.filter(过滤条件).delete()
```

### 3.4  查询

#### 3.4.1  简单查询

（1）查询单挑记录

模型类名.objects.get(查询条件)

[^get()方法只允许返回一条记录，否则报错]: 

```python
get()返回一条记录：

查询主键为1的记录(返回的就是模型对象)

stu = Student.objects.get(id=1)

也可以通过pk关键字参数，直接查询某主键值对应的记录：

student = Student.objects.get(pk=1)

```

（2）查询所有记录

模型类名.objects.all()

```
查询所有学生：

students = Student.objects.all()

返回Django中的QuerySet对象，该对象是个容器对象，可以被遍历、切片、或者通过索引获取某个对象。
```

（3）查询某个（些）字段

方式一： 模型类名.objects.values("属性名")或模型类名.objects.values("属性名1","属性名2",...)

返回QuerySet容器对象，其中的元素以字典格式表示

```
也可以通过QuerySet进行调用values()

students = Student.objects.filter(score__gte=70).values('name','score')

```

方式二：模型类名.objects.values_list("属性名")或模型类名.objects.values_list("属性名1","属性名2",...)

返回QuerySet容器对象，其中的元素以元组格式表示

```
也可以通过QuerySet进行调用values_list()

students = Student.objects.filter(score__gte=70).values_list('name','score')
```

#### 3.4.2  过滤查询

（1）模型类名.objects.filter(查询条件)

```
过滤查询操作：

1.查询性别为“男”的学生

students = Student.objects.filter(sex='男')

返回Django中的QuerySet容器

2.查询姓名为“令狐冲”，性别为“男”的学生

students = Student.objects.filter(sex='男',name="令狐冲")
```

（2）使用魔法查询

```
字段后面跟双下划线"__"，表示特殊查询

常用的双下划线魔法参数有：

 __year     __month   __day   __startswith  __endswith    __gte  __lte  __gt  __lt  __contains (相当于模糊查询)

1. 名字中以"郭"开头的学生记录

  students = Student.objects.filter(name__startswith='郭')

2.名字中包含"郭"的学生记录

  students = Student.objects.filter(name__contains='郭')

3. 查询成绩大于等于85分的学生记录

   students = Student.objects.filter(score__gte=85)
```

#### 3.4.3  排除查询

模型类名.objects.exclude(查询条件)

```
查询分数小于85的学生记录

students = Student.objects.exclude(score__gte=85)
```

#### 3.4.4  限制查询

通过对查询结果进行"切片"完成限制查询

```
1.查询所有学生的第2、3记录

students = Student.objects.all()[1:3]

2.查询前两个男生

students = Student.objects.filter(sex="男")[:2]

3.查询所有学生的第1、3、5...条记录

students = Student.objects.all()[::2]
```

#### 3.4.5  排序查询

模型类名.objects.order_by("排序字段名")

```
1.对所有学生按照成绩降序排序

  students = Student.objects.order_by("-score")

2. 查询成绩为前三名的学生记录

   topstudents = Student.objects.order_by("-score")[:3]

还可以根据多个字段进行排序，比如：先根据成绩降序排序，如果成绩相等，则再根据年龄升序排序；

students = Student.objects.order_by("-score","age") 
```

#### 3.4.6  原生SQL查询

模型类名.objects.raw("原生SQL")
	（1）原生模糊查询
		eg:   select * from person where name like '%%o%%'

```
使用原生查询，查询名字中包含“郭”的记录

students = Student.objects.raw("select * from students where name like '%%郭%%'")
```

​	（2）原生SQL中使用占位符

```
1.使用原生SQL查询名字为“郭达”，性别为“男”的学生

使用元组填值：

 students = Student.objects.raw("select * from students where name='%s' and sex='%s'"%('郭达','男'))
 
使用列表填值：

students = Student.objects.raw("select * from students where name=%s and sex=%s",['郭达','男']  
```

​	（3）查询字段必须包含主键

#### 3.4.7  Q查询

​	（1）对于复杂组合查询条件，使用Q查询很方便
​	（2）通过实例化Q对象，将Q对象传递到filter()、get()方法中可以组合出多种查询条件
​	（3）Q查询依赖于Q对象(from django.db.models import Q)
​	（4）Q对象之间可以使用&、|。在Q对象前加~表示Q封装条件的否定条件

```
from django.db.models import Q

1.使用Q查询男生中90分以上的学生记录

 q1 = Q(sex='男')

 q2 = Q(score__gte=90)

students = Student.objects.filter(q1&q2)

2. 使用Q查询姓“郭”或者成绩大于等于90分的学生记录

q1 = Q(name__startswith='郭')

q2 = Q(score__gte=90)

students = Student.objects.filter(q1|q2)

3.使用Q查询名字中不包含“郭”的学生

 q = Q(name__contains='郭')

 students = Student.objects.filter(~q)

```

#### 3.4.8  F查询

F()对象生成一个SQL表达式，来描述数据库层级(而不是程序层级)所需要的操作,与更新操作相关

```
使用F对象更新学生成绩，将某个学生的成绩在原有基础上（数据库层级）加5分

stu = Student.objects.get(id=6)

stu.score = F("score") + 5

stu.save()

>>> from django.db.models import Q

>>> q1 = Q(price__gte=60)

>>> q2 = Q(price__lte=40)

>>> books = Book.objects.filter(q1|q2)

>>>books.update(price=F("price")+100)

```

#### 3.4.9  聚合查询

aggregate(*args,**kwargs)

```
1.查询所有学生的平均成绩：

avg_score = Student.objects.all().aggregate(Avg("score"))

结果是一个字典

还可以自定义结果字典的key:

avg_score = Student.objects.all().aggregate(avgscore=Avg("score"))

2.查询班级成绩最高的学生成绩

max_score = Student.objects.all().aggregate(scoremax=Max("score"))

或者：

max_score = Student.objects.aggregate(scoremax=Max("score"))
```

可能要导入的：from django.db.models import Min,Avg,Max,Sum,Count

#### 3.4.10  分组查询

annotate(*args,**kwargs)  

```
根据性别分组，查询男女生的平均成绩

students = Student.objects.values('sex').annotate(Avg('score'))

也可以自定义每一组成绩的key
Student.objects.values("sex").annotate(stu_score=Avg("score"))
结果为：<QuerySet [{'sex': '男', 'stu_score': 83.33333333333333}, {'sex': '女', 'stu_score': 68.0}]
```

## 4.  自定义管理器

继承Manager类(from django.db.models import Manager)

```
在自定义管理器中实现自己的方法

例如：

class CakeManager(Manager): # 自定义对象管理器

        def create_cake(self,name,price,color):

            cake = self.model( )

            cake.name = name

            cake.price = price

            cake.color = color

            cake.save()

            return cake

class Cake(models.Model):

        name = models.CharField(max_length=20)

        price = models.FloatField()

        color = models.CharField(max_length=20)

        cakemanager = CakeManager()  # 关联自定义对象管理器
```

# 第四章  Django原生SQL查询

## 1.  extra(  )方法

### 1.1  extra()方法的常用参数

（1）select参数
	select 参数在 select查询中添加其他字段信息，它应该是一个字典，存放着属性名到 SQL 从句的映射，结果  集中每个对象都会有一个额外的属性

```
class Emp(models.Model):   # 员工模型

    name = models.CharField(max_length=20)

    age = models.IntegerField()

    sex = models.CharField(max_length=10)

    salary = models.FloatField()

    is_married = models.BooleanField()  # 婚否

    class Meta:

        db_table = 'emp'

1. 查询所有员工信息，并添加一个额外字段，判断每个员工是否属于高工资

  emps = Emp.objects.extra(select={"is_high_salary":"salary>=5000"})

对应的底层SQL是：

SELECT (salary>=5000) AS `is_high_salary`, `emp`.`id`, `emp`.`name`, `emp`.`age`, `emp`.`sex`, `emp`.`salary`, `emp`.`is_married` FROM `emp` LIMIT 21
```

（2）where参数
指定查询条件

```
题目一：

查询性别为'女'，并且年龄大于等于25

1.使用对象管理器调用extra()

emps = Emp.objects.extra(where=["sex='女'","age>=25"])

底层SQL是：

"SELECT `emp`.`id`, `emp`.`name`, `emp`.`age`, `emp`.`sex`, `emp`.`salary`, `emp`.`is_married` FROM `emp` WH

ERE (sex='女') AND (age>=25) LIMIT 21"

2.使用QuerySet对象调用extra()

emps = Emp.objects.filter(sex='女').extra(where=['age>=25'])

题目二：

查询性别为女，或者年龄小于20岁

emps = Emp.objects.extra(where=["sex='女' or age<20"])

题目三：

查询性别为男，并且对筛选出的所有男性添加一个额外字段is_high_salary

emps = Emp.objects.extra(where=["sex='男'"],select={"is_high_salary":"salary>=5000"})


```

### 1.2  QuerySet对象.extra(参数)

​	students = Student.objects.filter(name__contains='郭').extra(where=['score>55','age>50'],select={"key":"子查询SQL"})

### 1.3  模型类名称.objects对象管理器.extra(参数)

​	Student.objects.extra(select={'count':'select count(*) from students'})

## 2.  raw( )方法

### 2.1  模型类名称.对象管理器.raw(原生sql)

​	books = Book.objects.raw("select id,name,price from books")
​	查询字段必须包含主键

### 2.2  模型类名.objects.raw("原生SQL")

​	（1）原生模糊查询
​	eg:   select * from person where name like '%%o%%'

```
使用原生查询，查询名字中包含“郭”的记录

students = Student.objects.raw("select * from students where name like '%%郭%%'")
```

​	（2)  原生SQL中使用占位符

```
1.使用原生SQL查询名字为“郭达”，性别为“男”的学生

使用元组填值：

 students = Student.objects.raw("select * from students where name='%s' and sex='%s'"%('郭达','男'))

使用列表填值：

students = Student.objects.raw("select * from students where name=%s and sex=%s",['郭达','男'])  
```

​	(3)  查询字段必须包含主键

## 3.  execute(  )方法

​	类似于pymysql的用法，灵活度很高,并且完全不依赖Model类
​	用法:

```
from django.db import connection

 cursor = connection.cursor()

 cursor.execute("select name,price from books")

 books = cursor.fetchall() # 包含结果集的元组
```

# 第五章  模型基本关系

## 1.  “一对多”关系

### 1.1 实现方式

通过在"多"方模型类中添加外键属性ForeignKey

```python
class School(models.Model):   # "一"方模型

    name = models.CharField(max_length=30)

    address = models.CharField(max_length=30)

    history = models.TextField(null=True)

class Student(models.Model):  # "多"方模型

    name = models.CharField(max_length=10)

    age = models.IntegerField()

    score = models.FloatField()

    sex = models.CharField(max_length=10)

    school = models.ForeignKey(School,on_delete=models.CASCADE)

补充：

   models.ForeignKey()中也可以添加related_name，如果设置了related_name，则通过“一”方查询多方，使用“一方模型对象.related_name名称.all()”
```

### 1.2  对象间的关联

​	方式一：通过"多"方模型的外键类属性关联"一"方模型的实例化对象 

```
添加学校对象（“一”方模型对象）

school1 = School.objects.create(name='清华大学',address='北京中关村')

school2 = School.objects.create(name='北京大学',address='北京',history='北京大学是一所知名大学')

school3 = School.objects.create(name='西安交通大学',address='西安',history='交大很好')

通过外键类属性关联：

stu1 = Student.objects.create(name='张三',age=20,score=93.5,sex='男',school=school1)
```

​	方式二：通过“多”方对应表的外键关联"一"方

```
通过“多”方对应表的外键关联

eg:两个学生都上的是3号学校

stu3 = Student.objects.create(name='郭靖',age=23,score=80,sex='男',school_id=3)

stu4 = Student.objects.create(name='黄蓉',age=21,score=85,sex='女',school_id=3)
```

### 1.3  查询

#### 1.3.1  从"一"查"多"

一方的实例化对象.多方模型类名小写_set.all()；
如果在ForeignKey属性添加了related_name参数，则为：一方的实例化对象.related_name指定的名称.all()

```
查询1号学校所有的学生：

school = School.objects.get(id=1)

students = school.student_set.all()
```

#### 1.3.2  从"多"查"一"

通过“多”方设置的关联类属性查询

```python
查询6号学生对应的学校：

 student = Student.objects.get(id=6)

 school = student.school
```

## 2.  “一对一”关系

### 2.1  实现方式

在负责维护关系的“一”方添加OneToOneField型的类属性

```python
from django.db import models

class Person(models.Model):

    name = models.CharField(max_length=20)

    age = models.IntegerField()

    sex = models.CharField(max_length=10)

class Card(models.Model):

    cardno = models.CharField(max_length=20,unique=True)   # 卡号类属性

    color = models.CharField(max_length=10)

    person = models.OneToOneField(Person,on_delete=models.CASCADE)  # 使用OneToOneField进行“一对一”关联
```

### 2.2  对象间关联

方式一：通过主动一方模型类属性，关联另一方对象

```
创建人对象：

per = Person(name='张三',age=20,sex='男')

per.save()

通过Card类的person属性关联

card = Card(cardno='zs123456',color='绿卡',person=per)

 card.save()
```

方式二：通过对应表的唯一外键字段关联

```
per1 = Person(name='李四',age=22,sex='男')

 per1.save()

card1 = Card.objects.create(cardno='ls123456',color='黄色',person_id=per1.id)
```

### 2.3  查询

(1)从维护关系的"一"方查询
使用模型类中维护关系的那个类属性

```
查询3号卡关联的人

 card = Card.objects.get(id=3)

 per = card.person  # 使用关联的类属性查询
```

(2)从不维护关系的"一"方查询
使用另一个一方模型类名的小写（没有添加related_name参数）；
如果在维护关系的“一方”添加了related_name关键字参数，则使用related_name参数值查询

```
查询1号人的卡：

per1 = Person.objects.get(id=1)

card1 = per1.card  # 使用对方模型类名的小写
```

## 3.  “多对多”关系

### 3.1  三个模型实现"多对多"

#### 3.1.1  实现方式

在某个"多"方使用ManyToManyField，关联另一个"多"方和第三方模型维护关系

```python
from django.db import models

class Member(models.Model):   # 成员模型

    name = models.CharField(max_length=20)

    age = models.IntegerField()

    sex = models.CharField(max_length=10)

    def __str__(self):

        return self.name

class Community(models.Model):  # 社团模型

    name = models.CharField(max_length=20)

    buildtime = models.DateField()

    members = models.ManyToManyField(Member,through="Relation")

    def __str__(self):

        return self.name

class Relation(models.Model):

    member = models.ForeignKey(Member,on_delete=models.CASCADE)

    community = models.ForeignKey(Community,on_delete=models.CASCADE)

    join_reason = models.CharField(max_length=100)
```

#### 3.1.2  对象间关系

通过第三方模型对象维护

```python
创建Member对象：

member1 = Member.objects.create(name='马小跳',age=20,sex='男')

member2 = Member.objects.create(name='李丽丽',age=25,sex='女')

member3 = Member.objects.create(name='黄大牛',age=35,sex='男')

创建Community对象：

community1 = Community.objects.create(name='天涯吉他社',buildtime=date(2016,6,6))

community2 = Community.objects.create(name='读书会',buildtime=date(2017,10,1))

 community3 = Community.objects.create(name='瑜伽协会',buildtime=date(2008,9,3))

创建Relation关系对象，通过实例化对象关联：

r1 = Relation.objects.create(member=member2,community=community1,join_reason='好玩')

通过外键值关联：

r2 = Relation.objects.create(member_id=member3.id,community_id=community1.id,join_reason='交朋友')
```

#### 3.1.3  查询

(1)  从主动维护关系的"多"方查询
实例化对象.通过关联的类属性.all()

```python
查询3号社团所有的成员：

方式一：借助中间模型查询：

community = Community.objects.get(id=3)

relations = Relation.objects.filter(community=community)

 for r in relations:

      print(r.member)

方式二：借助关联的类属性直接查询

all_members = community.members.all()

for m in all_members:

     print(m)
```

(2)从不负责维护关系的"多"方查询
实例化对象.对方模型类名小写_set.all()

```
查询1号成员参加的社团：

member1 = Member.objects.get(id=1)

all_community = member1.community_set.all()

for c in all_community:

     print(c)
```

### 3.2  两个多方模型实现"多对多"

在某个"多"方模型中，通过ManyToManyField()生成的类属性关联另一个"多"方模型

```python
from django.db import models

class User(models.Model):  # “用户”模型

    username = models.CharField(max_length=20)

    password = models.CharField(max_length=20)

    sex = models.CharField(max_length=10)

    class Meta:

        db_table = "users"

class Group(models.Model):   # “组”模型

    groupname = models.CharField(max_length=20)

    users = models.ManyToManyField(User,related_name="yourgroups")



    class Meta:

        db_table = "groups"

操作：

先添加用户：

 user1 = User.objects.create(username='tom',password='123',sex='男')

user2 = User.objects.create(username='jerry',password='456',sex='男')

user3 = User.objects.create(username='alice',password='789',sex='女')

再添加组：

g1 = Group.objects.create(groupname='吉他组')

g2 = Group.objects.create(groupname='街舞组')

设置关系：

1. user1用户主动加入g1组：

   user1.yourgroups.add(g1)

2. g1组主动添加user3用户

   g1.users.add(user3)

3. user3主动加入g2组

   user3.yourgroups.add(g2)

4. g1组主动删除user1用户

   g1.users.remove(user1)

5. user3主动退出g1组

   user3.yourgroups.remove(g1)

6.查询g1组的所有用户：

    for u in g1.users.all():

         print(u)

7. 查询user3加入了哪些组：

    for g in user3.yourgroups.all():

         print(g)

8. 将g2组与其所有成员解除关系

    g2.users.clear()

9. user3主动退出所有组：

    user3.yourgroups.clear()
```



## 4.  模型外键自关联

### 4.1  场景：在需要引用自身模型的时候

### 4.2  引用方式

#### (1)"一对多"自引用

​		models.ForeignKey('self',on_delete=models.CASCADE,null=True)

```
from django.db import models

class Emp(models.Model):

    name = models.CharField(max_length=20)

    age = models.IntegerField()

    sex = models.CharField(max_length=10)

    salary = models.FloatField()

    manager = models.ForeignKey('self',on_delete=models.CASCADE,null=True)   # 外键自引用

创建最高领导boss和其他员工：

boss = Emp.objects.create(name='马云',age=55,sex='男',salary=3000.5)

emp1 = Emp.objects.create(name='马晓云',age=25,sex='男',salary=2000,manager=boss)

emp2 = Emp.objects.create(name='张勇',age=58,sex='男',salary=6000,manager=boss)

emp3 = Emp.objects.create(name='张三',age=58,sex='男',salary=3000,manager=emp2)

emp4 = Emp.objects.create(name='张丽',age=28,sex='女',salary=3000,manager=emp3)

emp5 = Emp.objects.create(name='张小勇',age=22,sex='男',salary=3000,manager=emp2)

查询id为3的员工的直接下属与直属上司：

emp = Emp.objects.get(id=3)

workers = emp.emp_set.all()  # 直属下属

leader = emp.manager

级联删除所有员工：

boss.delete()
```

#### (2)  “一对一”自引用

​		models.OneToOneField('self',on_delete=models.CASCADE,null=True)

```
from django.db import models

class Spy(models.Model):

    name = models.CharField(max_length=20)

    age = models.IntegerField()

    sex = models.CharField(max_length=10)

    manager = models.OneToOneField('self',on_delete=models.CASCADE,null=True)



    class Meta:

        db_table = 'spies'

创建Spy对象boss,并创建其他对象

boss = Spy.objects.create(name='戴笠',age=45,sex='男')

spy1 = Spy.objects.create(name='毛人凤',age=30,sex='男',manager=boss)

spy2 = Spy.objects.create(name='张三',age=25,sex='男',manager=spy1)

查询boss的直属下属的名字：

boss.spy.name
```

#### (3)"多对多"自引用

​		不能使用ManyToManyField,否则报错：(fields.E332) Many-to-many fields with intermediate tables must not be symmetrical
​		关注功能实现

```python
from django.db import models

class WeiUser(models.Model):

    username = models.CharField(max_length=20) 

class WeiRelation(models.Model):

    # 关联"被关注者"

    followed = models.ForeignKey(WeiUser,on_delete=models.CASCADE,related_name="followed_relations")

    # 关联"粉丝"

    fans = models.ForeignKey(WeiUser,on_delete=models.CASCADE,related_name="fans_relations")

添加用户：

u1 = WeiUser.objects.create(username='二狗')

u2 = WeiUser.objects.create(username='张学友')

u3 = WeiUser.objects.create(username='小芳')

u4 = WeiUser.objects.create(username='小刚')

u5 = WeiUser.objects.create(username='翠花')

建立关系：

r1 = WeiRelation.objects.create(fans=u1,followed=u2)

r2 = WeiRelation.objects.create(fans_id=1,followed_id=3)

r3 = WeiRelation.objects.create(fans=u4,followed=u3)

r4 = WeiRelation.objects.create(fans=u4,followed=u5)

r5 = WeiRelation.objects.create(fans=u5,followed=u4)

查询4号用户关注了哪些用户？

user = WeiUser.objects.get(pk=4)

relations = user.fans_relations.all()

for r in relations:

     print(r.followed.username)

查询谁是4号用户的粉丝？

user = WeiUser.objects.get(pk=4)

relations = user.followed_relations.all()

>>> for r in relations:

             print(r.fans.username)

4号用户取消对3号用户的关注

 r = WeiRelation.objects.filter(fans_id=4,followed_id=3).first()

r.delete()python
```

# 第六章  Django基础四

## 1.  CSRF

（1）跨网站请求伪造(Cross-site request forgery)：攻击者盗用了你的身份，以你的名义发送恶意请求。是一种对网站的恶意利用
（2）开启CSRF中间件CsrfViewMiddleware；Django表单防御crsf：在表单中使用{% csrf_token %}标签，可以动态生成token令牌
（3）Django的CSRF防护原理

```
   (1)在用户访问网站时，Django在网页的表单中生成一个隐含字段csrfmiddlewaretoken,这个值是在服务器端随机生成的；

   (2)当用户提交表单时，服务器校验表单的csrf的token是否和自己保存的csrftoken一致，用来判断当前请求是否合法；

   (3)如果用户被CSRF攻击并从其他地方发送攻击请求，由于其他地方不可能知道隐藏的csrftoken信息，因此导致网站后台校验csrftoken失败，攻击就被成功防御。

   注：CSRF防护只作用于POST请求，并不防护GET请求，因为GET请求以只读形式访问网站资源，并不破坏和篡改网站数据。
```

（4）取消csrf防护
在相应的视图函数上添加装饰器@csrf_exempt,则该视图函数不经过CSRF中间件的验证

## 2.  Cookie、Session与Token

### 2.1  Cookie

Cookie是服务端创建，但保存于客户端，客户端每次发送请求时都会将Cookie信息发送到服务器(因为Cookie是请求头信息的一部分)
Cookie常用操作

```python
HttpResponse对象操作

response.set_cookie("Cookie名称","Cookie值",max_age=保存时间秒数)

获取Cookie

request.COOKIES.get("cookie名称")

获取所有Cookie,以字典形式返回

request.COOKIES

删除Cookie: 

response.delete_cookie("cookie名称")
举例：

def add_cookie_view(request):
    response = HttpResponse("<h4>Cookie成功添加~~~~</h4>")
    response.set_cookie("sport","football")
    response.set_cookie("fruit","apple",max_age=60)  #设置Cookie的保存时间为60秒
    return response

def show_cookies_view(request):
    html = ""
    for k,v in request.COOKIES.items():
        html += k
        html += "=========>"
        html += v
        html += "<br/>"
    return HttpResponse(html)

def get_cookie_view(request,name):
    cookie_value = request.COOKIES.get(name,"没有该Cookie!")
    result = "cookie名称为" + name + "对应的值为：" + cookie_value
    return HttpResponse(result)

def delete_cookie_view(request,name):
    response = HttpResponse("<h4>名称为"+name+"的Cookie已经删除!</h4>")
    response.delete_cookie(name)
    return response

```

### 2.2 Session

(1) Session是用来表示一个用户与服务端的一次“会话”。
(2)原理：Django的Session数据默认存储于服务器端的数据库表中(django_session表)。使用客户端发送的sessionid（存在与Cookie中）与服务端的sessionid匹配，找到客户端请求所属的“会话”，经常用于登录验证。
(3)Session常用操作

```

Session的数据类型类似于字典：

添加session:   request.session["属性名"] = 属性值

获取键为k1对应的值，若k1不存在则报错：

request.session['k1']

获取键k1对应的值，若k1不存在则返回空字符串：

request.session.get('k1','')

获取sessionid:

request.session.session_key

删除session中的某属性： del request.session["属性名"]

退出登录，cookie与session一起删掉：

 request.session.flush()

```

(4)Session重要配置

```
1. SESSION_EXPIRE_AT_BROWSER_CLOSE = True # 当浏览器关闭时，清除本地sessionid的那个Cookie

注意：该属性与浏览器内核相关

2. SESSION_COOKIE_AGE = 60 # 设置session保存的秒数
```

(5)Session的保存		

1) 在Django中，Session默认使用数据库保存

2) 若想变更Session的保存方式，需要添加SESSION_ENGINE配置信息

3) 可将Session保存到Redis中

```
一：Redis在windows的安装与配置：

解压ZIP，在命令行中输入以下命令启动Redis

redis-server redis.windows.conf   

若要设置Redis可以被远程访问：

redis.windows-service.conf中注释bind变量。

测试访问：

redis-cli -a 密码 -h ip地址 -p 6379

若想给Redis添加密码，则将配置文件redis.windows.conf的requirepass配置放开,并设置密码

二：将Session保存在Redis中的方法：

1.  安装django-redis-sessions库

pip install django-redis-sessions

2.在settings.py中配置

SESSION_ENGINE = 'redis_sessions.session'

SESSION_REDIS = {

    'host':'Redis所在的主机',

    'port':6379,

    'db':0,

    'password':'Redis密码',

    'prefix':"key前缀",

    'socket_timeout':5

}
```

### 2.3  Token

(1)Token，就是令牌，最大的特点就是随机性，不可预测。一般黑客或软件无法猜测出来
(2)Token的使用场景
1)  防止csrf攻击（跨站点请求伪造）

```
当客户端请求页面时，服务器会生成一个随机数Token，并且将Token存储于服务器端中，然后将Token发给客户端（一般通过构造hidden表单）。下次客户端提交请求时，Token会随着表单一起提交到服务器端

如果应用于“csrf攻击”，则服务器端会对Token值进行验证，判断客户端发送的Token是否和服务端值相等，若相等，则可以证明请求有效，不是伪造的。
```

2) 防止表单重复提交

```
对于“防止表单重复提交”（eg:注册成功后，点击后退按钮到原来的注册页面(没有重新加载注册页面的情况)，再次点击注册按钮），服务器端第一次验证通过后，会将服务器端中的Token值更新下，若用户重复提交，第二次的验证判断将失败，因为用户提交的表单中的Token没变，但服务器端中Token已经改变了
```

3)  基于token的身份认证
token验证原理

```
Token验证原理：

(1):当第一次验证成功（用户名和密码正确），服务端对用户数据进行签名生成一个token，并将该token发送到客户端

(2):当客户端发送请求时,会携带该token到服务器端，服务端对该token进行验证（验证方式有多种，例如解密验证、与服务端存储的token对比等）

(3):当验证成功，则说明用户验证通过，否则验证失败
```

### 2.4  三者对比

(1)   Cookie使用更简洁，服务器压力更小，数据不是很安全
(2)  Session服务器要维护Session，相对安全
(3)  Token拥有Session的所有优点，自己维护略微麻烦，支持更多的终端

## 3.  Django分页器

### 3.1 分页的好处

通过分页管理多条数据，可以美化界面并能提高查询效率

### 3.2  分页器对象

from django.core.paginator import Paginator

#### 3.2.1 实例化分页器对象

paginator = Paginator(数据源,每页最多显示的条数)

#### 3.2.2  常用方法

page(页码参数):该方法返回一个Page对象，该Page对象封装了某一页的数据

#### 3.2.3  常用属性

(1)  num_pages   # 返回总的页数
(2)  page_range  # 返回从1开始的页码范围

### 3.3  页面对象

Page(可迭代对象)
常用方法
(1)  has_previous()  当前页是否有前一页
(2)  previous_page_number() 前一页的页码
(3)  has_next()  当前页是否有下一页
(4)  next_page_number()  下一页的页码

# 第七章  Django进阶一

## 1.  Django中间件

### 1.1 简介

在django中,中间件就是一个类,在请求到来和结束后,django会根据自己的规则在合适的时机执行中间件中相应的方法

### 1.2运用思想

AOP(Aspect Oriented Programming).Django中间件使用了AOP的编程思想，将独立的功能封装到中间件

```
AOP:Aspect Oriented Programming，面向切面编程。

    切面：独立于业务逻辑的某一方面的功能,eg:日志功能

    切点：在某个时机执行的代码。
```

### 1.3实现中间件的步骤

（1）在项目下，建立某个目录(或包)，在该目录（或包）下创建模块，在模块中创建一个继承MiddlewareMixin类的中间件类
（2）在自定义的中间件类中添加相关执行时机的方法
			process_request(self, request)

```
请求预处理方法，执行时机：接收到请求，但还未解析URL到指定view。
```

​			process_view(self, request, view, args, kwargs)

```
View预处理方法，执行时机：执行完request预处理函数并确定待执行的view之后，但在view函数实际执行之前。
```

​			process_response(self, request, response)

```
Response后处理方法，执行时机：在Django执行view函数并生成response之后或其他地方返回响应对象后

该方法需要return response对象
```

​			process_exception(self, request, exception)

```
Exception后处理方法：执行时机：request处理过程中出了问题并且view函数抛出了一个未捕获的异常时才会被调用
```

（3）在settings.py中的MIDDLEWARE对自定义的中间件进行注册

### 1.4  执行顺序

​		中间件的配置顺序特别重要，请求按照中间件的配置顺序执行，响应按照中间件的倒序执行

## 2.  文件上传

### 2.1 表单上传

​	（1）获取上传文件对象：upload_file = request.FILES.get("文件域名称")
​	（2）分块写入服务器

```
for chunk in upload_file.chunks():  

           目标文件.write(chunk)  # 分块写入
```

### 2.2  Ajax上传

​	使用FormData对象完成Ajax的上传

```
var formdata = new FormData(); //FormData对象

 formdata.append("上传参数名",上传数据) // 添加上传数据

            

formdata.append("csrfmiddlewaretoken",csrf值);

            $.ajax({

                url:"url地址",

                data:formdata,

                type:"POST",

                contentType:false,

                processData:false,

                success:function(data){

                    alert(data);

                }

            });

选项解释：

processData:默认情况下，通过data选项传递进来的数据，如果是一个对象(技术上讲只要不是字符串)，都会处理转化成一个查询字符串，以配合默认内容类型"application/x-www-form-urlencoded"。如果不希望转换的信息，设置为 false
```

​	上传文件所在的表单必须设置enctype="multipart/form-data"

## 3.  admin后台权限管理

​	（1）操作准备
​		1. 创建超级用户：python manage.py createsuperuser
​		2. 在后台管理注册模型：admin.site.register(模型类名称(或模型列表))
​		3. 中文风格显示   将settings.py的LANGUAGE_CODE设置为zh-Hans
​	（2）Django的权限系统是通过User(用户)、Group(组)、Permission(权限)完成的
​	（3）Permission是针对模型(Model)的权限，每个模型默认有添加、修改、删除三个权限
​	（4）用户与组是“多对多”关系
​	（5）用户与权限也是“多对多”关系
​	（6）组与权限也是"多对多"关系

# 第八章  Django进阶二  权限与权限管理

## 1.  认证管理

### 1.1  创建用户

from django.contrib.auth.models import User
	user = User.objects.create_user(username, email, password)
	该模型映射为auth_user表

### 1.2  认证用户

from django.contrib.auth import authenticate
	user = authenticate(username=username, password=password)
		该方法也会自动检查is_active标志位

### 1.3  登录

#### 1.3.1 from django.contrib.auth import login

​	（1）login向session中添加User对象的ID, 便于对用户进行跟踪
​	（2）login(request, user)
​		user=authenticate(username=username,password=password)
​		if user is not None:
​       			 login(request, user)

#### 1.3.2 判断是否登录

​	@login_required

```
修饰器修饰的view函数会先通过检查是否登录, 已登录用户可以正常的执行操作, 未登录用户将被重定向到login_url指定的位置；若未指定login_url参数, 则重定向到settings.LOGIN_URL
```

### 1.4  登出

from django.contrib.auth import logout
	logout(request)

### 1.5  获取当前发送请求的用户对象

request.user

## 2.  权限管理

### 2.1 创建用户（见认证部分）

### 2.2 创建组

from django.contrib.auth.models import Group
	（1）该模型在数据库被映射为auth_group数据表
	（2）group = Group.objects.create(name=group_name)

### 2.3 组与用户的关系

（1）用户加入用户组
		user对象.groups.add(group)
		group对象.user_set.add(user)
（2）用户退出用户组
		user对象.groups.remove(group)
		group对象.user_set.remove(user)
（3）用户退出所有用户组
		user对象.groups.clear()
（4）用户组中所有用户退出组
		group对象.user_set.clear()

### 2.4  权限

#### 2.4.1 简介

Django的auth系统提供了模型级的权限控制， 即可以检查用户是否对某个数据表拥有增(add), 改(change), 删(delete)权限

#### 2.4.2  权限对象

(1)  from django.contrib.auth.models import Permission
(2)  获取权限对象
permission = Permission.objects.filter(codename='权限代码')[0]

#### 2.4.3  检查用户所拥有的权限

（包括用户直接权限或者间接通过组获取的权限）
	(1)  user对象.has_perm(applabelname.codename)

```
先获取用户名为alice的用户

>>> user1 = User.objects.get(username='alice')

检查user1用户是否有myapp应用中codename为change_student的权限

>>> user1.has_perm("myapp.change_student")

False
```

​	(2)  @permission_required装饰器可以代替has_perm并在用户没有相应权限时重定向到登录页或者抛出异常

```
permission_required:验证当前用户是否拥有相应的权限。

permission_required的参数如下：

参数perm:必须参数，判断当前用户是否拥有权限，参数值为固定格式：appLable名.codename

参数login_url:设置登录界面的URL地址
```

​	(3)  在模板中检查用户权限

```
在模板处理器(auth模板处理器)中向模板返回了一个字典,字典的两个key分别是user、perms，其中user代表当前用户（如果当前用户没登录，则为AnonymousUser对象）;perms代表当前用户的权限；所以在模板中可以直接使用user与perms模板变量。

在模板中判断用户是否登录使用user.is_authenticated

在模板中使用perms查看当前用户是否有某个权限，语法是：

perms.appLabel.权限codename

例如：

{% if user.is_authenticated %}

        恭喜~~~  {{ user.username }}  登录成功 <br/>

        {% else %}

        您还未登录，请先<a href="{% url 'myapp:login' %}">登录</a>  <br/>

    {% endif %}



    {% if perms.myapp.add_student %}

        <a href="#">新增一个学生</a>  <br/>

    {% endif %}



    {% if perms.myapp.change_student %}

        <a href="#">修改学生</a>  <br/>

    {% endif %}



     {% if perms.myapp.delete_student %}

        <a href="#">删除学生</a>  <br/>

    {% endif %}
```

#### 2.4.4  管理用户权限

​	添加权限
​		user对象.user_permissions.add(permission对象)

```
获取权限对象

permission = Permission.objects.filter(codename="change_student").first()

获取用户对象

user1 = User.objects.get(username='alice')

给用户添加权限

user1.user_permissions.add(permission)
```

​	删除权限
​		user对象.user_permissions.remove(permission对象)

```
获取权限对象

permission = Permission.objects.filter(codename="change_student").first()

删除用户1的permission权限

user1.user_permissions.remove(permission)
```

​	清空权限
​		user对象.user_permissions.clear()

```
清空user1的所有权限

user1.user_permissions.clear()
```

#### 2.4.5   管理组权限

​	添加权限
​		group对象.permissions.add(permission对象)
​	删除权限
​		group对象.permissions.remove(permission对象)
​	清空权限
​		group对象.permissions.clear()

#### 2.4.6  自定义权限

​	可以在模型的Meta中设置自定义权限
​	例如

```python
class Product(models.Model):

     name = models.CharField(max_length=20)

     ...

     class Meta:

            permissions = (

                 ('visit_product','Can Visit Product'),

            )

注：visit_product和Can Visit Product分别是数据表auth_permission的codename和name字段
```

# 第九章  Django进阶三

## 1.Django缓存

### 1.1  使用缓存可以大大提高程序的响应速度，增强用户体验

### 1.2  数据库缓存

​	（1）创建缓存表:python manage.py createcachetable 自定义缓存表名称
​	（2）在settings.py中设置缓存为数据库表

```
CACHES = {

        'default': {

            'BACKEND': 'django.core.cache.backends.db.DatabaseCache',

            'LOCATION': '缓存表名称',

        }

    }
```

### 1.3  Redis缓存

#### 1.3.1安装Django中支持Redis缓存的库

（1）pip install django-redis
（2）pip install django-redis-cache
（3）小技巧：也可以将要安装的库写入项目的requirements.txt中，使用"pip install -r  requirements.txt"   命令安装

#### 1.3.2  Redis缓存配置

```
CACHES = {

     'default': {

        "BACKEND": "django_redis.cache.RedisCache",

        "LOCATION": "redis://127.0.0.1:6379/1",

        "OPTIONS": {

            "CLIENT_CLASS": "django_redis.client.DefaultClient",

          'PASSWORD':'123',

        }

    }

}
```

### 1.4  程序层面操作缓存

#### 1.4.1使用cache底层API操作

（1）操作default配置的缓存

```
from django.core.cache import cache
cache.set(key,value,timeout=缓存秒数)、cache.get()
```


（2）操作多种缓存

```python
from django.core.cache import caches
c = caches["缓存配置key"]
c.set(key,value,timeout=缓存秒数)
c.get(key)
```



#### 1.4.2 视图缓存装饰器cache_page

```python
装饰器方式：@cache_page(timeout=缓存秒数,cache="CACHES缓存配置对应的key",key_prefix="前缀")
```





## 2.Django REST framework(一)

### 2.1 REST介绍

1. 全称：Representational State Transfer，表现层状态转化。“表现层”其实指的是“资源(Resource)”的“表现层”

```
    资源（Resource）

    所谓“资源”，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本，一张图片，一首歌曲，一种服务，总之就是一个具体的实例。你可以使用一个URI（统一资源识别符）指向它，每种资源对应一个特定的URI。要获取这个资源，访问它的URI就可以了，因此URI就成了每一个资源的地址或独一无二的识别符。所谓“上网”就是与互联网上一系列的“资源”互动，调用它们的URI。



   表现层（Representation）

“资源”是一种信息实体，它可以有多种外在表现形式。我们把“资源”具体呈现出来的形式，叫做它的”表现层“（Representation）。

URI只代表资源的实体，不代表它的形式。严格地说，有些网站最后的”.html“后缀名是不必要的，因为这个后缀表示格式，属于”表现层“范畴，而URI应该只代表”资源“的位置。它的具体表现形式，应该在HTTP请求头的信息中用Accept和Content-Type字段指定。



    状态转换（State Transfer）

    如果客户端想要操作服务器，就必须通过某种手段，让服务器端发生”状态转换（State Transfer）“。而这种转换是建立在表现层之上的，所以就是”表现层状态转化“

    客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议中，四个表示操作方式的动词：GET，POST，PUT，DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可用于更新资源），PUT用来更新资源，DELETE用来删除资源
```

2.一种软件架构风格、设计风格、而不是标准，提供了一组设计原则和约束条件。

3.到底什么是RESTful架构？
	1.每个URI对应一个特定的资源
	2.客户端和服务器之间，传递这种资源的某种表现层
	3.客户端通过四个HTTP动词(动作谓词)，对服务端资源进行操作，实现”表现层状态转换“

4.参考文档

#### RESTful API.docx【】

```python
RESTful API

什么是REST
一种软件架构风格、设计风格、而不是标准，只是提供了一组设计原则和约束条件。它主要用户客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。
REST全程是Representational State Transfer，表征性状态转移。首次在2000年Roy Thomas Fielding的博士论文中出现，Fielding是一个非常重要的人，他是HTTP协议（1.0版和1.1版）的主要设计者，Apache服务器软件的作者之一，Apache基金会的第一任主席。所以，他的这篇论文一经发表，就引起了广泛的关注。
论文:
本文研究计算机科学两大前沿----软件和网络----的交叉点。长期以来，软件研究主要关注软件设计的分类、设计方法的演化，很少客观地评估不同的设计选择对系统行为的影响。而相反地，网络研究主要关注系统之间通信行为的细节、如何改进特定通信机制的表现，常常忽视了一个事实，那就是改变应用程序的互动风格比改变互动协议，对整体表现有更大的影响。我这篇文章的写作目的，就是想在符合架构原理的前提下，理解和评估以网络为基础的应用软件的架构设计，得到一个功能强、性能好、适宜通信的架构。
(This dissertation explores a junction on the frontiers of two research disciplines in computer science: software and networking. Software research has long been concerned with the categorization of software designs and the development of design methodologies, but has rarely been able to objectively evaluate the impact of various design choices on system behavior. Networking research, in contrast, is focused on the details of generic communication behavior between systems and improving the performance of particular communication techniques, often ignoring the fact that changing the interaction style of an application can have more impact on performance than the communication protocols used for that interaction. My work is motivated by the desire to understand and evaluate the architectural design of network-based application software through principled use of architectural constraints, thereby obtaining the functional, performance, and social properties desired of an architecture. )
REST爆发
其实在REST架构推出的十几年间，它并没有一路高歌的发展，真正的大范围推广是在2013年之后，伴随着移动端的飞速发展，越来越多人的开始意识到，网站即软件，而且是一种新型的软件。
这种"互联网软件采用"客户端/服务器"模式，也就是我们常说的C/S模式，这一切建立在分布式体系上，通过互联网通信，具有高延时，高并发等特点。
网站开发，完全采用软件开发开发的模式。但传统上，软件和网络是两个不同的领域，很少有交集，软件开发主要针对单机环境，网络则主要研究系统之间的通信。我们需要考虑的是如何开发在互联网环境中使用软件。
理解RESTful
要理解RESTful架构，最好的就是去理解它的单词 Representational State Transfer 到底是什么意思，它的每一个词到底要表达什么。
REST的释义，"表现层状态转化"，其实这省略了主语。“表现层”其实指的是“资源(Resource)”的“表现层”。
资源（Resource）
所谓“资源”，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本，一张图片，一首歌曲，一种服务，总之就是一个具体的实例。你可以使用一个URI（统一资源定位符）指向它，每种资源对应一个特定的URI。要获取这个资源，访问它的URI就可以了，因此URI就成了每一个资源的地址或独一无二的识别符。所谓“上网”就是与互联网上一系列的“资源”互动，调用它们的URI。
表现层（Representation）
“资源”是一种信息实体，它可以有多种外在表现形式。我们把“资源”具体呈现出来的形式，叫做它的”表现层“（Representation）。
URI只代表资源的实体，不代表它的形式。严格地说，有些网站最后的”.html“后缀名是不必要的，因为这个后缀表示格式，属于”表现层“范畴，而URI应该只代表”资源“的位置。它的具体表现形式，应该在HTTP请求头的信息中使用Accept和Content-Type字段指定。
状态转换（State Transfer）
访问一个网站，就代表客户端和服务端的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。
互联网通信协议HTTP协议，是一个无状态协议。这意味着，所有的状态都保存在服务端。因此，如果客户端想要操作服务器，就必须通过某种手段，让服务器端发生”状态转换（State Transfer）“。而这种转换是建立在表现层之上的，所以就是”表现层状态转化“。
客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议中，四个表示操作方式的动词：GET，POST，PUT，DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可用于更新资源），PUT用来更新资源，DELETE用来删除资源
到底什么是RESTful架构
1.每一个URI代表一个特定的资源
2.客户端和服务器之间，传递这种资源的某种表现层
3.客户端通过四个HTTP动词(动作谓词)，对服务端资源进行操作，实现”表现层状态转换“
RESTful API设计
协议
API与用户的通信协议，通常使用HTTP(S)协议。
域名
应该尽量将API部署在专用域名之下。
http://api.rock.com
如果确定API很简单，不会有大规模扩充，可以考虑放在主域名之下。
http://www.rock.com/api/
版本
应该将API的版本号放入URL。
http://api.rock.com/v1/
也有做法是将版本号放在HTTP的头信息中，但不如放在URL中方便和直观。GITHUB是这么搞的。
路径（Endpoint）
路径又称”终点“（endpoint），表示API的具体网址。
在RESTful架构中，每个网址代表一种资源（Resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的”集合“（collection），所以API中的名词也应该使用复数。
HTTP动词(动作谓词)
对于资源的具体操作类型，由HTTP动词表示。
HTTP常用动词
GET（SELECT）：从服务器取出资源
POST（CREATE or UPDATE）：在服务器创建资源或更新资源
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）
DELETE（DELETE）：从服务器删除资源
HEAD：获取资源的元数据
OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的
示例
GET /students：获取所有学生
POST /students：新建学生
GET /students/id：获取某一个学生
PUT /students/id ：更新某个学生的信息（需要提供学生的全部信息）
PATCH /students/id：更新某个学生的信息（需要提供学生变更部分信息）
DELETE /students/id：删除某个学生
过滤信息（Filtering）
如果记录数量过多，服务器不可能将它们返回给用户。API应该提供参数，过滤返回结果。
?limit=10
?offset=10
?page=2&per_page=20
?sortby=name&order=desc
?student_id=id
参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复，比如 GET /students/id 和 ？student_id=id
状态码
服务器向用户返回的状态码和提示信息，常见的有以下一些地方
200 OK - [GET]：服务器成功返回用户请求的数据
201 CREATED -[POST/PUT/PATCH]：用户新建或修改数据成功
202 Accepted - [*] ：表示一个请求已经进入后台排队（异步任务）
204 NO CONTENT - [DELETE]：表示数据删除成功
400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误
401 Unauthorized - [*] ：表示用户没有权限（令牌，用户名，密码错误）
403 Forbidden - [*]：表示用户得到授权，但是访问是被禁止的
404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录
406 Not Acceptable - [*]：用户请求格式不可得
410 Gone - [GET] ：用户请求的资源被永久移除，且不会再得到的
422 Unprocesable entity -[POST/PUT/PATCH]：当创建一个对象时，发生一个验证错误
500 INTERNAL SERVER EROR - [*] ：服务器内部发生错误
错误处理
如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error做为键名
返回结果
针对不同操作，服务器想用户返回的结果应该符合以下规范
GET /collection：返回资源对象的列表（数组，集合）
GET /collection/id：返回单个资源对象
POST /collection：返回新生成的资源对象
PUT /collection/id：返回完整的资源对象
PATCH /collection/id：返回完整的资源对象
DELETE /collection/id：返回一个空文档
使用链接关联资源
RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。
{
    "link": {
        "rel":   "collection https://www.rock.com/zoostudents",
        "href":  "https://api.rock.com/students",
        "title": "List of students",
        "type":  "application/vnd.yourformat+json"
      }
}
rel：表示这个API与当前网址的关系
href：表示API的路径
title：表示API的标题
type：表示返回的类型
其它
服务器返回的数据格式，应该尽量使用JSON
API的身份认证应该使用OAuth2.0框架
© 2018 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
Pricing
API
Training
Blog
About
Press h to open a hovercard with more details.

```



### 2.2  Django REST framework框架初级使用

#### 官方网站：

https://www.django-rest-framework.org
中文翻译网站：https://q1mi.github.io/Django-REST-framework-documentation/

#### 2.2.1  需要安装的库：

pip install djangorestframework
pip install django-filter

#### 2.2.2  配置方式

(1)INSTALLED_APPS
	rest_framework
(2)分页设置

```python
REST_FRAMEWORK = {

    'DEFAULT_PAGINATION_CLASS':'rest_framework.pagination.PageNumberPagination',

    'PAGE_SIZE':3,

}
```

#### 2.2.3 序列化：

将复杂的数据结构，例如ORM中的QuerySet或者Model实例对象转换成Python内置的数据类型，从而进一步方便数据和json，xml等格式的数据进行交互
(1)  ModelSerializer

```python
from rest_framework import serializers

class  自定义序列化类(serializers.ModelSerializer):

    class Meta:

        model = 关联模型

        fields = (

            与模型关联的属性名

        )
```

(2)HyperlinkedModelSerializer

2.2.4  请求与响应
	(1)  restful框架的request对象：data属性返回请求体中的内容
	(2)  restful框架的response对象：from rest_framework.response import Response

```
Response(响应的数据,status=状态码)
```

#### 2.2.5  Django Rest Framework实现API开发的方法

##### (1)重构ViewSets类

参考官方文档：http://www.django-rest-framework.org/tutorial/6-viewsets-and-routers/

```python
编写模型类
class Article(models.Model):
     title = models.CharField(max_length=10)
     desc = models.CharField(max_length=100)
     class Meta:
     db_table = 'articles'
```


​		

```python
编写序列化类：
from rest_framework import serializers
class ArticleSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = ['id','title','desc']
```


​		

```python
编写视图集类
from rest_framework import viewsets,mixins
class ArticleViewSet(viewsets.GenericViewSet,
                  mixins.ListModelMixin,
                  mixins.RetrieveModelMixin,
                  mixins.DestroyModelMixin,
                  mixins.CreateModelMixin):
       queryset = Article.objects.all()
       serializer_class = ArticleSerializer
```


​		

```python
编写子路由模块
from rest_framework.routers import SimpleRouter

router = SimpleRouter()
router.register(r'articles',ArticleViewSet)
```

​		

```python
编写总路由模块
urlpatterns = [
    path('app/',include(router.urls))  # 关联前缀路由与子路由模块中的router对象
]
```



##### (2)  基于函数的视图

@api_view视图函数装饰器
  	使用方法：@api_view(["GET","PUT","DELETE"])

```python
例如：

@api_view(['GET','POST','PUT','DELETE'])

def student(request,sid):

    try:

       # 根据主键查询学生

        student = Student.objects.get(pk=sid)  

        

    except Student.DoesNotExist:

        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'GET':

       # 序列化查询到的这个学生对象

        ser = StudentSerializers(student)   

        return Response(ser.data)

    elif request.method == 'PUT':

        ser = StudentSerializers(student,data=request.data)  # 使用新数据更新学生对象

        if ser.is_valid():

            ser.save()

            return Response(ser.data)

        else:

            return Response(status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':

        student.delete()

        return Response(status=status.HTTP_204_NO_CONTENT)
```

# 第十章 Django进阶 四

## 1.  Django REST framework(二)

### 1.1  源码摘抄

```python
GenericViewSet中的方法（也是继承来的）

def get_queryset(self):

    # 默认返回  self.queryset

def get_object(self):

    # 默认根据对象ID返回单个对象

def get_serializer(self, *args, **kwargs):

    # 默认返回self.serializer_class对应的序列化实例对象

def filter_queryset(self, queryset):

    # 对传递进来的queryset进行过滤，返回过滤后的queryset
```

### 1.2  分页配置

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS':'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE':3,
}
```

### 1.3  过滤器

#### 1.3.1  在INSTALLED_APPS中注册应用：django_filters

#### 1.3.2  搜索与排序功能的实现

  (1)  在视图集中添加过滤后端支持类	

```python
from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import filters
filter_backends = (DjangoFilterBackend, filters.SearchFilter,filters.OrderingFilter)
```

（2）搜索
在视图集类中添加search_fields属性，属性值为元祖
（3）排序
在视图集中添加ordering_fields属性，属性值为元祖

#### 1.3.3  自定义过滤器

（1） 继承FilterSet，并编写过滤器参数

```python
import django_filters

class ArticleFilter(django_filters.rest_framework.FilterSet):

    # 过滤字段

    # 左边变量为api接口中过滤的参数

    q = django_filters.CharFilter(field_name='title', lookup_expr='contains')

    is_delete = django_filters.CharFilter(field_name='is_delete')

    id_min = django_filters.CharFilter(field_name='id', lookup_expr='gte')

    id_max = django_filters.CharFilter(field_name='id', lookup_expr='lte')

    class Meta:

        # 过滤模型

        model = Article

        # 过滤字段

        fields = ['title', 'is_delete']
```

（2）在自定义的视图集(ViewSet)类中关联过滤器（通过类属性）：
filter_class = 自定义过滤器类

### 1.4  序列化类的验证功能

(1)   可以在序列化类中添加类属性（类属性名应该与模型类属性一致），指定验证格式规则
(2)  可以重写validate( )方法，只有在格式验证通过后才会自动调用该validate( )方法,进行逻辑验证
(3)  验证动作是通过调用序列化对象的is_valid(raise_exception=True)方法
raise_exception若为True,则验证失败后会抛出异常；否则不抛出异常

### 1.5  renderer重构

​	(1)  作用：对返回的数据进行格式定义
​	(2)  编写继承自JSONRenderer的类

```python
from rest_framework.renderers import JSONRenderer

class MyJsonRenderer(JSONRenderer):

    def render(self, data, accepted_media_type=None, renderer_context=None):

        res = {

            'code': code,

            'msg': msg,

            'data': data

        }

        return super().render(res)
```

​	(3)  配置：

```python
REST_FRAMEWORK = {
    # 重构renderer
    'DEFAULT_RENDERER_CLASSES': (
        'MyJsonRenderer位置',
    ),
}
```



## 2.  Form表单类

#### 自定义表单类的步骤

##### （1）  继承django.forms.Form类

##### （2）  编写对应于表单域的类属性

```
from django import forms

class RegForm(forms.Form):  # 继承Form类

          regname = forms.CharField(label="注册用   户名",max_length=10)

          regpwd = forms.CharField(label="注册密码",widget=forms.PasswordInput())

          reghome = forms.ChoiceField(label="籍贯",choices=(("1","陕西"),("2","北京"),("3","山东")))

          sex = forms.ChoiceField(label="性别",choices=(("0","男"),("1","女")),widget=forms.RadioSelect())
```

##### （3）  视图函数中使用表单类对象

```
实例化一个空的表单对象

RegForm()   

将POST数据传入表单对象的构造方法中

RegForm(request.POST) 

表单对象的常用方法：

is_valid()  # 验证表单数据是否有效

cleaned_data["类属性"] # 接收指定表单域的数据
```

##### （4）在模板上渲染表单

```
{{ regform.as_p }}
```

# 第十一章 Django进阶五——Django-CBV

##  1.  Django-CBV(class base views)

————继承View类(from django.views import View)的通用视图 

```
CBV（class base views） 基于类的视图；
FBV（function base views） 基于函数的视图

CBV的优点：
提高了代码的复用性，可以使用面向对象的技术，比如Mixin（多继承）；
可以用不同的函数针对不同的HTTP方法处理，而不是通过很多if判断，提高代码可读性
```

## 2.  在类中编写处理相应请求的方法

例如：
get(self,request)
post(self,request)

## 3.  在重写dispatch方法中添加请求通用功能

​	  

```python
  def dispatch(self, request, *args, **kwargs):
        print('before')
        obj = super(通用视图类名,self).dispatch(request,*args,**kwargs)
        print('after')
        return obj
```

## 4.  添加装饰器

(1)@method_decorator装饰器的作用：
通过为函数视图装饰器补充第一个self参数，以适配类视图方法，最终将函数装饰器转化为方法装饰器

(2)自定义装饰器实现函数
		

```python
def mydecorator(func):
    def wrapper(*args,**kwargs):
        print(time.time())
        return func(*args,**kwargs)
    return wrapper
```


(3)  在指定方法上添加装饰器
		 

```python
   @method_decorator(mydecorator)
    def get(self,request):
        print('before get......')
        return HttpResponse('Login...get')
```


(4)  在类上添加装饰器
		

```python
@method_decorator(mydecorator,name='dispatch')
class Login(View):
        def dispatch(self, request, *args, **kwargs):
        print('before')
        obj = super(Login,self).dispatch(request,*args,**kwargs)
        print('after')
        return obj
```

# 第十二章  Day10-Django的跨域解决

## Django解决跨域

### 	跨域

​		当一个请求url的协议、域名、端口三者之间任意一个与当前页面url不同即为跨域

### 	同源策略

​		为了解决Ajax跨域所产生的不安全因素，提出了“浏览器同源策略”

### 	Django解决跨域问题的办法

​		(1)  安装django-cors-headers
​			pip install django-cors-headers
​		(2)  在settings.py中配置

```python
INSTALLED_APPS = [

    'corsheaders'，

 ] 

MIDDLEWARE_CLASSES = (

    ...

 'corsheaders.middleware.CorsMiddleware',

    'django.middleware.common.CommonMiddleware', # 注意顺序

    ...

)

# 跨域允许的请求方式

CORS_ALLOW_METHODS = (

    'GET',

    'POST',

    'PUT',

    'PATCH',

    'DELETE',

    'OPTIONS'

)

# 允许跨域的请求头，可以使用默认值，默认的请求头为:

# from corsheaders.defaults import default_headers

# CORS_ALLOW_HEADERS = default_headers

CORS_ALLOW_HEADERS = (

    'XMLHttpRequest',

    'X_FILENAME',

    'accept-encoding',

    'authorization',

    'content-type',

    'dnt',

    'origin',

    'user-agent',

    'x-csrftoken',

    'x-requested-with',

    'Pragma',

)

# 跨域请求时，是否运行携带cookie，默认为False

CORS_ALLOW_CREDENTIALS = True

# 允许所有主机执行跨站点请求，默认为False

# 如果没设置该参数，则必须设置白名单，运行部分白名单的主机才能执行跨站点请求

CORS_ORIGIN_ALLOW_ALL = True
```

