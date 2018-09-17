---
layout: post
title:  "常用设计模式的例子"
date:   2018-09-14 00:13:19 +0800
categories: jekyll update
tags: 设计模式
---
常用设计模式的例子
---

### 写在之前
 本片博客对几种常用的设计模式写了几个例子。但是设计模式重要的将该种思想应用于某种场景，而不应该生搬硬套。但是没有例子的思想是空泛的，这里只是想通过例子，来进一步加深对设计模式的理解。
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
```
 public interface Product{
   void function();
 }

 public class ProductFactory{
     private ProductFactory(){}
     private static ProductFactory instance;
     public static ProductFactory getInstance(){
       if(instance == null){
         instance = new ProductFactory();
       }
       return instance;
     }
     public Product createProdcut(Class<T> clazz){
        Product product = null;
        try{
          product = (Product)Class.forName(clazz.getName()).newInstance();
        }catch(Exception e){
           System.out.println("error");
        }
        return product;
     }
 }
```
### 三、抽象工厂模式

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

### 六、建造器模式

```
public class PlayerInfo{
  private int playerId;
  private String playerName;
  private int roleId;
  private String serverName;
  private String level;
  private String roleName;

  private PlayerInfo(Builder builder){
    playerId = builder.playerId;
    playerName= playerName;
    roleId = builder.roleId;
    serverName= serverName;
    level= level;
  }
  public static class Builder{
    private int playerId;
    private String playerName;
    private int roleId;
    private String serverName;
    private String level;
    private String roleName;     
    public Builder setPlayerId(int id){
       this.playerId = playerId;
       return this;
     }
    pulbic Builder setPlayerName(String name){
       this.playerName = playerName;
       return this;
    }
   pulbic Builder setRoleId(int roleId){
       this.roleId = roleId;
       return this;
    }  
    ...

    public PlayerInfo build(){
      return  new PlayInfo(this);
   }
  }
}
```

### 七、代理模式
```
public class Subject{
   void function();
}

public class RealSubject implements Subject{
  public void funtion(){
    System.out.println("RealSubject");
  }
}

public class Proxy implements Subject{
  private Subject real;
  public Subject setSubject(Subject subject){
    this.real = subject;
  }

  public void function(){
    this.beforeFunction();
    real.function();
    this.afterFunction();
  }

  public void beforeFunction(){
     System.out.println("before function");
  }

  public void afterFunction(){
    System.out.println("after function");
  }

  public void function(){
      //实体的方法
  }

}
```
