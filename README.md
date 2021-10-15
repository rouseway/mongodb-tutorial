# 一、概述

MongoDB是一个由C++ 语言编写的 **基于分布式文件存储的数据库**

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

![](./images/form.png)



**① 与mysql的区别：**

- Mysql等数据库的是类似于Excel **表格式** 的数据，这种表格式的数据库也称之为**关系型数据库**，表叫做**关系表**。

- MongoDB数据库存储的是类似 **JSON** 格式的数据，称之为 **bson**，这种数据库由于比较自由，数据间并不一定有关系，我们称之为**非关系型数据库**。

  然后又由于不是表结构，我们不在使用sql语句去操作他，所以我们也称MongoDB为**nosql**数据库的一种。

**② MongoDB 特点：**高性能、易部署、易使用，存储数据非常方便。

**③ 诞生目的：**

> 其实，你可以认为 **MongoDB** 是 **mysql** 等关系型数据库的一种补充。
>
> 无可否认的，mysql等关系型数据库非常有用，并且在很多业务场景中都发挥了极大的作用。
>
> 但是，它并不是万能的。
>
> 在进入到web2.0时代（由用户主导而生成的内容互联网产品模式）后，mysql遇到了很多他没法实现或很难实现的问题，但是这个问题却又必须要解决，于是就诞生了nosql系列的非关系型数据，其中MongoDB就是一个代表。
>
> <u>**所以MongoDB的诞生目的就是为了补充mysql等关系数据库不足的，即用来完成mysql等数据库无法完成或很难完成的任务。**</u>

例如：

- 数据的高并发读写
- 数据的海量数据存储
- 数据的高可扩展性
- 数据的高可用性

# 二、下载

[点击前往官网下载中心 >>](https://www.mongodb.com/try/download/community)

![](./images/download.png)

根据系统，选择相应的安装包。

## 1. macOS

**① 将压缩包解压至任意位置，我放在 “/usr/local/” 目录下，并重新命名为 “mongodb”**

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
MongoDB shell version v4.2.2
git version: a0bbbff6ada159e19298d37946ac8dc4b497eadf
allocator: system
modules: none
build environment:
    distarch: x86_64
    target_arch: x86_64
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
bind_ip=127.0.0.1
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

\1. 打开mac 启动台 -> 其他 -> 活动监视器，删除 mongodb 服务

\2. 在data/db/目录下删除 mongod.lock 文件

\3. 终端执行：mongod --repair

\4. 根据配置文件启动：mongod -f /etc/mongodb.conf

## 2. Windows

### 2.1. 安装

**① 下载 msi 文件之后，双击安装**

**② 修改安装目录**

默认会安装到C盘，你可以修改安装目录：

![](./images/cg_install_loc_1.png)

![](./images/cg_install_loc_2.png)

**③. 取消勾选 Install MongoDB Compass**

在安装过程中会出现如下界面，其中**install mongoDB compass**"不要勾选需要去掉，否则可能要很长时间都一直在执行安装。

MongoDB Compass 是一个图形界面管理工具，我们可以在后面自己到官网下载安装，下载地址：https://www.mongodb.com/download-center/compass。

![](./images/no_compass.png)



**④ 安装完成**

- 安装完成后在mongodb的bin目录的同级下创建 **data/db** ，如：E:\软件\MongoDB\data\db（db目录需自行创建）
- 然后将bin目录配置进入**环境变量**。

**⑤ 最后启动**

1. 以管理员身份打开cmd，执行命令： **mongod --dbpath db的路径**（路径如果有空格记得加上引号）
2. 最后执行mongo连接数据库，开启交互模式。

[参考示例 >>](https://www.runoob.com/mongodb/mongodb-window-install.html)

### 2.2. 基本使用

**①. 启动mongoDB 服务**

```shell
 $ mongod.exe --dbpath  E:\软件\MongoDB\data\db
```

**②. 链接服务**

```shell
$ mongo.exe
```



# 三、概念解析

| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                           |
| :----------- | :--------------- | :---------------------------------- |
| database     | database         | 数据库                              |
| table        | collection       | 数据库表/集合                       |
| row          | document         | 数据记录行/文档                     |
| column       | field            | 数据字段/域                         |
| index        | index            | 索引                                |
| table joins  |                  | 表连接,MongoDB不支持                |
| primary key  | primary key      | 主键,MongoDB自动将_id字段设置为主键 |

通过下图实例，我们也可以更直观的了解Mongo中的一些概念：

![](./images/concept.png)





