---
title: 使用正则处理字幕文件
date: {{ date }}
tags: 
categories:
comments: true
---


### 使用python中的re模块处理YouTube字幕文件

状况：18个字幕文件，编码格式Window-1254，命名规律，按数字排序

需求：将字幕文件中的演讲词提取出来，剔除无用的字符，并合并为一个文件

#### 文件命名格式
```
ls tmp/
total 84K
-rw-r--r-- 1 root root 3.1K Apr 22 14:50 02-Odyssey_Plans__The_Stages_of_Life.srt
-rw-r--r-- 1 root root 4.6K Apr 22 14:51 03-Odyssey_Plans__What_is_an_Odyssey_Plan_.srt
-rw-r--r-- 1 root root 2.5K Apr 22 14:51 04-Odyssey_Plans__What_does_an_Odyssey_Plan_Include_.srt
-rw-r--r-- 1 root root 2.9K Apr 22 14:52 05-Odyssey_Plans__Presentation_Format.srt
-rw-r--r-- 1 root root 1.5K Apr 22 14:52 06-Odyssey_Plans__5-Year_Timelines.srt
-rw-r--r-- 1 root root 1.2K Apr 22 14:53 07-Odyssey_Plans__6-Word_Title.srt
-rw-r--r-- 1 root root 2.0K Apr 22 14:53 08-Odyssey_Plans__Designing_3_Timelines.srt
-rw-r--r-- 1 root root 1.9K Apr 22 14:54 09-Odyssey_Plans__Building_your_10-year_timeline.srt
-rw-r--r-- 1 root root 1.3K Apr 22 14:54 10-Odyssey_Plans__Choosing_a_Symbol.srt
-rw-r--r-- 1 root root 3.2K Apr 22 14:54 11-Odyssey_Plans__Creating_a_Dashboard.srt
-rw-r--r-- 1 root root 1013 Apr 22 14:54 12-Odyssey_Plans__Identifying_Questions.srt
-rw-r--r-- 1 root root 1.6K Apr 22 14:55 13-Odyssey_Plans__Writing_a_Thank-You_note.srt
-rw-r--r-- 1 root root 2.8K Apr 22 14:55 14-Odyssey_Plans__How_to_'Prototype'_your_Odysseys.srt
-rw-r--r-- 1 root root 4.7K Apr 22 14:55 15-Odyssey_Plans__Prototype_Conversations_and_Experiences.srt
-rw-r--r-- 1 root root 3.7K Apr 22 14:55 16-Odyssey_Plans__How_often_to_design_an_Odyssey_plan.srt
-rw-r--r-- 1 root root 3.8K Apr 22 14:56 17-Odyssey_Plans__Insights_and_Takeaways.srt
-rw-r--r-- 1 root root 3.2K Apr 24 19:55 01-Odyssey_Plans__What_are_the_Odyssey_Years_.srt
-rw-r--r-- 1 root root 5.8K Apr 24 20:05 18-Odyssey_Plans__Applying_Designers'_Mindsets.srt
```
#### 文件内容格式
```
1
00:00:03,590 --> 00:00:04,710
My name is Bill Burnett.

2
00:00:04,710 --> 00:00:07,460
I'm one of the co-authors
of Designing Your Life,

3
00:00:07,460 --> 00:00:09,410
How to Live a
Well-Lived Joyful Life.

......
```
#### 思路分析
- 获取所有文件名存入列表  <--  方便循环进行文件操作
- 然后用正则匹配演讲词并打印出来  <-- 剔除无用字符
- 执行脚本并将输出重定向到一个新文件 <-- 合并为一个文件

#### 实现代码
```
# Script_name: merge_final.py

import re
import os

# 字幕文件所在目录
dir = "/root/tmp/"

def process(filename):
        '''对文件进行操作'''
        print(filename)
        
        with open(dir+filename, 'r', encode='utf-8') as f:       # 打开文件
                lines = f.readlines()       # 按行读取整个文件，存入列表
                
                for line in lines:
                        # 剔除无用字符
                        lrc = re.search(r'^\D.*[a-z.]', line) 
                        if lrc: 
                                print(lrc.group()) 
        
        print("--------------------")


def main():
        filelist = os.listdir(dir)      # 获取目录下所有的文件名称
        filelist.sort()                 # 按照数字排序
        
        for filename in filelist:
                process(filename)       # 将文件名传过去


if __name__ == "__main__":
        main()
```
#### 执行脚本
```
[root@dayday ~]# python merge_final.py > lecture.txt
```

#### 知识点整理
**1. 编码格式问题**

在读取中文的情况下，通常会遇到一些编码的问题，但是首先需要了解目前的编码方式是什么，然后再用decode或者encode去编码和解码，下面是使用chardet库来查看编码方式的。
```
import chardet

path = "E:/t.csv"
f = open(path,'rb')

data = f.read()
print(chardet.detect(data))
```
```
[root@dayday ~]# python test.py
{'encoding': 'GB2312', 'confidence': 0.99, 'language': 'Chinese'}
```
当时的字幕是从[downsub.com](http://downsub.com/)网站下载的，编码格式是Windows-1254格式，在Python进行文件操作时总是报错，偷懒在notepad++下手动更改编码格式为utf-8，当然也可以用Python脚本实现。

**2. 导入目录下所有文件的名称&排序问题**

这个就是知识没掌握的问题了，在此记录。os是个强大的模块！

**3. 正则语法**

好多语法格式都不熟练，一边调试一边查[runoob.com](runoob.com)，着实耗时间，需反复练习！

**4. re.match()返回值问题**

re.match()匹配成功返回re.match()对象，可用.group()方法提取字符串；匹配失败返回None。

文件时是按行读取的，因此循环对行进行正则匹配，返回的结果中穿插着None和re.match对象，而None调用.group()方法会报错，所以在这简单的用if过滤掉了None。

在敲这篇记录时突然想起，应该可以用Python的异常处理过滤掉报错，改天试试看

**5. 养成随手记录的习惯**

在整个解决过程中，查了很多资料，但没有随时记下来，解决完问题做记录的时候有些问题都忘记了！