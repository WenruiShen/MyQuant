## Brief Tutorial for MongoDB Modules

[原文 -> Github](https://github.com/WenruiShen/MyQuant)

[转载请注明！]

Author: Shen Wenrui

Email:  Thomas.shen3904@qq.com

### 1. Installation:
MongoDB 提供了 OSX 平台上 64 位的安装包，你可以在官网下载安装包。

[下载地址](https://www.mongodb.com/download-center#community)


> vim ~/.bash_profile

    添加一行: export PATH=/Users/mobike/Documents/EG/mongodb-osx-x86_64-3.6.3/bin:$PATH

> source ~/.bash_profile

或者直接:

> sudo brew install mongodb

### 2. 运行 MongoDB:

##### 2.1 首先我们创建一个数据库存储目录 /data/db：

> sudo mkdir -p /data/db
>
> sudo chmod 777 /data/db

启动 mongodb，默认数据库目录即为 /data/db：

> mongod

常用的启动参数：

    --dbpath：指定存储数据的文件夹
    --logpath：指定日志存储文件
    --logappend：日志以增加方式产生
    --port指定端口，如果不写的话，默认是27017
    --fork代表后台运行
   
再打开一个终端进入执行以下命令：

> mongo

    MongoDB shell version v3.4.2
    connecting to: mongodb://127.0.0.1:27017
    MongoDB server version: 3.4.2
    Welcome to the MongoDB shell.
    ……
    > 1 + 1
    2
    > 

** 注意：如果你的数据库目录不是/data/db，可以通过 --dbpath 来指定。**


##### 2.2 停止Mongodb:

在客户端进去，使用shutdown命令

> use admin;
>
> db.shutdownServer();

### 3 References:
[1].http://www.runoob.com/mongodb/mongodb-osx-install.html

[2].[mongoDB 启动与停止](https://blog.csdn.net/leshami/article/details/52371395)

[3].[MongoDB的启动与停止（一）](https://www.cnblogs.com/lemon-le/p/7132038.html)

[4].[MongoDB 教程 (菜鸟学院)](http://www.runoob.com/mongodb/mongodb-tutorial.html)
