layout:     post
title:      uicollectionView
subtitle:   比较
date:       2018-01-30
author:     Shuxia
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS -

# uicollectionView 刷新不出数据的问题记录:
当 reloadsections 与 reloadData 共存的时候 先 relaoddata  否则某些 cell 里面的数据 有可能刷新不出来