---
title: React Native 开发环境安装
date: 2018-06-21 10:20:52
categories: 
    - React Native
tags:
    - React Native
---
### 1. 安装Brew

由于我以前安装过brew了，所以只需要更新到最新到版本

     brew update
     brew -v
     brew doctor    // 确保brew是安全可靠的

<!--more-->
### 2. 安装node.js

确保有权限安装

    sudo mkdir /usr/local/include
    sudo chown -R $(whoami) $(brew --prefix)/*
    
安装node.js

    brew install node
    brew link node
    
### 3. 安装React Native

安装yarn

    npm install -g yarn react-native-cli

安装Watchman

      brew install watchman

创建项目

    react-native init AwesomeProject
    cd AwesomeProject
    react-native run-ios
如何运行不成功，就用xcode打开项目查看具体的错误

### 3. 用编译器打开项目
#### 3.1 用xcode打开IOS项目

打开项目后需要设置code sigin `AwesomeProject` 和`AwesomeProjectTests` 工程都要进行设置

#### 3.2 用Android Studio打开Android项目

Android需要设置sdk路径

    vi ~/.bash_profile
    export ANDROID_HOME=/Users/phepe/Library/Android/sdk
    source ~/.bash_profile
    
然后插上手机 运行

     react-native run-android
          

