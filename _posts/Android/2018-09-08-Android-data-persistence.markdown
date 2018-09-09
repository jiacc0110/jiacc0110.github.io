---
layout: post
title:  "Android数据持久化"
date:   2018-09-08 19:13:19 +0800
categories: jekyll update
---
Android的数据持久化
---
Android数据持久化是Android开发基础知识之一。包括几个方面。所谓数据持久化就是**内存中数据模型转换为存储模型，以及存储模型转为内存数据的统称**。

Android数据持久化包括以下几个方面：
1. SharedPerferances
2. 文件存储（内部存储和外部存储）
3. Sqlite存储
4. 网络存储

另外，网络上常有将ContentProvider作为数据持久化方式之一，但是ContentProvider作为夸进程交互的接口，并没有与存储介质交互，这里不做讨论。

### SharedPreferances存储
SharedPreferances存储是一个轻量级的存储，可以存储键值对，用法非常简单就不说了。
SharedPerferances的存储对应的是一个XML文件,此文件存储在内部存储器的专属文件夹下面。
![datapersistence]({{ site.url }}/imgs/datapersistence/20180908213332.png)

对应SharedPreferces的存储，需要先获得一个Editor实例，通过Eidtor将要提交的键值对加进去，最后使用**commit()** 或者 **apply()** 进行一个提交。
> commit() 与 apply() 方法的选择需要注意一下，commit的提交是同步的，即将增加的键值对同步到XML文件后，返回一个true。这之中包括IO的读取操作，如果在主线程中容易造成卡顿。而apply()是将键值对同步到缓存的map中，然后开启工作线程同步到XML文件的一个异步操作。
**所以，在主线程中使用时，一定要使用apply()**


### 文件存储
文件存储的关键，是分清楚**内部存储**和**外部存储**的物理概念，然后选择对应的api，获取io流或者文件夹进行文件操作。需要注意的是，内部存储一定是在手机机身之中，外部存储设备在早期的版本就是指SD卡，4.4以上的手机，外部存储设备可能是SD卡，也可能是内嵌在机身的外部存储设备。

> 什么是APP专属文件？</br>
>所谓专属文件就是它是属于某个具体的应用的，他的**文件路径都带有相应的包名**，当APP卸载时，它们会随应用一起删除。

我们在存储的时候，要根据需要选择不同的API，获取不同的文件夹实例。
![专属文件夹]({{site.url}}/imgs/datapersistence/20180908215938.png)

内部存储保存位置一定是在专属文件夹下，外部存储不一定。对于一些多媒体文件例如相片等，是不应该跟同app一起删除的，这些文件要存在外部存储的共享文件夹下。api的使用可以参[考官方文档](https://developer.android.google.cn/guide/)

### Sqlite存储
#### 1. Sqlite基本用法
1. 创建SQLiteOpenHelper，onCreate()  第一次用数据库时执行，在里面创建表操作 ，onUpgrade()   数据库版本更新时执行，在里面进行升级。

2. 获取SQLiteDatabase数据库实例，进行增删改查的操作。可以使用封装好的增删改查方法，也可以使用SQL语句

#### 2. Room框架的使用
谷歌官方推荐使用Room框架代替SQLite。[看这里](https://developer.android.google.cn/training/data-storage/room/
)

![room框架]({{site.url}}/imgs/datapersistence/20180908220911.png)

>基本用法：
1. 创建数据库类继承自RoomDatabase,用@dababase注解修饰；
2. 创建DAO数据访问对象接口，用@dao注解修饰；
3. 修改实体类，用@Entity注解修饰字段，并用对于注解修饰各个字段。
4. 根据业务调用

 优势：对象关系映射（ORM）方便的完成数据库和实体类之间的转换，不需要写中间的实例化过程和SQL操作。只需要定义接口，注解处理器帮助我们完成实例化和赋值操作。

 原理：在javac阶段 ，使用注解处理器（在Gradle中依赖的AnnotationProcessor），
将我们自己写的接口类DAO、抽象类AppDatabase，自动生成DAO_IMPL、 AppDatabase_IMPL实现类,我们无需对Entity类做任何操作，省去了自己写SQL语句。

![room框架原理]({{site.url}}/imgs/datapersistence/20180908221309.png)









### 网络存储

常用的网络库 socket、
    HttpURLConnection、
    HttpClient(Android6.0开始弃用)、
    Volley、
    OkHttp、
    native层的网络库

 各个网络库的选择、使用、支持的功能、内部的原理，可以专门作为一个专题，  这里不做过多介绍。
