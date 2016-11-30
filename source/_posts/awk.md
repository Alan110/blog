---
title: awk 简单入门
date: 2016-11-30 16:01:21
tags:
---

awk是一个强大的文本处理工具，在处理简单的文本数据时非常有用，甚至在写mapreduce的时候也能派上用场。它是按行处理文本的, 可以与sed，grep结合

## 指定分隔符

默认是按\t制表符分割

```shell

awk -F, '{print $1,$2}'   log.txt

# 或者使用内建变量
awk 'BEGIN{FS=","} {print $1,$2}'     log.txt

# 使用多个分隔符.先使用空格分割，然后对分割结果再使用","分割
awk -F '[ ,]'  '{print $1,$2,$5}'   log.txt
```


## 按行过滤

```awk

awk '{[pattern] action}' {filenames}   # 行匹配语句 awk '' 只能用单引号

awk '$1==2 {print $1,$3}' log.txt    #命令
```

## 整体结构

* BEGIN{ 这里面放的是执行前的语句 }
* END {这里面放的是处理完所有的行后要执行的语句 }
* {这里面放的是处理每一行时要执行的语句}

```awk
awk '
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0
 
    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}' test.txt
```

## 内建变量

| name            | describe                                          |
| : ------------- | :-------------                                    |
| $n              | 当前记录的第n个字段，字段间由FS分隔               |
| $0              | 完整的输入记录                                    |
| ARGC            | 命令行参数的数目                                  |
| ARGIND          | 命令行中当前文件的位置(从0开始算)                 |
| ARGV            | 包含命令行参数的数组                              |
| CONVFMT         | 数字转换格式(默认值为%.6g)ENVIRON环境变量关联数组 |
| ERRNO           | 最后一个系统错误的描述                            |
| FIELDWIDTHS     | 字段宽度列表(用空格键分隔)                        |
| FILENAME        | 当前文件名                                        |
| FNR             | 同NR，但相对于当前文件                            |
| FS              | 字段分隔符(默认是任何空格)                        |
| IGNORECASE      | 如果为真，则进行忽略大小写的匹配                  |
| NF              | 当前记录中的字段数                                |
| NR              | 当前记录数                                        |
| OFMT            | 数字的输出格式(默认值是%.6g)                      |
| OFS             | 输出字段分隔符(默认值是一个空格)                  |
| ORS             | 输出记录分隔符(默认值是一个换行符)                |
| RLENGTH         | 由match函数所匹配的字符串的长度                   |
| RS              | 记录分隔符(默认是一个换行符)                      |
| RSTART          | 由match函数所匹配的字符串的第一个位置             |
| SUBSEP          | 数组下标分隔符(默认值是/034)                      |

## 语法

* 基本上如同c语言的语法
* 变量不需要声明
* 全部是全局变量
* 有关联数组, for in 遍历

## 完整例子

```sh
cat  ./${date} | awk '
{
    if($2 > 4 && $3 > 1000){
        pv[$2]+=$3;
        time[$2]+=$4

        pvall += $3;
        timeall += $4;

        if($2 < 8){
            pv8 += $3;
            time8 += $4;
        }
    }
}END{
    print "{";
    printf ( "\"%s\":\"%s\",\n", "all",(timeall/pvall) );
    printf ( "\"%s\":\"%s\",\n", "<8.0",(time8/pv8) );
    for(i in pv){
        if( time[i] > 0 && pv[i] > 0) {
            printf ( "\"%s\":\"%s\",\n", i,(time[i]/pv[i]) );
        }
    } ;
    print "}";
}'  | tac | sed '2s/\,//' | tac > $datapath/$date
```


## 引用

[awk教程](http://www.runoob.com/linux/linux-comm-awk.html)
