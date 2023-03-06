---
description: 参考文章：https://opensource.actionsky.com/20210716-mysql/
---

# 关于Mysql的临时表

在部分sql执行时会产生临时表的使用，可以分为两类：显性使用，隐性使用。

隐性使用主要出现在慢sql中。

使用`explain`可以查看是否会产生临时表：

> 如果 explain 的 Extra 列包含 Using temporary ，那么说明会使用临时空间，
>
> 如果包含 Using filesort ，那么说明会使用文件排序(临时文件)

临时表一般所需的空间较小，会优先存放于内存中，若超过一定的大小，则会转换为磁盘临时表或者临时文件

#### 临时表的空间释放

只有在mysql重启后才会释放这部分磁盘空间。
