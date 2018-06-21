---
title: GitHub搭建博客教程
date: 2018-06-20 18:25:54
categories: 
    - Git
tags:
    - GitHub
---

### 1.安装hexo
#### 1.1 安装node.js
  在node.js官网下载对应系统的安装包，按照提示进行安装，前提是需要安装好npm，然后通过npm安装各种需要的工具，安装成功后输入以下命令检测

    npm -v
    node -v
#### 1.2 安装hexo

    npm install hexo-cli -g
    hexo -v

### 2. 创建博客项目
#### 2.1 在需要存放博客项目的目录下创建博客
    hexo init Blog
    cd Blog
    npm install

#### 2.2 生成静态页面
    hexo clean
    hexo g      //  g即generate
    
#### 2.3 运行
    hexo s      // s即server
访问 localhost:4000 查看效果

### 3.配置
网站的设置大部分都在_config.yml文件中

* title：网站标题
* subtitle：网站副标题
* description：网站描述
* author：你的名字
* language：网站使用的语言

language: default #默认是英文

    # language: zh-CN
    # language: fr-FR
    # language: zh-HK
    # language: zh-TW
    # language: en
    # language: ru
    # language: de

**注意：**进行配置时，需要在冒号:后加一个英文空格

在GitHub上`Create a new repository`, `Repository name`必须以`.github.io`

修改最后一行的配置

    deploy:
          type: git
          repository: https://github.com/phepe/phepe.github.io
          branch: master
修改URL 设定为 git pages 地址

    url: https://phepe.github.io/

完成配置后安装 hexo-deployer-git

    npm install hexo-deployer-git --save

部署到GitHub上

    hexo g        // 重新生成博客
    hexo s        // 本地预览博客
    hexo d        // d即deploy 运行的过程中输入对应的github的账号密码
    
访问 https://phepe.github.io/ 查看效果

### 4. 新增博客
#### 4.1 命令方式
在博客目录下输入

    hexo new "Tutorail"
此时会在source/_posts目录下生成Tutorail文件，写入一些内容再保存,然后执行

    hexo clean
    hexo g
    hexo s     // -p 9090  设定端口为 9090
然后生成静态页面，访问 localhost:4000 查看效果

#### 4.2 直接方式
在 source/_posts/下新建.md文件，然后写入内容，然后使用命令生成静态页面

### 5. 设置博客主题
#### 5.1 下载主题
将网站找到的主题下载到项目的themes目录下，使用next主题

    git clone https://github.com/theme-next/hexo-theme-next themes/next
    
#### 5.2 设置主题
在项目目录下的_config.yml中，配置theme
    
    theme: next 
可在/theme/{theme}/_config.yml 主题的配置文件下进行主题的配置，根据[官方文档](http://theme-next.iissnan.com/getting-started.html "")配置。`scheme` `language `等配置
   
### 6.设置分类功能
#### 6.1 启用分类功能
* 查看项目目录下的_config.yml 看是否有`category_dir: categories`
* 查看theme目录下的_config.yml 看是否有`categories: /categories`

#### 6.2 新建分类
使用如下命令，会在source目录下生成categories/index.md文件
    
    hexo new page categories
修改categories/index.md 增加`type`和`comments`字段

    ---
    title: categories
    date: 2018-06-20 20:50:29
    type: "categories"
    comments: false
    ---
#### 6.3 给文章设置分类
在文章的头部加上`categories`字段，通过这个字段设置分类

    ---
    title: GitHub搭建博客教程
    date: 2018-06-20 18:25:54
    categories: 
        - Git
    tags:
    ---
    
### 7.设置标签功能
#### 7.1 启用标签功能
* 查看项目目录下的_config.yml 看是否有`tag_dir: tags`
* 查看theme目录下的_config.yml 看是否有`tags: /tags`

#### 7.2 新建标签
使用如下命令，会在source目录下生成tags/index.md文件
    
    hexo new page tags
修改tags/index.md 增加`type`和`comments`字段

    ---
    title: tags
    date: 2018-06-21 10:32:06
    type: "tags"
    comments: false
    ---
    
#### 7.3 给文章设置分类
在文章的头部加上`tags `字段，通过这个字段设置分类

    ---
    title: GitHub搭建博客教程
    date: 2018-06-20 18:25:54
    categories: 
        - Git
    tags:
        - GitHub
    ---

### 8. 增加评论功能
#### 8.1 安装gitment

    npm install gitment --save
    
#### 8.2 在Github上注册

在[Github](https://github.com/settings/applications/new)进行注册，获取Client ID和Client Secret 

    Application name : phepe
    Homepage URL : https://phepe.github.io/
    Application description : blog
    Authorization callback URL : https://phepe.github.io/
 
#### 8.3 在主题里面开启Gitment评论功能

打开themes/next目录下的_config.yml文件进行修改并保存, 修改下面展示的字段

    gitment:
      enable: true
      github_user: phepe
      github_repo: phepe.github.io
      client_id: *******
      client_secret: *****

#### 8.4 初始化评论功能

生产网站，提交到github上，点击一个文章，初始化下方的评论按钮，会提示你用github登陆，发布评论，测试是否成功。











 













 