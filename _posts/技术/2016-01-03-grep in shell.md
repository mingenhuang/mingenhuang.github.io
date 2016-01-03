---
layout: post
title:  再来说说linux下的grep命令
category: 技术
tags: shell Linux
keywords: shell,linux
description: 
---
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;grep命令大家应该都比较熟悉，特别是平时需要做一些运维相关工作的同学。grep可以进行文本搜索，可以匹配正则表达式进行搜索。笔者此处主要想讲讲grep的-A-B-选项，grep能找出带有关键字的行，但是工作中有时需要找出该行前后的行。那么这样的场景就需要-A-B-选项来实现。

### grep的-A-B-选项用法

+ 找出filename中带有keyword的行，输出中除显示该行外，还显示之后的一行(After 1)。

```
grep -A1 keyword filename

```

+ 找出filename中带有keyword的行，输出中除显示该行外，还显示之前的一行(Before 1)

```
grep -B1 keyword filename

```

+ 找出filename中带有keyword的行，输出中除显示该行外，还显示之前的一行(Before 1)和显示之后的一行(After 1）

```
grep -1 keyword filename

```

### 总结

&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;grep功能还是比较单一的，只能进行一些简单的文本搜索，想要了解更多可以在终端下使用man grep命令来查看~






