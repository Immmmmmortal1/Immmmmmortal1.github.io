layout:     post
title:      layoutSubviews,layoutIfNeed,setNeedsLayout
subtitle:   比较
date:       2018-06-01
author:     Shuxia
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS -
# layoutSubviews,layoutIfNeed,setNeedsLayout  的使用

一,官方解释,原理说明.不要直接调用layoutSubviews方法,
二,想要强制重新布局子视图,应该调用setNeedsLayout方法,这样设置之后再次进入下个渲染周期时,就相当于给这个view进行了一个标记,需要重新布局.
三,当你想要马上重新布局的时候,应该直接调用layoutIfNeed

