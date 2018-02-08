---
layout: post
title: "Spring-Data-Redis存储键值出现乱码"
---  

   最近在利用Spring Data操作redis出现了乱码，在key前面出现了\xAC\xED\x00\x05t\x00，value也是如此，只是内容不同，如下图所示：
    ![乱码](/img/post/20180208.png)