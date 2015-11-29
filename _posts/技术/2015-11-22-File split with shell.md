---
layout: post
title:  用shell切割文件的方法
category: 技术
tags: shell Linux
keywords: shell,linux
description: 
---
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;有这样的场景，我们有个比较大的文件要处理，想要把它切成若干份，每份N行，以便并行处理。linux下面通过shell脚本可以很轻松的实现文件的切割，下面笔者就介绍两种，使用的场景还是有些区别的。

### sed命令实现

sed是linux下的流编辑器，sed是stream editor的缩写。废话少说，直接上干货。

+	首先来看看sed的相关用法

```
sed -n '5p' datafile   #只打印第五行
sed -n '1,200p' datafile  #只查看文件的第1行到第200行
sed '1,200d' datafile  #删除第1行到第200行

```

+	用sed实现文件切割

假设你的文件有60万行，需要切割为10个文件，可以通过如下方式来实现切割：

```
sed -n '1,60000p' sg_data.txt  > sg_data_1.txt
sed -n '60001,120000p' sg_data.txt  > sg_data_2.txt
sed -n '120001,180000p' sg_data.txt > sg_data_3.txt
sed -n '180001,240000p' sg_data.txt > sg_data_4.txt
sed -n '240001,300000p' sg_data.txt > sg_data_5.txt
sed -n '300001,360000p' sg_data.txt > sg_data_6.txt
sed -n '360001,420000p' sg_data.txt > sg_data_7.txt
sed -n '420001,480000p' sg_data.txt > sg_data_8.txt
sed -n '480001,540000p' sg_data.txt > sg_data_9.txt
sed -n '540001,600000p' sg_data.txt > sg_data_10.txt

```

可以看到这种方式其实是比较繁琐的，不太适合我们使用shell想简化操作的目的。但是其实sed的这种方式也有自己的使用场景，比如你就想取文件的第1行到第1000行进行采样分析，那么就可以通过以下命令轻松实现：

```
sed -n '1,1000p' datafile

```

### split命令实现

相对于sed命令较为繁琐的实现，linux给我们提供了更为直接的、用于切割文件的命令，十分的方便。还是上面的例子，例如要把一个55万条记录的文件切割成6个文件，每个文件10万。那么通过split命令可以这样实现：

```
split -l 100000 datafile

```

很简单吧，这个命令会在当前目录下生成xaa，xab，xac，xad，xae，xaf 六个文件。
wc -l 可以看到六个文件行数如下：

```
>> wc -l xa*
100000 xaa
100000 xab
100000 xac
100000 xad
100000 xae
50000 xaf
550000 总计
```

###总结

&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;其实linux系统有很多shell命令都可以轻松的实现我们的目标，所以在遇到一些需要处理的问题的时候，先试着这样问自己：“是不是有这样的一个命令能够很好的实现我的目标？不用再重复造轮子了。”这大概也是初级工程师和高级工程师的区别吧~






