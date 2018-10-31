---
title: Jenkins打包
date: 2018-06-27 11:26:11
categories: 
    - Basic Skills
tags:
    - Continuous integration
---

### 1. 创建节点

#### 1.1 创建Mac Mimi节点

设置名称: mac-mimi

并发构建数: 2

远程工作目录: /Users/jenslave

标签: mac_mini

用法: 只允许运行绑定到这台机器的Job

启动方式: Launch agent agents via ssh

> 主机: 172. * . * . *

> Credentials: 添加登陆的用户名和密码

> Host Key Verification Strategy: Non Verifying Verification Strategy

可用性: 尽量保持代理在线

节点属性

> 环境变量 键：PATH 值：$PATH:/usr/local/bin


### 2. 创建任务


General

> 限制项目的运行节点
>> 标签表达式: mac_mimi

源代管理

>Git 
>> Repositories 

>>> Repository URL: GitLab的路径

>>> Credentials: GitLab的账号名称和密码

>>> Branches to build:

>>>>Branch Specifier: */Master

构建
> 执行Shell
>> 命令: ./android_build.sh test

构建后操作: 需要安装HockeyApp插件

> Upload to HockeyApp 

>>>Applications

>>>> API Token：******

>>>> Upload Method

>>>>> Upload Version

>>>>> App ID: *****

>>>> App File : android/app/build/outputs/apk/TestEnv/debug/debug.apk

>>>> Release Notes 
 
>>>>> Load Release Notes from File

>>>>>> File Name: android/ChangeLog

>>>>> Interpret Release Notes as Markdown

>>>> Allow Downloads