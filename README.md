# 一、概述

数据库分SQL（关系型数据库）和NoSQL（非关系型数据库）。

MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统 => NoSQL。

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

![](./images/form.png)



# 二、下载

## 1. macOS

### 1.1. 下载

**# [官网下载](https://www.mongodb.com/download-center/community)**

\1. 官网下载相应版本

![](./images/download.png)

\2. 将压缩包解压至任意位置，我放在 “/usr/local/” 目录下，并重新命名为 “mongodb”

\3. 添加环境变量

```shell
$ open ~/.bash_profile
```

\4. 添加如下代码

```shell
export PATH=$PATH:/usr/local/mongodb/bin 
```

\5. 查看版本，如果正常显示版本号则安装成功

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

[参考示例 >>](https://www.runoob.com/mongodb/mongodb-window-install.html)

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





