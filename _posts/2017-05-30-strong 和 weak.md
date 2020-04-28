---
layout:     post
title:      strong 和 weak
subtitle:   iOS
date:       2017-05-30
author:     Shuxia
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS -
---
# strong 和 weak
**self = 控件;**
_strong_
strong 就是强引用,会跟随父控件 的销毁而销毁.
1.在强引用的控件,使用 [self removeFromSuperView];只是把该视图移除了,但是该块内存还是存在的,就是说该对象并没有销毁.
2.在强引用的控件,使用了[super addSubView:self]之后,使用 self = nil; 并没有让该控件原来的内存释放,,你只是重新创建了一个新的对象self1, 但是 self 还是在视图上.

_weak_

使用 weak 弱引用,就是他的引用计数不会加一,
使用 weak 修饰的控件,使用 [self removeFromSuperView]; 这时 self 就自动被置为 nil;