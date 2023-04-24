---
title: sql-count
tags: sql
categories: sql
date: 2023-4-24
author: sec
---

# Mysql

## Mysql卸载

1. 此电脑 - 服务 - 关闭Mysql服务

2. 在环境变量中将Mysql环境变量删除

3. 软件所在位置

4. 数据所在位置

5. 注册表

   ```
   HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\Mysql服务
   HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Mysql服务 目录删除
   HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\Mysql服务
   HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Mysql服务 目录删除
   HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\Mysql服务
   HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Mysql服务 目录删除
   ```

   

## Mysql4大版本

1. Mysql Community Server 社区版
2. Mysql Enterprise Edition 企业版本
3. MYSQL Cluster 集群版
4. MYSQL Cluster CGE 高级集群版



## 安装

官网：https://www.mysql.com/

`推荐下载msi文件，不推荐下载压缩包`

用安装包直接下一步下一步

配置Path环境变量->将bin目录放到Path环境变量中