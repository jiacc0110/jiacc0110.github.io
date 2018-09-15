---
layout: post
title:  "NDK"
date:   2018-09-15 23:13:19 +0800
categories: jekyll update
---
Bionic 是Android平台为C和C++进行原生应用开发所提供的POSIX标准C库。

Bionic提供了什么：
- 内存管理
- 文件输入输出
- 字符串操作
- 数学
- 日期和时间
- 进程控制
- 信号处理
- 网络套接字
- 多线程
- 用户和数组
- 系统配置
- 命名服务切换

缺什么：
 Binoic是专门为Android平台进行移动计算而设计的。Binoic不会支持所以的标准库。缺失的函数会在缺失的函数会在Android_NDK_HOME目录下的platforms/android-<api-level>/arch-<architecture>/usr/include中头文件中标明。

### 一、内存管理
 ##### c语言动态内存管理
1. 在C中分配动态内存
 void * malloc(size_t size)
```
 #include <stdlib.h>
   ...
   //分配16个元素的整形数组
  int * dynamicIntArray = (int *) malloc(sizeof(int) * 16);
  if(NULL == dynamicIntArray){
    /* 不能分配足够的内存*/
  }else{
    /*通过整形指针使用内存*/
    *dynamicIntArray = 0;
    dynamicIntArray[8]=8;
    ...
    free(dynamicIntArray);
    dynamicIntArray = NULL;
  }
```
2. 释放内存
  void free（void * memory)

3. 改变C语言中动态分配的内存
  void * realloc(void * memory,size_t size)
 若重新分配成功，保存原来的内容并移到一个新位置上；若失败，保留原来分配的动态内存分配不变并返回NULL。


 ##### C++的动态内存管理

 1. C++中动态内存分配
  用new关键字后面紧跟的数据类型来分配内存。
  ```
    int * dynamicInt = new int;
    if(NULL == dynamicInt){
      /*不能分配足够内存*/
    } else{
      /*使用分配的内存*/
      *dynamicInt = 0；
    }
  ```
  2. 释放C++中的动态内存
  delete dynamicInt;

### 二、标准I/O

  1）stdin 标准输入流

  2）stdout 标准输出流

  3）stderr 标准错误流

 FILE * fopen(const char* filename,const char* opentype);
 opentype的值：
 + r:只读方式打开
 + w：只写方式打开
 + a：附加方式打开
 + r+：在读写模式下打开
 + w+：打开文件进行读写和附加。

 具体API的使用方式，此处不再详述。

### 三、与进程交互

##### 执行shell命令
  可以用system函数向shell传递命令。
  ```
  #include <stdlib.h>

  int result;
  /*执行shell命令*/
  result = system("mkdir /data/data/com.example.hellojni/temp");
  if(-1 == result || 127 == result){
    /*执行shell命令失败*/
  }

  ```
##### 与子进程通信
  可以使用popen函数在父进程和子进程之间打开一个双向通道。
  FILE *pOpen(const char* command,const char* type);

  ```
  #include <stdio.h>
  ...
  FILE* stream;

  /*向ls命令打开一个只读通道*/
  ```
  stream = popen("ls","r");
  if(NULL == steam){
    MY_LOG_ERROR("Unable to execute the command.");
  }else{
    char buffer[1024];
    int status;

    /*从命令输出中读取每一行*/
    while(NULL!= fgets(buffer,1024,stream))
    {
      MY_LOG_INFO("read:%s",buffer);
    }
    status = pclose(stream);
    MY_LOG_INFO("process exited with status %d",status);

  }


##### 系统配置
 通过名称获取系统属性值：
 int __system_property_get(const char* name,char* value);

 通过名称获取系统属性：
 const prop_info* __system_property_find(const char* name);
