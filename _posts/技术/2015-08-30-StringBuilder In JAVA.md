---
layout: post
title: 那些年在JAVA开发中使用StringBuilder踩过的坑
category: 技术
tags: JAVA
keywords: 
description: 
---
众所周知，在JAVA开发中如果涉及到字符串的拼接处理等就经常会使用到StringBuilder类，这个类的基本使用场景就是当我们需要拼接大量的字符串，它会比使用“+”暴力拼接要高效。StringBuilder的底层实现是通过数组来缓存各字符串的内容，当调用toString()方法时就创建并返回一个字符串对象。跟StringBuilder具有类似功能的类还有一个叫StringBuffer，这两个类的功能基本相同，唯一的区别是StringBuilder是非线程安全的，而StringBuffer是线程安全的。

###StringBuilder的基本用法

```
public class TestStringBuilder {

	public static void main(String[] args) {
		StringBuilder sb = new StringBuilder(100);//可以初始化容量
		sb.append("abc");
		sb.append("defg");
		System.out.println(sb.toString());//打印出来结果为：abcdefg
	}

}

```

###说说笔者遇到的坑
开始时笔者写了如下方法，方法的功能是对传进来的字符串数组进行拼接并用md5加密并返回加密后的字符串。

```
public static void process(String[] args) {
		StringBuilder sb = new StringBuilder();
		sb.append("{");
		for(String str : args){
			sb.append(str);
		}
		sb.append("}").toString();
		return utils.md5(sb.toString());
	}

```

后来为了调试方便，笔者在return之前打印了一行logger，代码如下：

```
public static void process(String[] args) {
		StringBuilder sb = new StringBuilder();
		sb.append("{");
		for(String str : args){
			sb.append(str);
		}
		//想看看加密前方法的参数是否正确
		logger.info("string={}", sb.append("}").toString());
		sb.append("}").toString();
		return utils.md5(sb.toString());
	}
```
就是因为加上了这么一行日志，结果程序死活调不通了，当时算出的加密串和网上在线生成md5加密串的结果完全不一样，笔者还以为发现了md5算法的一个大BUG，结果debug一下程序发现是因为logger.info("string={}", sb.append("}").toString())这行代码。相当于执行了两遍sb.append("}")，而我一直以为utils.md5(sb.toString())传进去的参数是对的，因为前一行logger打印出来的结果确实是对的。

###总结
作为开发来说还是要严谨细心，像这种BUG一旦在大的系统中埋下，其实很难找到。






