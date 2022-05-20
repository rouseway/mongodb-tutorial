# 一、概述

[mongodb-docs >>](https://www.mongodb.com/docs/)

[mongoosejs-docs >>](https://mongoosejs.com/docs/guide.html)

MongoDB是一个由C++ 语言编写的 **基于分布式文件存储的数据库**

MongoDB 将数据存储为一个文档，数据结构由键值（`key:value`）对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

![](./images/form.png)

## 1. vs mySql

- Mysql 数据库是类似于Excel **表格式** 的数据，这种表格式的数据库也称之为 **关系型数据库**，表叫做**关系表**。

- MongoDB 数据库存储的是类似 **JSON** 格式的数据，称之为 **bson**，这种数据库由于比较自由，数据间并不一定有关系，我们称之为**非关系型数据库**。然后又由于不是表结构，我们不再使用sql语句去操作他，所以我们也称MongoDB为 **nosql** 数据库的一种。

## 2. 特点

高性能、易部署、易使用，存储数据非常方便。

## 3. 诞生目的

**MongoDB** 是 **mysql** 等关系型数据库的一种补充。无可否认的，mysql等关系型数据库非常有用，并且在很多业务场景中都发挥了极大的作用。但是，它并不是万能的。在进入到web2.0时代（由用户主导而生成的内容互联网产品模式）后，mysql遇到了很多他没法实现或很难实现的问题，但是这个问题却又必须要解决，于是就诞生了nosql系列的非关系型数据，其中MongoDB就是一个代表。

所以MongoDB的诞生目的就是为了补充mysql等关系数据库不足的，即用来完成mysql等数据库无法完成或很难完成的任务。例如：

- 数据的高并发读写
- 数据的海量数据存储
- 数据的高可扩展性
- 数据的高可用性

## 4. 概念

| SQL术语/概念  | MongoDB术语/概念 | 解释/说明                              |
| :------------ | :--------------- | :------------------------------------- |
| `database`    | `database`       | 数据库                                 |
| `table`       | `collection`     | 数据库表/集合                          |
| `row`         | `document`       | 数据记录行/文档                        |
| `column`      | `field`          | 数据字段/域                            |
| `index`       | `index`          | 索引                                   |
| `table joins` |                  | 表连接，MongoDB不支持                  |
| `Primary key` | `Primary key`    | 主键，MongoDB自动将`_id`字段设置为主键 |

> [参照 SQL 到 MongoDB 的映射图标 >>](https://docs.mongoing.com/mongodb-crud-operations/sql-to-mongodb-mapping-chart)



# 二、安装、启动

[点击前往官网下载中心 >>](https://www.mongodb.com/try/download/community)

![](./images/download.png)

根据系统，选择相应的安装包。

## 1. macOS

**① 将压缩包解压至任意位置，我放在 *`/usr/local/`* 目录下，并重新命名为 *`mongodb`***

**② 添加环境变量**

```shell
$ open ~/.bash_profile
```

```shell
export PATH=$PATH:/usr/local/mongodb/bin 
```

**③ 查看版本，如果正常显示版本号则安装成功**

```shell
$ mongo --version
MongoDB shell version v5.0.3
Build Info: {
    "version": "5.0.3",
    "gitVersion": "657fea5a61a74d7a79df7aff8e4bcf0bc742b748",
    "modules": [],
    "allocator": "system",
    "environment": {
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```

**# Brew 下载**

[参考地址 >>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

```shell
$ brew tap mongodb/brew
$ brew install mongodb-community@4.2
$ mongo --version
MongoDB shell version v4.2.2
git version: a0bbbff6ada159e19298d37946ac8dc4b497eadf
allocator: system
modules: none
build environment:
    distarch: x86_64
    target_arch: x86_64
```

### 1.2. 创建目录

```shell
# 进入安装目录
$ cd /usr/local/mongodb/
# 创建日志输出、数据库目录
$ mkdir -p data/logs data/db
# 进入日志输出目录并创建日志文件
$ cd /data/logs && touch mongodb.log
```

### 1.3. 配置文件

```shell
$ vim /etc/mongodb.conf
```

按 `i` 输入如下内容：

```ini
# 数据库存放地址
dbpath=/usr/local/mongodb/data/db
# 日志输出文件路径
logpath=/usr/local/mongodb/data/logs/mongodb.log
# 错误日志采用追加模式，配置这个选项后mongodb的日志会追加到现有的日志文件，而不是从新创建一个新文件
logappend=true
# 启用日志文件，默认启用
journal=true
# 这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false
quiet=true
# 端口号
port=27017
# 守护进程
fork=true
# 绑定ip
bind_ip=0.0.0.0
# 开启认证
auth=true
```

按 `ESC` 输入 `:wq!` 保存退出！

### 1.4. 运行

```shell
# 通过配置文件启动mongodb
$ mongod -f /etc/mongodb.conf
```

如果出现successful就表示服务已经启动成功

浏览器输入：

```
http://localhost:27017/
```

网页呈现如下信息即表示成功：

```markdown
It looks like you are trying to access MongoDB over HTTP on the native driver port.
```

> 提示：后续使用mongodb只需要打开终端输入mongo指令即可。

### 1.5. 重启服务

1）打开mac 启动台 → 其他 → 活动监视器 → 删除 mongodb 服务

2）在 *`data/db/`* 目录下删除 mongod.lock 文件

3）终端执行：`mongod --repair`

4）根据配置文件启动：`mongod -f /etc/mongodb.conf`

## 2. Windows

### 2.1. 安装

**① 下载 msi 文件之后，双击安装**

**② 修改安装目录**

默认会安装到C盘，你可以修改安装目录：

![](./images/cg_install_loc_1.png)

![](./images/cg_install_loc_2.png)

**③. 取消勾选 Install MongoDB Compass**

在安装过程中会出现如下界面，其中 **install mongoDB compass** 不要勾选需要去掉，否则可能要很长时间都一直在执行安装。

MongoDB Compass 是一个图形界面管理工具，我们可以在后面自己到官网下载安装，[前往下载 >>](https://www.mongodb.com/download-center/compass)。

![](./images/no_compass.png)



**④ 安装完成**

- 安装目录下创建db目录：*`E:\MongoDB\data\db`*
- 然后将 bin 目录配置进入**环境变量**，配置步骤：右建此电脑 → 属性 → 高级系统设置 → 环境变量

### 2.2. 配置文件

默认安装目录在：*`<install directory>\bin\mongod.cfg`*

```ini
# mongod.conf

# for documentation of all options, 
# see: http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: E:\MongoDB\data
  journal:
    enabled: true

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path:  E:\MongoDB\log\mongod.log
  quiet: true

# network intierfaces
net:
  port: 27017
  bindIp: 0.0.0.0

# security
security:
  authorization: enabled
```

### 2.3. 启动服务

打开终端，以 **管理员身份运行**：

```shell
# 命令行参数启动
$ mongod.exe --dbpath E:\MongoDB\data\db --serviceName "MongoDB" --install
# 配置文件方式启动
$ mongod --config "E:\MongoDB\bin\mongod.cfg" --serviceName "MongoDB" --install
```

```shell
# 查看服务
services.msc
# 启用服务
net start MongoDB
# 停止服务
net stop MongoDB
```

> **Tips：** 使用管理员模式运行终端，切记切记！

# 三、管理构建

## 1. 用户管理

1）基础命令

```markdown
# 1. 创建用户
db.createUser({user:'账号', pwd:'密码', roles:[{role:'角色名', db: '数据库名'}]})
# 2. 查看用户
db.getUsers()
# 3. 移除用户
db.dropUser("用户名")
# 4. 用户登陆
db.auth(user, pwd) 
# 5. 修改用户密码
db.changeUserPassword('用户名', '新密码')
```

代码示例：

```mysql
# 创建超级用户
> use admin
> db.createUser({user:'root', pwd:'123', roles:[{role:'root', db: 'admin'}]})
# 创建普通用户
> db.createUser({user:'lee', pwd:'123', roles:[{role:'readWrite', db: 'DB-TEST'}]})
```

> 提示：创建普通用户时需先登陆超级用户再创建。

2）角色分类

- 数据库用户角色：`read`、`readWrite`
- 数据库管理员角色：`dbAdmin`、`dbOwner`、`userAdmin`
- 集群管理员角色：`clusterAdmin`、`clusterManager`、`clusterMonitor`、`hostManager`
- 备份和恢复角色：`backup`、`restore`
- 所有数据库角色：`readAnyDatabase`、`readWriteAnyDatabase`、`userAdminAnyDatabase`、`dbAdminAnyDatabase`
- 超级用户角色：`root`

3）角色功能

- `read`：允许用户读取指定数据库
- `readWrite`：允许用户读写指定数据库
- `readAnyDatabase`：只在`admin`数据库中可用，赋予用户所有数据库的读权限
- `readWriteAnyDatabase`：只在`admin`数据库中可用，赋予用户所有数据库的读写权限
- `dbAdmin`：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问`system.profile`
- `dbAdminAnyDatabase`：只在`admin`数据库中可用，赋予用户所有数据库的`dbAdmin`权限。
- `userAdmin`：允许用户向`system.users`集合写入，可以找指定数据库里创建、删除和管理用户
- `userAdminAnyDatabase`：只在`admin`数据库中可用，赋予用户所有数据库的`userAdmin`权限
- `clusterAdmin`：只在`admin`数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
- `root`：只在`admin`数据库中可用。超级账号，超级权限

## 2. 数据库操作

```markdown
# 1. 查看数据库
show dbs
# 2. 创建/切换数据库
use <数据库名>
# 3. 查看当前数据库
db
# 4. 删除数据库
db.dropDatabase()
# 5. 查看服务器地址
db.getMongo()
```

> 提示：刚创建的数据库需要插入数据才能够显示。

## 3. 集合操作（表）

1）常用指令：

```markdown
# 1. 查看集合帮助
db.<数据库名>.help()
# 2. 创建集合
db.createCollection(name, options)
# 3. 创建集合并插入一个文档
db.COLLECTION_NAME.insert(文档对象)
# 4. 查看集合
show collections / show tables
# 5. 删除集合
db.COLLECTION_NAME.drop()
# 6. 修改表名
db.COLLECTION_NAME.renameCollection("NEW_NAME");
```

2）示例：

```shell
# 创建表
db.createCollection('usrs', {capped: true, size:6142800, max: 10000})
```

3）附录说明：

创建集合options说明：

| 字段          | 类型      | 描述                                                         |
| ------------- | --------- | ------------------------------------------------------------ |
| `capped`      | `Boolean` | （可选）如果为true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。<br />**注意：当改值为  true 时，必须指定size 参数。** |
| `autoIndexId` | `Boolean` | （可选）如果为true，自动在 **_id** 字段创建索引，默认为 `false`。 |
| `size`        | `Number`  | （可选）为固定集合指定一个最大值，以千字节（KB）<br />**如果capped 为true，也需要指定该字段。** |
| `max`         | `Number`  | （可选）指定固定集合中包含文档的最大数量。                   |

> 提示：
>
> - 在插入文档时，MongoDB 首先检查固定集合的 size 字段，然后检查 max 字段，然后autoIndexId属性已经被遗弃了，未来将会被删除。
> - 在 MongoDB 中，其实当你插入一些文档时，MongoDB 也会自动创建集合。

# 四、CURD

> **Tips：** 示例基于 `inventory` 表操作。

## 1. 插入文档

```mysql
# 插入单个文档
db.inventory.insertOne(document)
# 插入多个文档
db.inventory.insertMany([document...])
```

**Tips：**插入文档时如果没有指定id，MongoDB会自动生成 `_id`，类型为 `ObjectId`

## 2. 查询文档

### 基础查询

示例数据：

```mysql
db.inventory.insertMany([
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "A" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" }
]);
```

代码示例：

```mysql
# 选择集合中的所有文档
db.inventory.find({})
# 返回 status = D 的数据
db.inventory.find( { status: "D" } )
# 返回 status = A 或者 D 的数据
db.inventory.find( { status: { $in: [ "A", "D" ] } } )
# $and → 复合查询，返回 status = A 并且 qty < 30 的数据
db.inventory.find( { status: "A", qty: { $lt: 30 } } )
# $or → 返回 status = A，或者  qty < 30 的数据
db.inventory.find( { $or: [ { status: "A" }, { qty: { $lt: 30 } } ] } )
# $and 和 $or 组合查询
# 示例：查询 status = A 并且 （qty < 30 或者 item 以 p 开头） 的数据
db.inventory.find( {
     status: "A",
     $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]
} )
```

**数组查询**

示例数据：

```mysql
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```

代码示例：

```mysql
# 条件查询，tags = ["red", "blank"] 的数据
db.inventory.find( { tags: ["red", "blank"] } )
# $all → 包含查询，只要 tags 包含 red 和 blank，都返回
db.inventory.find( { tags: { $all: ["red", "blank"] } } )
# 查询 tags 中包含 red 的数据
db.inventory.find( { tags: "red" } )
# 查询 dim_cm 集合中至少有一个值大于 25 对的数据
db.inventory.find( { dim_cm: { $gt: 25 } } )
```

维数组元素指定多个条件：

```mysql
# dim_cm 的元素可以大于15或者小于20或者两个都满足
db.inventory.find( { dim_cm: { $gt: 15, $lt: 20 } } )
```

### 附录1：查询操作符

- `$in`：查询值为指定集合中某个元素时
- `$nin`：查询值不为指定集合中某个元素时
- `$gt[e]`：大于[等于]某个值
- `$lt[e]`：小于[等于]某个值
- `$ne`：不等于某个值
- `$or`：或查询
- `$all`：匹配所有

### 数据映射

```mysql

# 指定返回字段，比如只返回 name/sex，通过0/1控制映射
db.users.find({ name: "李白" }, { name: 1, sex: 1} )
# 去除_id字段
db.users.find({ name: "李白" }, { name: 1, sex: 1, _id: 0 } )
# 去除指定字段，比如返回数据中去除 interest 和 location 字段
db.users.find({ name: "李白" }, { interest: 0, location: 0  } )
# 映射返回数组中指定的数组元素，$slice 表示截取最后1个元素，不能使用下标
db.users.find({ name: "李白" }, { interest: { $slice: -1 } } )
```

> `！Tips`
>
> - 除 `_id` 字段外，不能在映射文档中同时使用包含和去除语句。

## 3. 更新文档

语法解读：

```markdown
# 1. 更新一个文档
db.COLLECTION_NAME.updateOne(query, update, options)
# 2. 更新多个文档
db.COLLECTION_NAME.updateMany(query, update, options)
```

语法解读：

- `query` ：查询条件
- `update`：更新后的对象或指定一些更新的操作符
- `options`：可选项
  - `upsert`：可选，未查询到时是否插入updateObj，默认false。
  - `multi`：可选，是否更新所有查询到的文档，默认false。

**操作符**

##### $inc

在原基础上累加

```js
{ $inc: { <field1>: <amount1>, <field2>: <amount2>, ... } }
```

```mysql
db.users.updateOne({name:'张三'}, {$inc: { age: 10 }})
```

代码描述：查询 name 为张三的记录，并将其 age 字段累加10

##### $push

向数组中添加元素，不会覆盖已有的

```js
{ $push: { <field1>: <value1>, ... } }
```

```mysql
db.users.updateOne({name:'张三'}, {$push: { interest : "爬山" }})
```

代码描述：查询 name 为张三的记录，并在 interest 中追加 爬山 

##### $addToSet

给数组添加或者设置一个值

```js
{ $addToSet: { <field1>: <value1>, ... } }
```

##### $set

更新字段

```js
{ $set: { <field1>: <value1>, ... } }
```

```mysql
db.users.updateOne({name:'张三'}, {$set: { sex : "女" }})
```

代码解读：查询 name 为张三的记录，并将其 sex 字段更新为 女。

## 4. 删除文档

```markdown
# 1. 删除单个文档
db.COLLECTION_NAME.deleteOne(query)
# 2. 删除多个文档
db.COLLECTION_NAME.deleteMany(query)
```

# 五、聚合

查询文档可以使用 `.find()` 方法，这里主要推荐高级用法：聚合管道查询 → [aggregation >>](https://www.mongodb.com/docs/manual/reference/operator/aggregation/)

其思想是将文档（`Document`）在一个管道处理完毕后将结果传递给下一个管道处理

聚合主要用于处理数据（诸如统计平均值，求和等），并返回计算后的数据结果。聚合管道主要包含如下API：

| 命令         | 功能描述                                                     |
| ------------ | ------------------------------------------------------------ |
| *`$match`*   | 用于过滤数据，只输出符合条件的文档，与 `find()` 类似，基于MongoDB的标准查询操作 |
| *`$project`* | 修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档 |
| *`$skip`*    | 在聚合管道中跳过指定数量的文档                               |
| *`$limit`*   | 用来限制MongoDB聚合管道返回的文档数                          |
| *`$unwind`*  | 将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值 |
| *`$group`*   | 将集合中的文档分组，可用于统计结果                           |
| *`$lookup`*  | 引入其他集合的数据（表关联查询）                             |
| *`$sort`*    | 将输入文档排序后输出 *`1`：升序，`-1`：降序，默认为升序显示* |

> **Tips：**同样的操作符以及条件, 不同的排列顺序对查询结果会有影响

插入测试数据：

```mysql
db.users.insertMany([
  {"name":"张三", "sex": "男", "age": 31, "phone": "15666666666", "job":"前端工程师", "interest": ["烹饪", "运动"]},
  {"name":"李四", "sex": "女", "age": 16, "phone": "15777777777", "job":"后端工程师", "interest": ["追剧", "旅行"]},
  {"name":"赵二", "sex": "男", "age": 28, "phone": "15888888888", "job":"测试工程师", "interest": ["游戏", "摄影"]},
  {"name":"王五", "sex": "女", "age": 38, "phone": "15999999999", "job":"运维工程师", "interest": ["运动", "旅行"]}
])
```

##### $match

*`$match`* 用于过滤数据，只输出符合条件的文档

```markdown
# 1. 查询所有
db.users.aggregate() 
# 2. 查询 name = '张三' 的数据
db.users.aggregate([{ $match: { name: '张三' } }])
# 3. 查询年龄大于30的数据
db.users.aggregate([{ $match: { age: { $gt: 30 } } }])
```

##### $project

*`$project`* 指定输出文档里的字段，`1` 为显示，`0` 为不显示，除 `_id` 外，其他任意字段之间 **不可以** `0` 和 `1` 混用！

```mysql
db.users.aggregate([
    { $match: { name: "张三" } },
    { $project: { _id: 0, id: "$_id", name: 1, job: 1} }
])
```

输出结果：

```json
{ 
  name: '张三',
  job: '前端工程师',
  id: ObjectId("6281c0e0d73b9db975426f2e") 
}
```

##### \$skip、\$limit、$sort

列表分页一般会联合这三个操作符使用，其中

- *`$skip`*：指定跳过多少条记录
- *`$limit`*：每次查询条数

- *`$sort`*：排序 

> **Tips：**skip 的计算方式：(当前页码-1)*每页大小 如：`(pageIndex - 1) * pageSize`

```mysql
db.users.aggregate([
    { $match: {} },
    { $skip: 1 },   
    { $limit: 3 },  
    { $sort: { age: -1 } }
])
```

##### $lookup

[$lookup](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/) 表关联查询，语法结构如下：

```
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}
```

参数解读：

- `from`：同一个数据库下等待被Join的集合
- `localField`：源集合中的match值
- `foreignField`：待Join集合中的match值
- `as`：输出字段

**4）联合查询**

```markdown
# 1. and 查询，即指定多个键值
db.COLLECTION_NAME.find({ field1: value1, field2: value2 })
# 2. or 查询
db.COLLECTION_NAME.find({ $or: [{field1: value1}, {field2:value2} ] })
```



## 6. 索引

### 6.1. 概述

索引通常能够极大的提高查询的效率，如果没有索引，MongoDB 在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。

这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。

索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构

### 6.2. API

```markdown
# 1. 创建索引/v3.0之前
db.collection_name.ensureIndex(keys, options)
# 2. 创建索引/v3.0之后
db.collection_name.createIndex(keys, options)
# 3. 查看集合索引
db.collection_name.getIndexes()
# 4. 查看集合索引大小
db.collection_name.totalIndexSize()
# 5. 删除集合所有索引
db.collection_name.dropIndexes()
# 6. 删除集合指定索引
db.collection_name.dropIndex("索引名称")
```

> 语法提示：语法中 Key 值为你要创建的索引字段，1 为指定按升序创建索引，如果你想按降序来创建索引指定为 -1 即可。

options 参数类型：

| Parameter          | Type          | Description                                                  |
| :----------------- | :------------ | :----------------------------------------------------------- |
| background         | Boolean       | 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 "background" 默认值为**false**。 |
| unique             | Boolean       | 建立的索引是否唯一。指定为true创建唯一索引。默认值为**false**. |
| name               | string        | 索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。 |
| dropDups           | Boolean       | **3.0+版本已废弃。**在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 **false**. |
| sparse             | Boolean       | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 **false**. |
| expireAfterSeconds | integer       | 指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。 |
| v                  | index version | 索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。 |
| weights            | document      | 索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。 |
| default_language   | string        | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语 |
| language_override  | string        | 对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 language |

**# 注意：**

利用 TTL 集合对存储的数据进行失效时间设置：经过指定的时间段后或在指定的时间点过期，MongoDB 独立线程去清除数据。类似于设置定时自动删除任务，可以清除历史记录或日志等前提条件，设置 Index 的关键字段为日期类型 new Date()。

### 6.3. 索引类型

**1）_id 索引**

创建集合时，默认情况下MongoDB都会帮助我们创建一个名为`_id`的字段，这个字段就是一个索引。特别注意的是，固定集合不会将 _id 默认作为索引。

**2）复合索引**

如果我们的查询条件有多个的话，我们可以对这多个查询条件都建立索引，比如我们可以对heros文档中的position和proficient字段都建立索引，如下：

```mysql
> db.heros.createIndex({position:1, proficient:1})
```

此时执行如下查询语句时就会用到这个复合索引：

```mysql
> db.heros.find({position:"法师", proficient:{$gt:1000}})
```

上述示例查询英雄定位为法师并且熟练度在1000以上的数据。

**3）过期索引**

过期索引就是一种会过期的索引，在索引过期之后，索引对应的数据会被删除，创建方式如下：

```mysql
> db.collection_name.createIndex({time:1},{expireAfterSeconds:30})
```

expireAfterSeconds表示索引的过期时间，单位为秒。time表示索引的字段，time的数据类型必须是ISODate或者ISODate数组，否则的话，当索引过期之后，time的数据就不会被删除。

**4）全文索引**

[参考地址 >>](https://docs.mongodb.com/manual/reference/text-search-languages/)

MongoDB 从3.2 版本以后添加了对中文索引的支持。下面给大家举一个例子，我的数据是这样的：

```
{"name":"阿珂", "position": "刺客", "location": "野区", "speciality":"突进/收割"}
{"name":"关羽", "position": "战士", "location": "上路", "speciality":"突进/先手"}
{"name":"婉儿", "position": "法刺", "location": "中路", "speciality":"突进/收割"}
{"name":"蒙犽", "position": "射手", "location": "下路", "speciality":"远程/消耗"}
{"name":"李白", "position": "刺客", "location": "野区", "speciality":"突进/收割"}
```

我们可以给 speciality 字段添加一个全文索引，值为 `text`

```mysql
> db.heros.createIndex({speciality: "text"});
```

MongoDB会自动对speciality字段的数据进行分词，然后我们就可以通过如下语句进行查询：

```mysql
> db.heros.find({$text:{$search:"突进"}})
```



# 六、内置运算符

[参考指南 >>](https://docs.mongoing.com/can-kao/yun-suan-fu)

# 七、可视化工具

1. [前往下载 Nacicat Premium >>](https://www.navicat.com.cn/products/navicat-premium)
2. [前往下载 MongoDB Compass >>](https://www.mongodb.com/try/download/compass)
3. [前往下载 Robo 3T >>](https://studio3t.com/download/)

参考指南：

- [Navicat Premium 16.0.10 破解安装指南 >>](https://cloud.tencent.com/developer/article/1953103)

# 八、扩展

## 1. 忘记密码

### macOS

```shell
sudo vim /etc/mongodb.conf     # 修改 mongodb 配置，将 auth = true 注释掉，或者改成 false
...							   # 参照上述示例，重启mongo服务
mongo                          # 运行客户端（也可以去mongodb安装目录下运行这个）
use admin                      # 切换到系统帐户表
db.getUsers()                  # 查看当前帐户（密码有加密过）
db.system.users.remove({})     # 删除所有帐户
db.addUser('admin','password') # 添加新帐户

sudo vim /etc/mongodb.conf     # 恢复 auth = true
service mongodb restart        # 重启 mongodb 服务
...							   # 参照上述示例，重启mongo服务
```







