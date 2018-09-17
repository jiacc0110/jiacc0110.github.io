---
layout: post
title:  "Window和WindowManager"
date:   2018-09-17 23:31:36 +0800
categories: jekyll update
tags: 读书笔记
---
### Window和WindowManager

本文为超人气《Android开发艺术探索》读书笔记。

---
Window是一个抽象类，它的具体实现是PhoneWindow。WindowManager是外界访问Window的入口。

WindowManager提供常用的方法：添加View、更新View、删除View。
 ```
 public interface ViewManager{
    public void addView(View view,ViewGrop.LayoutParams params);
    public void updateViewLayout(View view,ViewGrop.LayoutParams params);
    public void removeView(View view);
 }
 ```
WindowManager接触了ViewManager。
```
public boolean onTouch(View view,MotionEvent event){
  int rawX = (int) event.getRawX();
  int rawY = (int) event.getRawY();
  switch(event.getAction()){
    case MotionEvent.ACTION_MOVE:
    mLayoutParams.x = rawX;
    mLayoutParams.y = rawY;
    mwindwonManager.updateViewLayout(mFloatingButton,mLayoutParams);
    break;

    default:
    break;
  }
  return false;
}
```

Window有三种类型：应用Window、子Windwo、系统Window。
