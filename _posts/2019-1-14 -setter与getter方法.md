layout:     post
title:      setter与getter方法
subtitle:   比较
date:       2019-06-12
author:     Shuxia
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS -
#同时书写 setter与getter方法
在@implementation 实现中添加一行代码就OK了

@synthesize wtName = _wtName;

详解:
oc最初的设定 @property 和@synthesize的作用:
@property的作用是定义属性,声明getter,setter方法,注意属性不是变量
@synthesize 的作用是实现属性的getter,setter方法
在声明属性的情况下,如果重写setter,getter方法.就需要把未识别的变量在@synthesize里面定义,把属性的存取方法作用变量.
@property 独揽三个功能 
1.生成了带下划线的私有成员变量,此之类不可以直接访问.但是可以通过get/set方法访问 那么想让定义的成员变量直接访问,那么只能写在.h文件里面
2.生成了get/set方法
3.用property声明的成员属性,相当于自动生成了set/get方法,如果重写了set/get 那么p与roperty声明的就不是一个成员变量了.是另一实例变量,而这个实例变量需要手动声明.所以会报错.

 总结:一定要分清属性和变量,不能混淆,@synthesize的意思是将set/get方法作用于这个变量.