---
layout:     post
title:      var let const
subtitle:   比较
date:       2018-06-07
author:     Shuxia
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS - swift -
---
# var let const
在JavaScript中有三种声明变量的方式：var、let、const。 var：声明全局变量，换句话理解就是，声明在for循环中的变量，跳出for循环同样可以使用。 [JavaScript] 纯文本查看 复制代码 ? 1 2 3 4 5 for(var i=0;i<=1000;i++){ var sum=0; sum+=i; } alert(sum); 声明在for循环内部的sum，跳出for循环一样可以使用，不会报错正常弹出结果 let：声明块级变量，即局部变量。 在上面的例子中，跳出for循环，再使用sum变量就会报错 注意：必须声明'use strict'后才能使用let声明变量否则浏览并不能显示结果 const：用于声明常量，也具有块级作用域 const PI=3.14;

