# Flask第二天

## 模板

在视图函数中通过render_template("模板地址",关键字参数)加载模板，并传递关键字实参

### 模板标签

```
1. if标签
    {% if 判断条件1 %}
        
    {% elif 判断条件2 %}
        ......
    {% else %}

    {% endif %}

2. for标签
     {% for 循环变量  in 将要遍历的变量 %}
          循环体
     {% endfor %}
```

### 模板继承

```
在父模板中使用
{% block 块标签名称 %}
 {% endblock %} “挖坑”；
在子模板中通过{% extends '父模板的位置' %}继承父模板,并通过块模板标签“填坑”。

  如果在子模板中需要继承父模板块标签中的内容，则使用{{ super() }}
```

### 模板包含

{% include '包含模板的路径' %}

### 模板过滤器

```
在模板变量显示之前通过模板过滤器对显示内容做最后处理
    常用过滤器：
    upper：  全部大写
    lower:   全部小写
    title:   对每个单词的首字母大写，每个单词的剩余字母全部小写
    capitalize: 对首字母大写，剩余字母全部小写
    reverse： 字符串倒序
    sum ： 数字列表求和
    safe: 不对特殊字符转义
```

### 宏(macro)

​	将模板中的常用处理逻辑封装起来，方便模板重复调用
​	宏的使用步骤
​			定义宏

```
在某个模板中，通过
        {% macro 宏名称(参数) %}
             宏的具体实现
        {% endmacro %}
```

​			调用宏

```
在其他模板中，先通过{% import '包含宏的那个模板名' as 模板别名 %}导入包含宏的模板；
     使用 {{ 模板别名.宏名称(传递给宏的参数) }}
```

## 静态资源的加载

​	url_for('static',filename="文件的路径")
​	模板中使用:  {{ url_for('static',filename="文件的路径") }}

## jsonify()的用法

> 将字典包装（将字典、列表、元祖传入jsonify()中），返回给客户端，客户端以json格式接收
>
> 响应头的Content-Type部分是:application/json

# Flask第三天

## 蓝图(蓝本)插件

​	安装方式：pip install flask-blueprint
​	作用：把不同功能的模块分开，实现应用的模块化

### 一般套路

- 在App的__init__.py中，创建程序实例，并注册“蓝图”对象
  	程序实例对象.register_blueprint(蓝图对象)
- 在应用的views.py(或者views包下的视图模块)中创建蓝图对象，实现请求处理函数(视图函数)，并使用蓝图关联路由
- 在服务启动文件（管理文件），创建Manager对象，并与程序实例关联

## 请求参数的接收方式

- 接收查询字符串参数(使用?将URL与参数隔开)
  		request.args.get("参数名","默认信息")
- 接收封装到请求体(body)中的参数
  		request.form.get("参数名","默认信息")

## Cookie与Session

### Cookie

Cookie是生成于服务端，但保存于客户端的信息;Cookie保存于客户端后，作为请求头(request headers)信息的一部分，每次发送请求时都会将Cookie发送到服务器端。

常用操作：

​			添加Cookie
​				响应对象.set_cookie("cookie名称","Cookie值",max_age=Cookie				保存的秒数)
​			删除Cookie
​				响应对象.delete_cookie(cookiename) 
​			查看某个Cookie
​				request.cookies.get("Cookie名称","default提示")
​			查看所有Cookie
​				request.cookies
​					返回的是字典对象（dict的子类对象）

### Session

​		根据Session的存储位置分类

- server side session（将Session信息保存于服务器端）
  客户端通过保存的携带sessionid值的那个cookie，与服务端Session信息进行匹配。
- client side session（将Session信息保存于客户端）
  	Flask的Session机制(Flask的Session信息默认保存于客户端Cookie中)

> Session出现的原因：http协议是一种“无状态”协议，这意味着仅凭http协议，服务端无法判断多次请求是否来自同一个客户端；这时，为了弥补http的“无状态”特性，使用“会话”Session来记录跟踪不同的客户端。

### Flask的Session操作

> 设置Session的秘钥

​		程序实例.secret_key = '密钥'
​		程序实例.config["SECRET_KEY"] = '密钥'			

> 给Session添加数据

​		session["属性名"] = 属性值

> 删除session的某个属性

​		session.pop(session的属性名,"default")

> 清空session

​		session.clear()			

> 遍历session

​		通过session.items()

> 根据属性名查看Session

​		session.get('属性名','default')

## Flask-Session插件

作用：给Flask应用添加服务端Session(Server-side Session)的支持
安装：pip install Flask-Session		

> 常用配置

- SESSION_TYPE = "redis"   设置Session的存储类型

- SESSION_REDIS = redis.Redis(host='session所在主机',port=端口,db=数据库索引)

- SESSION_KEY_PREFIX = “设置的前缀”

  PERMANENT_SESSION_LIFETIME = timedelta(seconds=30) 最大不交互时间

> app(Flask实例对象，程序实例)与Session关联

​				from flask_session import Session  
​					Session(app)

> Cookie与服务端Session的关系

服务端保存了session的具体数据（session的属性名和对应的属性值），而sessionid(session唯一标志)“一式两份”，一份存储于服务端，另一份存储于客户端的Cookie,每次客户端发送请求时，都会携带着这个sessionid去查找服务端对应的该sessionid是否存在，如果存在，则该请求就属于这个session会话。

# Flask第四天

## “闪”消息

“闪消息”存储于session中，当在模板中显示了“闪消息”后，自动删除session中的闪消息数据
		flash("闪消息内容")

在模板中显示“闪消息”并从session中删除该闪消息，需要在模板中使用
		{% for message in get_flashed_messages() %}
           {{ message }}
		{% endfor %}

## flask-sqlalchemy插件

> 用途：给Flask框架添加了SQLAlchemy的支持，提供操作关系型数据库的通用操作
> 安装方式：pip install flask-sqlalchemy

### 操作演练

​		通过shell交互环境
​			python 启动文件  shell
​		启动开发服务器

### 使用方法

​		创建SQLAlchemy对象：db = SQLAlchemy()
​		创建模型

```
class Product(db.Model):
    __tablename__ = "products"
     _id = db.Column(db.Integer,primary_key=True)
    price = db.Column(db.Float)
    address = db.Column(db.String(20))
```

​		创建程序实例，并设置数据库配置

```
app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+pymysql://root:123456@localhost:3306/mydb?charset=utf8"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = True
db.init_app(app)  # 将app与db关联
```

​		相关操作

```
1. 创建表
   db.create_all()
   
   删除表
   db.drop_all()

 2. 添加记录
   db.session.add(模型对象)
   db.session.commit()
   db.session.add_all([模型对象1,模型对象2,...])

3. 删除某条记录
   db.session.delete(product)
   db.session.commit()

4. 查询所有记录
   模型类名称.query.all()  # 查询结果是list

5. 根据条件查询
   模型类名称.query.filter_by(关键字参数)  # 查询结果是BaseQuery
       BaseQuery调用all()返回list;调用first()返回模型对象

6. 通过filter()方法进行复杂查询
eg:
products = Product.query.filter(Product.name.startswith("产品")).all()  
products = Product.query.filter(Product.name.startswith("产品")).first()
fruits = Fruit.query.filter(Fruit.name.endswith("子"))

7. 通过get()方法查询主键,返回模型对象模型类名称.query.get(主键值)
eg:Product.query.get(11)
```

## flask-migrate插件

> 作用：迁移模型
> 安装方式：pip install flask-migrate

使用flask-migrate插件与flask-script插件相结合的方式，进行迁移数据库

- 启动文件加入

  ```
  # 指定app与db，目的是创建migrations目录
  migrate = Migrate(app,db)  
  
  manager = Manager(app)
    
  #  注册一个自定义的迁移命令 
  manager.add_command("nicedb",MigrateCommand) # 注册命令
  ```

  

- 迁移步骤

  ```
  第一步：初始化迁移目录
  python 启动脚本.py 注册命令 init  # 本例中注册命令为nicedb
  
  第二步：制作迁移文件
  python 启动脚本.py 注册命令 migrate
  
  第三步：迁移模型到数据库
  python 启动脚本.py 注册命令 upgrade
  ```

## “一对多”关系模型

> 关联方式

- 在“一”方模型中通过relationship函数从对象角度关联"多"方
  			db.relationship('多方模型类名称',backref='反向引用名称')

- 在“多”方模型中通过在Column()中设置ForeignKey()从数据库表角度设置外键，关联“一”方
  			db.Column(db.Integer,db.ForeignKey("一方表名称.一方表主键"),nullable=False) 
              返回的类属性名作为表中的外键名。

- 一对多代码

  ```python
  from SQLAlchemyAPP.ext import db
  # db是SQLAlchemy的实例化对象
  
  class School(db.Model):   # "一方"
      __tablename__ = "schools"
      id = db.Column(db.Integer,primary_key=True,autoincrement=True)
      name = db.Column(db.String(20),nullable=False)
      address = db.Column(db.String(50))
      students = db.relationship("Student",backref="univercity") # 关联学生模型，并设置反向引用名称
  
  class Student(db.Model):   # "多方"
      __tablename__ = "students"
      id = db.Column(db.Integer, primary_key=True, autoincrement=True)
      name = db.Column(db.String(20), nullable=False)
      age = db.Column(db.Integer)
      sex = db.Column(db.String(10))
      score = db.Column(db.Float)
      # 在多方从数据库层面关联一方
      school_id = db.Column(db.Integer,db.ForeignKey("schools.id"),nullable=False)
  ```

  > 操作举例

```
1. 添加“多”方记录：
       通过外键关联“一”方：
       eg:  student1 = Student(name="张三",age=20,sex="男",school_id=1)

       通过反向引用名称关联“一”方：
       eg:  student2 = Student(name="李四",age=25,sex="男",university=school1)
            注：university是反向引用名称


 2. 从“一”方查询“多”方
       通过“一”方模型中relationship()函数对应的类属性直接查询。
       eg: 已知学校查询学生
           school1.students

 3. 从“多”方查询“一”方
       通过反向引用名称直接查询对应的“一”方实例对象
       eg: 已知学生查询学校
           student3.university
```

## "一对一"关系模型

在某一方：db.relationship('另一方模型类名称',backref='反向引用名称',uselist=False)

在维护关系的“一”方：db.Column(db.Integer,db.ForeignKey("另一方表名称.主键"),nullable=False,unique=True)

# Flask第五天

## “多对多”关系  

​	创建db = SQLAlchemy()
​	db.Table()创建中间关系表
​	创建两个“多”方模型
​	案例

```
db = SQLAlchemy()

# 中间关系表对应的代码
user_group = db.Table(
      "user_group_relation",
db.Column("userid",db.Integer,db.ForeignKey("users.id"),primary_key=True),
db.Column("groupid",db.Integer,db.ForeignKey("groups.id"),primary_key=True),
)

# 用户模型
class User(db.Model):
       __tablename__ = 'users'
       id = db.Column(db.Integer,primary_key=True)
       name = db.Column(db.String(20))
       ugroups = db.relationship("Group",secondary=user_group,backref=db.backref("gusers"))

#组模型
class Group(db.Model):
    __tablename__ = 'groups'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(20))

    具体操作：
    添加两个用户：
    user1 = User(name="令狐冲")
    user2 = User(name="郭靖")
    user3 = User('黄蓉')
    db.session.add(user1)
    db.session.add(user2) 
    db.session.add(user3)
    db.session.commit()

    添加三个组：
    group1 = Group(name="乒乓球组")
    group2 = Group(name="喝酒组")
    group3 = Group(name="读书组")
    db.session.add(group1)
    db.session.add(group2)
    db.session.add(group3)
    db.session.commit()

    添加关联关系：
    user1.ugroups.append(group1)
    user1.ugroups.append(group2)
    db.session.commit()

     user2.ugroups.append(group1)
     user2.ugroups.append(group3)
     db.session.commit()

     也可以通过组，主动关联人
     group3.gusers.append(user3)

     # 用户关联多个组，也可以使用extend
     user2.ugroups.extend([group1,group3])


    查询user1加入的组名称：
    for g in user1.ugroups:
        print(g.name)

    查询加入group1的用户姓名：
    for u in group1.gusers:
        print(u.name)
```

## flask-cache缓存插件

​	安装方式：pip install flask-cache
​	使用方式

```
  一：完全使用Redis的默认缓存配置
   缓存到本地端口为6379的Redis的0号库：
   cache = Cache(config={'CACHE_TYPE': 'redis'})
   cache.init_app(app程序实例)

   @cache.cached(timeout=超时时间秒数,key_prefix="可以设置缓存key的前缀")
    def 视图函数名称():
        耗时操作

    注意:如果使用的flask-cache插件是0.13.1版本，则需要将jinja2ext.py源码中
        第33行代码修改为：from flask_cache import make_template_fragment_key。

  二：使用完整的Redis配置

   cache_config = {
    'CACHE_TYPE':'redis',
    # REDIS所在的主机
    'CACHE_REDIS_HOST':'10.12.155.62',  
    'CACHE_REDIS_PORT':6379,    # Redis端口
    'CACHE_REDIS_DB':3,   # Redis数据库索引
    'CACHE_REDIS_PASSWORD':'',  # Redis密码
}

    app = Flask(__name__)
    app.config["SECRET_KEY"] = "abcdef"
    app.register_blueprint(blue)
    c.init_app(app,cache_config)   # 关联程序实例与缓存对象

c = Cache()

@blue.route("/cache/")
@c.cached(timeout=30,key_prefix="前缀")
def cache_view():
    return "视图函数返回的结果"
```

## Flask的请求钩子 

- Flask的请求钩子是通过装饰器的形式实现

- before_first_request
  注册一个函数，在处理第一个请求之前运行

- before_request
  注册一个函数，在每次请求之前运行

- after_request
  正常请求（没发生异常,或异常被处理了）之后执行
  在调试模型关闭时，发生了异常且异常未被处理，也会执行装饰的该方法
  被装饰的请求钩子函数接收一个响应对象的参数，最终一定要返回该响应对象

- teardown_request
  注册一个函数(该函数接收一个异常对象的参数)，即使有未处理的异常抛出，也在每次请求之后运行(关闭调试模式)

- 使用技巧
  在请求钩子函数和视图函数之间共享数据一般使用上下文全局变量g

  例如，before_request 处理程序可以从数据库中加载已登录用户，并将其保存到 g.user 中。随后调用视图函数时，视图函数再使用 g.user 获取用户

- 使用举例
  app使用

  ```
  from flask import request
  
  
  def load_middleware(app):
  
      @app.before_request
      def before():
          print("中间件", request.url)
  
          """
              统计
              优先级
              反爬
              频率
              用户认证
              用户权限
          """
  
      @app.after_request
      def after(resp):
  
          return resp
  
      @app.teardown_request
      def teardown(e):
          print('teardown()钩子函数调用了，',e)
  ```

  ## 四大内置对象 

- request

- session

- g

  一个请求中的全局对象，只要在同一个请求内，都可以通过g对象传递数据

- config

  - 针对程序实例的配置
  - 在模板中直接使用config内置对象
  - 在函数中通过current_app.config访问config对象
  - 遍历config内容
    for k,v in config对象.items():
            print(k,"=========>",v)

# Flask第六天

flask-restful插件  

## 	REST介绍

- 全称：Representational State Transfer，表现层状态转化。“表现层”其实指的是“资源(Resource)”的“表现层”

- 一种软件架构风格、设计风格、而不是标准，提供了一组设计原则和约束条件。

- 到底什么是RESTful架构？
  			1.每一个URI代表一个资源
  			2.客户端和服务器之间，传递这种资源的某种表现层
  			3.客户端通过四个HTTP动词(动作谓词)，对服务端资源进行操作，实现”表现层状态转换“
  		

- 参考文档
  RESTful API.docx

- Flask-RESTful 是一个快速构建 Rest APIs 的 Flask 扩展
  	安装：pip install flask-restful

- 该插件通过Api对象注册资源类，将资源类与路由关联起来

  ```
  from flask_restful import Resource
  class StudentResource(Resource):
      def post(self):
          添加资源逻辑
          return data
  
      def get(self):  # 可传入其他参数
          查询资源逻辑
          return data
  api = Api()
  
  api.add_resource(StudentResource,"/student/","/student/<int:sid>/")
  ```

  ## Flask-Restful格式化输出

  ​	默认输出字典(字典的value是基本数据类型)，可以直接进行序列化

  ​	如果包含自定义对象,默认会抛出异常，对象不可JSON序列化

  ​    使用格式化工具
  ​			marshal 函数
  ​				marshal(原始数据,格式化模板字典对象)
  ​			marshal_with 装饰器

  ```
  @marshal_with(字典对象)
      
      from flask_restful import marshal_with
      解释：@marshal_with()装饰器中传递的字典对象参数组成最终的序列化响应输出。
      eg:fruit_output = {
           "name":fields.String(attribute="fruitname"),
           "color":fields.String,
           "msg":fields.String(default="default msg"),
         }
         @marshal_with(fruit_output)
         def xxx():
               pass
  
      注意：如果定制输出的结果包含对象、字典则需要使用fields.Nested(字典对象)；
            如果定制输出包含列表，则需要使用fields.List(fields.Nested(字典对象))
  ```

  ​			条件

  ```
  - 格式
        - 字典格式
        - 允许嵌套
        - value 是 fields.xxx
      - 数据
        - 允许任何格式
  
      - 如果格式和数据完全对应，数据就是预期格式
      - 如果格式比数据中的字段多，程序依然正常运行，不存在的字段是默认值
      - 如果格式比数据中的字段少，程序正常执行，少的字段不会显示
      - 以格式的模板为主
  
    - 结论
  
      - 想要什么格式的返回
      - 格式工具（模板）就是什么样的
      - 和传入的数据没什么直接关系
  
    - 格式和数据的映射
  
      - 格式中的字段名和数据中的名需要一致
        - 也可以手动指定映射
        - attribute=‘Property_name’
      - 也可以对属性指定默认值
        - default
        - 指定默认值，值传递使用传进来的值
        - 未传递，则使用默认值
  ```

  ## 	Restful请求参数转换

  ​		RequestParser参数解析

  ```
  - 使用过程
      - 先定义一个RequestParser对象
      - 向对象中添加字段add_argument()方法
      - 解析参数  parse_args() 
        获取字段  get()
    - 对象在添加参数的时候，可以实现数据预校验
      - 参数是否必须 required
      - 数据的类型  type
      - 还可以设置错误提示  help
      - 接收多个值  action="append"
      - 也可以在接收参数的时候指定别名(用dest关键字参数)
      - location 可以指定参数的来源
           -- headers  : 接收请求头中的信息
           -- cookies : 接收Cookie中的信息
           -- args :  接收查询字符串中的信息
           -- form :  接收请求体中的信息
  
  
  rp2 = RequestParser()  
  rp2.add_argument("hobby",action="append")   # 可以接收多个同名参数hobby
  rp2.add_argument("User-Agent",location="headers",dest="ua")   # 从请求头信息接收User-Agent头信息
  
  rp2.add_argument("__guid",location="cookies",dest="gid")   # 接收指定Cookie名称的信息
  ```

  ​		案例

  ```
  from flask_restful import Resource, fields, marshal_with, marshal, abort, reqparse
  
  parser = reqparse.RequestParser()
  parser.add_argument("g_name", type=str,required=True ,help="please input g_name")
  
  parser.add_argument("g_price", type=float, help="please input number")
  
  parser.add_argument("mu", action="append")
  
  parser.add_argument('rname', dest="name")
  
  parser.add_argument("OUTFOX_SEARCH_USER_ID_NCOO", dest="lo", action="append", location=["cookies", "args"])
  parser.add_argument("User-Agent",dest="ua", location="headers")
  ```

  ​		完整案例

  ```
  from flask_restful import reqparse, Resource
  
  parser = reqparse.RequestParser()   # 实例化RequestParser对象，“创建枪”
  parser.add_argument("name",required=True,help="必须输入姓名！！！",dest="stuname")   # 添加将要解析的参数，“装子弹”
  parser.add_argument("age",type=int)
  parser.add_argument("score",type=float,required=True,help="必须输入考试成绩！")
  parser.add_argument("hobby",action="append")  # 以列表的形式接收参数
  
  parser2 = reqparse.RequestParser()
  parser2.add_argument("cakename",location="form")  # 从body中接收参数
  parser2.add_argument("cakeprice",type=float,location="args")   # 从URL中接收参数
  parser2.add_argument('User-Agent', location='headers',dest="ua")  # 从请求头接收参数
  parser2.add_argument("__guid",location="cookies",dest="mycookie_name")  # 从Cookie中获取数据
  
  class StudentResource(Resource):
  
      def post(self):
          args = parser.parse_args()  # 解析参数，“开枪”
          stuname = args.get("stuname")   # 通过dest关键字参数指定的别名获取参数
          stuage = args.get("age")
          stuscore = args.get("score")
          hobby = args.get("hobby")
          print("hobby=",hobby)
          return {"stuname":stuname,"stuage":stuage,"stuscore":stuscore,"hobby":hobby}
  
  
  class CakeResource(Resource):
      def post(self):
          args = parser2.parse_args()
          cakename = args.get("cakename")
          cakeprice = args.get("cakeprice")
          user_agent = args.get("ua")
          print("cakename=",cakename)
          print("cakeprice=",cakeprice)
          print("user_agent=",user_agent)
          return {"cake_name":cakename,"cake_price":cakeprice,"user_agent":user_agent}
  
      def get(self):
          args = parser2.parse_args()
          mycookie_value = args.get("mycookie_name")
          print("mycookie_value=",mycookie_value)
          return {"mycookie_value":mycookie_value}
  ```

  # Flask-CORS 

  ## 基本概念

  > 什么是跨域：

  跨域指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的,是浏览器对javascript施加的安全限制。

  > 浏览器的同源策略：

  浏览器同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源进行狡猾。
  同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源

  > 怎样才算同源？

  同源是指：同一个请求协议(如：Http或Https)、同一个Ip、同一个端口，3个全部相同，即为同源

  W3C组织制定了一个Cross-Origin Resource Sharing规范，简写为Cors，现在这个规范已经被大多数浏览器支持，从而使解决跨域问题称为可能

  > 如何解决跨域问题

  ​	前端解决：JSONP
  ​	后端解决：后端应用中对CORS进行配置

  > Flask解决方案：

  (1)安装flask_cors插件
  (2)与Flask程序实例关联
      CORS(app, supports_credentials=True)

