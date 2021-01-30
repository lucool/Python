# 补课-01

## 一、Python上下文

#### 1.1 什么是上下文

任意的Python对象都可以使用上下文环境，使用with关键字。上下文是代码片段的区域，当对象进入上下文环境时，解析器会调用对象的`__enter()__`,当对象退出上下文环境时，会调用对象的`__exit__()`。

#### 1.2 为什么使用

对象在使用上下文环境时，为了确保对象正确的**释放资源**，避免出现**内存遗漏。**存在以下对象经常使用上下文with：

- 文件操作对象 open()
- 数据库的连接 conn()
- 线程锁Lock

#### 1.3 如何使用

#### 1.3.1 重写类的方法

上下文的两个核心方法

```python
class A:
	def __enter__(self):
        # 进入上下文时，被调用的
        # 必须 返回一个相关的对象
        print('--enter--')
        return 'yunshao'
 
    def __exit__(self,except_type,value,tb):
        # 退出上下文的时候，被调用的
        # except_type 如果存在异常时，表示为异常类的实例对象
        # val 异常的信息Message
        # tb 异常的跟踪栈
        print('--exit--')
        # 返回 True 表示 存在异常不向外抛出
        # 返回 False 表示存在异常时，向外抛出
        return True
```



#### 1.3.2 with中使用

```python
a = A()
with A() as ret:
    print(ret)
    assert isinstance(ret,int)
    print('OK')
```

## 二、Dao设计

DAO(Data Access Object)数据库访问对象只是一种设计思想，目的是简化对数据库层操作。针对实体类（数据模型类）对象，封装一套与数据库操作的SDK(Software Develope Kit)。

合理的DAO的SDK的设计：

```
- dao （基础数据库操作类）
	|--__init__.py
	|--base.py
- entity （实体类模块）
	|--user
	|--order
- db (具体数据操作)
	|--user_db.py
	|--order_db.py
```



## 三、元类之ORM

