# ElasticSearch使用文档

## 一、ElasticSearch Docker安装文档

> 参考文档： https://www.elastic.co/guide/en/elasticsearch/reference/6.7/docker.html#docker-prod-cluster-composefile

### 拉 ES镜像

```
docker pull docker.elastic.co/elasticsearch/elasticsearch:6.7.0
```

或者直接拉

```
docker pull elasticsearch
```



### 运行ES镜像

> 开发模式下的启动方式

```
docker run -dit --name es0 -p 80:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.7.0
```

 测试是否启动成功：  http://localhost:9200，返回json数据如下：

```
{
  "name" : "4XRkHUX",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "IzcS_d3mTjmWUKCUDMiTyg",
  "version" : {
    "number" : "5.6.12",
    "build_hash" : "cfe3d9f",
    "build_date" : "2018-09-10T20:12:43.732Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}

```

### 产品模式

关于 vm.max_map_count 内核参数， 在产品模式下，至少设置为 262144

1. Linux

   ```
   $ grep vm.max_map_count /etc/sysctl.conf
   vm.max_map_count=262144
   ```

   应用设置命令

   ```
   sysctl -w vm.max_map_count=262144
   ```

2. Mac

   ```
   $ screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
   ```

   

3. Window-DockTools

   `必须为每一台docker-machine设置vm.max_map_count`参数:

   ```
   docker-machine ssh
   sudo sysctl -w vm.max_map_count=262144
   ```

   

### 集群-compose方式

使用docker-compose编排技术， 创建docker-compose.yml 文件

```
version: '2.2'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.7.0
    container_name: elasticsearch
    environment:
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - esdata1:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - esnet
  elasticsearch2:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.7.0
    container_name: elasticsearch2
    environment:
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - "discovery.zen.ping.unicast.hosts=elasticsearch"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - esdata2:/usr/share/elasticsearch/data
    networks:
      - esnet

volumes:
  esdata1:
    driver: local
  esdata2:
    driver: local

networks:
  esnet:
```

## API接口

### API接口约定

> 参考： 
>
> - https://www.elastic.co/guide/en/elasticsearch/reference/6.7/api-conventions.html
>
> - https://es.xiaoleilu.com/050_Search/15_Pagination.html

ElasticSearch整体使用RESTful规范，基于HTTP协议，并使用json格式的数据进行交互， 除非有特殊规定。

主要分四个方面：

- Multiple Indices  多索引
- Date math support in index names 索引名中支持日期
- Common options  通用参数
- URL-based access control 网址访问控制

### ES术语

- Document文档数据（就是具体存在ES中一条数据)
- Type索引中的数据类型（可以理解为mysql中的table）  
- Index索引（可以理解为mysql中的数据库）  
- Field字段，文档的属性  
- Query DSL查询语法



### 索引API

#### 1. 创建索引

接口： http://localhost:9200/foods  

请求方法： PUT

参数：

```Javascript
{
   "settings" : {
      "number_of_shards" : 5,
      "number_of_replicas" : 1
   }
}
```

参数说明：

- number_of_shards 主分片数量（默认一个索引被分配5个主分片）
- number_of_replicas 复制分片的数量（每个主分片都有一个复制分片 ）

返回数据：

```
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "aa_indexs"
}
```



#### 2. 查看索引 

接口： http://localhost:9200/foods  

方法： GET

返回： 

```
{
    "foods": {
        "aliases": {},
        "mappings": {},
        "settings": {
            "index": {
                "creation_date": "1553840645474",
                "number_of_shards": "5",
                "number_of_replicas": "1",
                "uuid": "2uh6s1YxR4S_8AtA4JAR6g",
                "version": {
                    "created": "5061299"
                },
                "provided_name": "foods"
            }
        }
    }
}
```

#### 3. 删除索引 

Delete方法

响应数据

```

```



###文档API

> 参考： https://www.elastic.co/guide/en/elasticsearch/reference/6.7/docs.html

​         程序中大多的实体或对象能够被序列化为包含键值对的JSON对象，**键(key)**是**字段(field)**或**属性(property)**的名字，**值(value)**可以是字符串、数字、布尔类型、另一个对象、值数组或者其他特殊类型，比如表示日期的字符串或者表示地理位置的对象。

​        通常，我们可以认为**对象(object)**和**文档(document)**是等价相通的。不过，他们还是有所差别：对象(Object)是一个JSON结构体——类似于哈希、hashmap、字典或者关联数组；对象(Object)中还可能包含其他对象(Object)。
在Elasticsearch中，**文档(document)**这个术语有着特殊含义。它特指最顶层结构或者**根对象(root object)**序列化成的JSON数据（以唯一ID标识并存储于Elasticsearch中）。

#### 文档元数据

​        一个文档不只有数据。它还包含了**元数据(metadata)**——**关于**文档的信息。三个必须的元数据节点是：

| 节点     | 说明               |
| -------- | ------------------ |
| `_index` | 文档存储的地方     |
| `_type`  | 文档代表的对象的类 |
| `_id`    | 文档的唯一标识     |

#### 添加文档数据

**格式：**  /_index/_type/_id

自增ID接口：  /foods/text/

自定义ID接口： /foods/text/1

方法： POST

参数：

```
{
  "name": "蒸蛋",
  "price": 20.5,
  "images": "foods/1.jpg"
}
```

返回数据

```
{
    "_index": "foods",
    "_type": "text",
    "_id": "AWnIMtiq2SkaP_lK0C2N",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "created": true
}
```

#### 查询文档

格式:   /index/type/id?_source=字段1,字段2&pretty

说明：

- id 是添加的id值

-  _source 检索文档的一部分字段
- pretty 美化输出

请求方法： GET

如：  /foods/text/AWnIMtiq2SkaP_lK0C2N



### 搜索API

#### 1. 空搜索

接口： /_search

请求方法： GET

返回数据

```
{
    "took": 220,
    "timed_out": false,
    "_shards": {
        "total": 10,
        "successful": 10,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 3,
        "max_score": 1,
        "hits": [
            {
                "_index": "foods",
                "_type": "text",
                "_id": "1",
                "_score": 1,
                "_source": {
                    "name": "蒸蛋",
                    "price": 20.5,
                    "images": "foods/1.jpg"
                }
            },
            {
                "_index": "foods",
                "_type": "text",
                "_id": "AWnIMtiq2SkaP_lK0C2N",
                "_score": 1,
                "_source": {
                    "name": "蒸蛋",
                    "price": 20.5,
                    "images": "foods/1.jpg"
                }
            },
            {
                "_index": "foods",
                "_type": "text",
                "_id": "3",
                "_score": 1,
                "_source": {
                    "name": "蒸饼",
                    "price": 1.5,
                    "images": "foods/3.jpg"
                }
            }
        ]
    }
}
```

 响应中最重要的部分是`hits`，它包含了`total`字段来表示匹配到的文档总数，`hits`数组还包含了匹配到的前10条数据。

`hits`数组中的每个结果都包含`_index`、`_type`和文档的`_id`字段，被加入到`_source`字段中这意味着在搜索结果中我们将可以直接使用全部文档。这不像其他搜索引擎只返回文档ID，需要你单独去获取文档。

每个节点都有一个`_score`字段，这是**相关性得分(relevance score)**，它衡量了文档与查询的匹配程度。默认的，返回的结果中关联性最大的文档排在首位；这意味着，它是按照`_score`降序排列的。这种情况下，我们没有指定任何查询，所以所有文档的相关性是一样的，因此所有结果的`_score`都是取得一个中间值`1`

`max_score`指的是所有文档匹配查询中`_score`的最大值。

took   告诉我们整个搜索请求花费的毫秒数。

`_shards`节点告诉我们参与查询的分片数（`total`字段），有多少是成功的（`successful`字段），有多少的是失败的（`failed`字段）。通常我们不希望分片失败，不过这个有可能发生。如果我们遭受一些重大的故障导致主分片和复制分片都故障，那这个分片的数据将无法响应给搜索请求。这种情况下，Elasticsearch将报告分片`failed`，但仍将继续返回剩余分片上的结果。

`time_out`值告诉我们查询超时与否。一般的，搜索请求不会超时。如果响应速度比完整的结果更重要，你可以定义`timeout`参数为`10`或者`10ms`（10毫秒），或者`1s`（1秒）

```javascript
GET /_search?timeout=10ms
```

Elasticsearch将返回在请求超时前收集到的结果。



#### 2. 多索引和多类型

- 格式1： /index/_search，  如:  /foods/\_search
- 格式2： /index1， index2/\_search
- 格式3 ： /a\*, b\*/\_search    使用*通配符
- 格式4： /\_index1/\_type/\_search   在指定的索引和文档中搜索
- 格式5：  /index1， index2/\_type1, \_type2/\_search
- 格式6： /\_all/\_type1, \_type2/_search

#### 3. 分页

在ES中，接受`from`和`size`参数, 类似于MySQL的limit关键字一样。

```
size`: 结果数，默认`10
from`: 跳过开始的结果数，默认`0
```

如果你想每页显示5个结果，页码从1到3，那请求如下：

```javascript
GET /_search?size=5
GET /_search?size=5&from=5
GET /_search?size=5&from=10
```

#### 4. 查询字符串

两种写法：

- 查询字符串方式，将参数拼接到 url中
- json格式的请求体

**以上两种搜索写法，在ES中被称之为结构化查询语句(DSL)**。

用法：

- ```javascript
  GET /_all/text/_search?q=name:蒸
  GET /_all/text/_search?q=name:蒸+name:john+price:1.5
  ```

  `"+"`前缀表示语句匹配条件**必须**被满足。类似的`"-"`前缀表示条件**必须不**被满足。所有条件如果没有`+`或`-`表示是可选的——匹配越多，相关的文档就越多。

  

  查询结果:

  ```
  {
      "took": 10,
      "timed_out": false,
      "_shards": {
          "total": 10,
          "successful": 10,
          "skipped": 0,
          "failed": 0
      },
      "hits": {
          "total": 1,
          "max_score": 0.62191015,
          "hits": [
              {
                  "_index": "foods",
                  "_type": "text",
                  "_id": "3",
                  "_score": 0.62191015,
                  "_source": {
                      "name": "蒸饼",
                      "price": 1.5,
                      "images": "foods/3.jpg"
                  }
              }
          ]
      }
  }
  ```

_all 全字段搜索

```javascript
GET /_search?q=mary
```

## 二、集群配置

### 2.1 准备数据装载目录及配置文件

**创建数据挂载的目录**

```sh
mkdir -p es/config
```

`-p`表示递归创建es及config目录

**进入es目录下创建三个数据节点目录**

```shell
cd es
mkdir data1 data2 data3
```

**修改各个数据节点目录的文件权限为 775**

```sh
chmod 775 data1 data2 data3
```

在config目录下，创建与data1..3对应的三个配置文件。

**es1.yml**

```yaml
cluster.name: elasticsearch-cluster
node.name: es-node1
network.bind_host: 0.0.0.0
network.publish_host: 192.168.0.237
http.port: 9200
transport.tcp.port: 9300
http.cors.enabled: true
http.cors.allow-origin: "*"
node.master: true 
node.data: true  
discovery.zen.ping.unicast.hosts: ["192.168.0.237:9300","192.168.0.237:9301","192.168.0.237:9302"]
discovery.zen.minimum_master_nodes: 2
```

**es2.yml**

```yaml
cluster.name: elasticsearch-cluster
node.name: es-node2
network.bind_host: 0.0.0.0
network.publish_host: 192.168.0.237
http.port: 9201
transport.tcp.port: 9301
http.cors.enabled: true
http.cors.allow-origin: "*"
node.master: true 
node.data: true  
discovery.zen.ping.unicast.hosts: ["192.168.0.237:9300","192.168.0.237:9301","192.168.0.237:9302"]
discovery.zen.minimum_master_nodes: 2
```

**es3.yml**

```yaml
cluster.name: elasticsearch-cluster
node.name: es-node3
network.bind_host: 0.0.0.0
network.publish_host: 119.3.170.97
http.port: 9202
transport.tcp.port: 9302
http.cors.enabled: true
http.cors.allow-origin: "*"
node.master: true 
node.data: true  
discovery.zen.ping.unicast.hosts: ["192.168.0.237:9300","192.168.0.237:9301","192.168.0.237:9302"]
discovery.zen.minimum_master_nodes: 2
```

【注】192.168.0.237是服务器所在的内网ip地址

配置jvm的最大线程限制，在 /etc/sysctl.conf 文件中,增加以下内容:  

```
vm.max_map_count=262144 
```

以上配置避免在启动容器时报以下错误

```
bootstrap checks failed max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
```

更新sysctl服务

```
sysctl -p
```



### 2.2 启动镜像服务

启动 es1

```sh
docker run -dit -e ES_JAVA_OPTS="-Xms256m -Xmx256m"  -p 80:9200 -p 9300:9300 -v /root/es/config/es1.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /root/es/data1:/usr/share/elasticsearch/data --name ES01 02982be5777d
```

`-v` 表示映射的数据，宿主机下的配置文件与容器内的配置文件进行同步。

02982be5777d 是docker.elastic.co/elasticsearch/elasticsearch:6.7.0 镜像的id

启动es2

```sh
docker run -dit -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -p 9201:9201 -p 9301:9301 -v /root/es/config/es2.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /root/es/data2:/usr/share/elasticsearch/data --name ES02 02982be5777d
```

启动es3

```sh
docker run -dit -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -p 9202:9202 -p 9302:9302 -v /root/es/config/es3.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /root/es/data3:/usr/share/elasticsearch/data --name ES03 02982be5777d
```

【注】设置-e ES_JAVA_OPTS="-Xms256m -Xmx256m" 是因为/etc/elasticsearch/jvm.options 默认jvm最大最小内存是2G，读者启动容器后 可用docker stats命令查看.

### 2.3 测试

测试是在华为云服务器通过docker部署的，云服务器的外网ip是119.3.170.97 。

查看ES集群是否ok, 可以访问http://119.3.170.97/_cat/nodes查看

```
192.168.0.237 57 96 1 0.00 0.10 0.07 mdi - es-node2
192.168.0.237 41 96 1 0.00 0.10 0.07 mdi - es-node3
192.168.0.237 67 96 1 0.00 0.10 0.07 mdi * es-node1
```

显示以上信息，则表示集群部署成功。后期只需要针对第一个数据结点操作，会快速同步或分散到其它结点上。

还可以通过`docker stats ES01 ES02 ES03` 查看内存、CPU等使用情况。 





## 三. 扩展知识

> 每个容器在运行后，内部都有一个ip地址，即是容器内部的IP，它的作用可以让多个容器之间可以进行网络通信。

### 3.1 网络配置参数

在运行镜像时，可以通过以下参数来指定。

```
--hostname ：指定hostname;
--net : 指定网络模式
--ip：指定IP
--add-host ：指定往/etc/hosts添加的host
```

