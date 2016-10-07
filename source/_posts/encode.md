---
title: 计算机编码的来龙去脉
date: 2016-10-07 13:27:31
tags:
---


## 计算机编码的基本原理

## 计算机编码的发展史

ASCII -> GB2312 -> GBK -> GB18030    中文

ASCII -> unicode -> utf(8,16) 世界


### ASCII
### GB2312
### GBK
### GBK18030

GB18030是取代GBK1.0的正式国家标准。该标准收录了27484个汉字，同时还收录了藏文、蒙文、维吾尔文等主要的少数民族文字。现在的PC平台必须支持GB18030，对嵌入式产品暂不作要求。所以手机、MP3一般只支持GB2312。

> 总结

从ASCII、GB2312、GBK到GB18030，这些编码方法是向下兼容的，即同一个字符在这些方案中总是有相同的编码，后面的标准支持更多的字符。在这些编码中，英文和中文可以统一地处理。区分中文编码的方法是高字节的最高位不为0。按照程序员的称呼，GB2312、GBK到GB18030都属于双字节字符集 (DBCS)。 不过GB18030相对GBK增加的字符，普通人是很难用到的，通常我们还是用GBK指代中文Windows内码。

### unicode

"Universal Multiple-Octet Coded Character Set",简称为UCS。UCS可以看作是”Unicode Character Set”的缩写

### utf8/utf16

UCS规定了怎么用多个字节表示各种文字。怎样传输这些编码，是由UTF(UCS Transformation Format)规范规定的，常见的UTF规范包括UTF-8、UTF-7、UTF-16。
