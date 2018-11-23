---
layout: post
title:  "gradle自定义插件"
date:   2018-09-17 21:28:00 +0800
categories: jekyll update
tags: gradle
---
### Gradle 自定义插件

在Android中，build.gradle文件中引用
```
apply plugin: 'com.android.application'
```
然后就可以使用插件中的功能了。

我们也可以自己定义插件。

本文主要为自定义Gradle插件的过程。
####1.工程的搭建
此处使用的是AndroidStudio搭建groovy工程。其实如果对编程有做够的经验应该可以理解到构建与IDE是没有关系的，为了方便使用，我们此处使用AndroidStudio搭建工程。

新建一个Android工城，修改工程结构，最终结构目录为：

[工程根目录]
     |--[插件工程名]
            |--libs
            |--src
                |--main
                    |--Groovy
                         |--[包名].sourceFile.groovy
                    |--resources
                         |--META-INF.gradle-plugins
                              |--xxxx.properties
     |--build.gradle
     |--settings.gradle

其中,resources文件夹下的目录为固定目录结构，xxxx.properties文件为插件的配置文件，配置插件的入口。这里要注意的是xxxx命名，就是apply plugin：'xxxx'在引用时的名字
xxxx.properties文件
```
implementation-class=[包名].[入口类的类名]
```
####2.groovy代码
根据上面的入口[包名].[入口类的类名]，创建groovy源文件，此入口类继承Plugin类。例：
```
class MyPlugin extends implements Plugin<Project>{
    @override
    void apply(Project project){

      println 'this is my first plugin'
      project.tasks.create('myTask',MyTask)
    }  
}
```

####3.上传插件

上传插件就是在这个Module的build.gradle中写Task了，看下代码,我就上传到工程目录的平级目录中的repo目录下了
```
apply plugin: 'maven'
group = 'com.jiacc.test'
version = '1.0.0'

uploadArchives {
    repositories {
        mavenDeployer {
            //本地的Maven地址设置为D:/repos
            repository(url: uri('../repo'))
        }
    }
}

```
执行gradle uploadArchives 命令，就可以上传到repo仓库，若要上传到远程仓库，
则将uri配置到远程maven仓库地址。
####4.使用插件

首先在要使用这个插件的app目录下的build.gradle中添加下面代码:

1.apply plugin的名字就是上面我们在resources中定义的properties文件的文件名;
2.maven仓库就是我们上面的仓库地址;
3.就是添加依赖，其中com.juexingzhe.mobile就是group,juexingzhe就是上面插件工程的工程名，1.0.0就是版本号了，group和version都是在上面的gradle中定义了，module没定义就默认使用工程名了。
。
```
apply plugin: 'com.jiacc.test'

buildscript {
    repositories {
        maven {
            url uri('E:/code/repo')
        }
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.3'
        classpath 'com.jiacc.test:MyPlugin:1.0.0'
    }
}
```
