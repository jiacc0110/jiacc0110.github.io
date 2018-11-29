
---
layout: post
title:  "Android下的MVVM框架学习"
date:   2018-11-29 19:30:00 +0800
categories: jekyll update
tags: java注解基础
---
###元注解
注解目前非常的流行，很多主流框架都支持注解，而且自己编写代码的时候也会尽量的去用注解，一时方便，而是代码更加简洁。
目前主流框架都会使用注解的形式，例如EventBus，在早期时候版本，都都是使用反射作为回调的，

1. @Target 修饰范围：
   CONSTRUCTOR/FIELD/LOCAL_VARIABLE/METHOD/PACKAGE/PARAMETER/TYPE

2. @Retention 保留时长（注解的生命周期）
   SOURCE/CLASS/RUNTIME
   RUNTIME属性的注解，可以被反射调用。

3. @Documented
   Documented用于描述其它类型的annotation应该被作为被标注的程序成员的公共API，因此可以被例如javadoc此类的工具文档化。Documented是一个标记注解，没有成员

4. @Inherited
@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。

###自定义注解：

 public @interface 注解名{定义体}

注解参数的可支持数据类型：

　　　　1.所有基本数据类型（int,float,boolean,byte,double,char,long,short)
　　　　2.String类型
　　　　3.Class类型
　　　　4.enum类型
　　　　5.Annotation类型
　　　　6.以上所有类型的数组

1.成员只能public 和default
2.基本类型、String、Enum、Class、annotation、数组、String
3.如果只有一个成员参数，设为value,后面加小括号。


@Target(ElementType.FILED)
@Rentention(RetentionPolicy.RUNTIME)
@Documented
public @interface FruitName{
  String value() default "";
}


@Target(ElenmentType.FILED)
@Rentention(RetentionPolicy.RUNTIME)
@Documented
public @interface FruitColr{
 public enum Color{BLUE,RED,GREEN}
 Color fruitColor() default Color.GREEN;
}


public class Apple{
 @FruitName("Apple")
 private String appleName;

 @FruitColor(fruitColor)
 private String appleColor;

 public void setAppleColor(String appleColor){
   this.appleColor = appleColor;
 }

 public void setAppleName(String appleName){
   this.appleName = appleName;
 }

 public String getAppleName(){
   return appleName;
 }

 public String getAppleColor(String appleColor){
   return appleColor;
 }

 public void displayName(){
   System.out.println("水果的名字是：苹果");
 }

}
