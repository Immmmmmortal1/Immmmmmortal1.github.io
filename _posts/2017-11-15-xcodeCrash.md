---
layout:     post
title:      图像化分析程序崩溃日志 
subtitle:   获取崩溃日志
date:       2019-11-13
author:     Shuxia
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS
---
# 图像化分析程序崩溃日志
崩溃的日志的获取 是调试程序过程中一个重要的环节.每个程序都会有bug,但是你多久能拿到崩溃日志,多久能够通过这些日志分析出问题所在 ,并解决,怎么样规避一些比较低级的bug.这是我们经常碰到的问题.今天就来分享一下自己在调试过程的崩溃信息的追踪方式.
今天主要有一下几点:
1.crash 文件的采集的具体步骤.
2.crash文件中的内容的含义.

4.dysm 文件来源和作用
5.crash文件中崩溃的地址的手动解析的方法

_**什么是 dSYM 文件**_
Xcode编译项目后，我们会看到一个同名的 dSYM 文件，dSYM 是保存 16 进制函数地址映射信息的中转文件，我们调试的 symbols 都会包含在这个文件中，并且每次编译项目的时候都会生成一个新的 dSYM 文件，位于 /Users/<用户名>/Library/Developer/Xcode/Archives 目录下，对于每一个发布版本我们都很有必要保存对应的 Archives 文件 ( AUTOMATICALLY SAVE THE DSYM FILES 这篇文章介绍了通过脚本每次编译后都自动保存 dSYM 文件)。

**_dSYM 文件有什么作用_**
当我们软件 release 模式打包或上线后，不会像我们在 Xcode 中那样直观的看到用崩溃的错误，这个时候我们就需要分析 crash report 文件了，iOS 设备中会有日志文件保存我们每个应用出错的函数内存地址，通过 Xcode 的 Organizer 可以将 iOS 设备中的 DeviceLog 导出成 crash 文件，这个时候我们就可以通过出错的函数地址去查询 dSYM 文件中程序对应的函数名和文件名。大前提是我们需要有软件版本对应的 dSYM 文件，这也是为什么我们很有必要保存每个发布版本的 Archives 文件了。



***一.xcode 还原法***
每次打包app的时候 我们都会勾上两个选项.其中第二个选项就是我们上传的日志文件.
![勾选处](blogImage/15446634260311/%E5%8B%BE%E9%80%89%E5%A4%84.png)

然后 我们就可以每周 来看一下 崩溃日志.xcode已经直接图形化崩溃日志并且有完整的调用栈. 这个 可能需要 将代码会退到和上传版本一样 的地方 ,否则会出现定位错误.
![选中你要查看的app和版本](blogImage/15446634260311/%E9%80%89%E4%B8%AD%E4%BD%A0%E8%A6%81%E6%9F%A5%E7%9C%8B%E7%9A%84app%E5%92%8C%E7%89%88%E6%9C%AC.png)


***二.友盟还原法.***
这种方法适用于线上App的错误分析.
这个是我们现在 正在使用的一种方法,就不细讲啦.
就是 打包之后获取当前 ipa包的dysm文件,上传到友盟后台.

***三.[dsymTools](https://github.com/answer-huang/dSYMTools)的工具还原法.手动解析方式.***
比如:我们遇到一个 -[__NSArrayM objectAtIndex:]: index 50 beyond ,或则是字典报错,有时候能够定位到这个错误,但是有时候定位不到,只是给出了 内存地址.如果在项目中查找 那么多的数组,显然不是很现实.类似的定位不到位置的很多.那么我们这时就可以使用手动解析的方法.通过第三方工具直接定位到我们工程中代码的位置.
还有一种就是:测试的设备上的崩溃比较难以复现的bug  就直接连接设备 获取崩溃日志 然后进行手动解析!
**1.获取设备的crash 文件**
连接崩溃设备 获取crash 文件
![连接设备](blogImage/15446634260311/%E8%BF%9E%E6%8E%A5%E8%AE%BE%E5%A4%87.png)
**2.选中"view device Logs "**
![手机上应用的crash日志](blogImage/15446634260311/%E6%89%8B%E6%9C%BA%E4%B8%8A%E5%BA%94%E7%94%A8%E7%9A%84crash%E6%97%A5%E5%BF%97.png)
**2.选中自己的app**
![YSPProject](blogImage/15446634260311/YSPProject.png)

**3.找到崩溃信息以工程名子开头的地方.后面的地址就是崩溃信息的内存地址**
![YSPProject](blogImage/15446634260311/YSPProject.png)
这里会有 两个地址
1>.sliderAddress
**0x104450000**
2>.错误信息内存地址
*0x104750c2c*
比如圈中的第一行地址:
4   YSPProject              *0x104750c2c* **0x104450000** + 3148844
前面的是内存地址  后面的是sliderAddress
**4.获取对应版本的dym 文件.可以直接将打包出来的xcodeArchive文件拖入到dsymTools窗口里面,可以同是拖多个但是 不能重名.(目前 会自动检索你的本地xcodeArchive文件):如下**
![工具界面](blogImage/15446634260311/%E5%B7%A5%E5%85%B7%E7%95%8C%E9%9D%A2.png)
**3.然后选择你的cpu类型**
![选中之后的界面](blogImage/15446634260311/%E9%80%89%E4%B8%AD%E4%B9%8B%E5%90%8E%E7%9A%84%E7%95%8C%E9%9D%A2.png)
**4.然后把 赋值 的地址依次填入到相应的位置中.**
![结果](blogImage/15446634260311/%E7%BB%93%E6%9E%9C.png)
然后就可以看到代码中错误的地方.
