---
title: ceph梳理
tags:
  - 技术
comments: true
author: cherrybaby
abbrlink: c7ad3fcb
date: 2018-07-28 12:04:37
---

## 一、数据寻址过程 ##
### 1.file ->object ###
将大小不统一的file 切成均匀的小块并给每个小块加标记，方便后续的处理
ino(inode number的缩写，意思是file的id) + non(object number的缩写，是这个file切分产生的序列号) = oid（object id）
举例：
ino = filename 这个file被切成了3个object，object的序号依次为0，1,2
最终oid的序号： filename0  filename1 filename2
期间要保证ino的唯一性，否则重复了，后边的映射就无法进行了。

### 2. Object -> PG ###
假设pg的总数为m个（m应该是2的整数幂），算法最终的目的是将多个object近似均匀的映射到pg中；
object映射到哪个pg？计算公式如下：
hash(oid) & mask ->pgid
算法字面解释：
ceph指定一个静态哈希函数，计算oid的哈希值，将oid映射成为一个近似均匀分布的伪随机值，将这个伪随机值与mask值按位相与，最终得到pg的序号（pgid）
什么这么算：
根据RADOS的设计，给定PG的总数为m（m应该为2的整数幂），则mask的值为m-1。因此，哈希值计算和按位与操作的整体结果事实上是从所有m个PG中近似均匀地随机选择一个。基于这一机制，当有大量object和大量PG时，RADOS能够保证object和PG之间的近似均匀映射。又因为object是由file切分而来，大部分object的size相同，因而，这一映射最终保证了，各个PG中存储的object的总数据量近似均匀。