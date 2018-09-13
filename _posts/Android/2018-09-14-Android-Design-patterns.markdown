---
layout: post
title:  "常用设计模式的例子"
date:   2018-09-14 00:13:19 +0800
categories: jekyll update
---
常用设计模式的例子
---
### 一、单例模式
单例模式有多种实现，这里使用的是双校验同步锁的方式。这种方式是为了线程安全，为了避免两个线程同时完成空判断而没有实例化，将判断和实例化加上同步锁作为一个原子操作。第一个空判断
是为了防止每次都进入同步块，可大大提升效率。
```
public class Emperor{
   private static Emperor instance;
   private Emperor();
   public static Emperor getInstance(){
       if(instance == null){
            Synchronized(Emperor.class){
                if(instance == null){
                     instance = new Emperor();
                }
            }
        }    
      return instance;
  }   
}
```

### 二、工厂方法模式

### 三、工厂模式

### 四、观察者模式
```
public class Observable{
  private List<Observer> observes;
  private static Observable instance;
  private Observable(){
     observes = new ArrayList<Observer>();
  }
  public static  Observable getInstance(){
      if(instance == null){
         instance = new Observable();
        }
    return instance；
  }
 public void update(Object obj){
     for(Observer observers: o){
          o.onUpdate(obj);
      }
  }
 public void registerObserver(Observer observer){
     observer.add(observer);
 }
 public void unRegisterObserver(Observer observer){
     observer.remove(observer);
 }
}

public interface Observer{
  void onUpdate(Object o);
}
```
### 五、命令模式
```
//定义命令接口
public interface Command{
   void execute();
}

public class AddCommand{
   Receiver receiver;
   public AddCommand(Receiver receiver){
      this.receiver = receiver;
   }
   public void execute(){
      receiver.doSomething2();
   }
}

public class DeleteCommand{
  Receiver recerver;
  public DeleteCommand(Recever receiver){
      this.receiver = receiver;
 }
  public void execute(){
     receiver.doSomething();
  }
}
//定义接收者
public interface Receiver{
   public void doSomething();
}

public class Receiver1 implements Receiver{
  public void doSomething(){
    System.out.println("Receiver1 dosomething");
 }
}
public class Receiver2 implements Receiver{
  pubilc void doSomething(){
    System.out.println("Receiver2 dosomething");
 }
}
//定义调用者
public class Involker{
  private Command commoand;
   public setCommand(Command command){
     this. command = command;
  }
  public void action(){
      command.execute();
  }
}

//用户 
public class Client{
 public static void main(String[] args){
    Invoker invoker = new Inverker();
    Receiver receiver = new Receiver1();
    Command command = new AddCommand(receiver);

   invoker.setCommand(receiver);
  invoker.action();
}
```
