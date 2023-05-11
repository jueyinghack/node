---
title: 多文件图片地址替换
tags: ["python","博客"]
categories: python
author: sec
date: 2023-05-4 19:04
---

> hexo加载网络地址图片失败，所以我将所有的csdn照片保存到了本地，然后将博客里面的地址全部替换，来解决这个问题

```python
import re
import os
import requests
path = "blog/" #文件夹目录
files = os.listdir(path) #得到文件夹下的所有文件名称
s = []
for file in files: #遍历文件夹
     if not os.path.isdir(file): #判断是否是文件夹，不是文件夹才打开
        f = open(path+"/"+file, encoding='utf-8') #打开文件
        lines = f.read()
        imgs = re.findall('\((https://img-blog.csdnimg.cn.+?)\)',lines)
        for img in imgs:
            lines = lines.replace(img, "img" + '/' +  re.findall('.+/(.*\.png)',img)[0])
        f1 = open(path + "/" + file, 'w', encoding='utf-8')  # 打开文件
        f1.write(lines)
```