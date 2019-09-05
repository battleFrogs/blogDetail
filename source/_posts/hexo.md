---
title: hexo常用命令
layout: post
tags:   
  - 常用命令  
---

hexo，一个轻量、简易、快速且高效的博客框架

<!--more-->


### npm包命令  


    npm install hexo -g #安装  
    hexo -v 检查hexo是否安装成功
    hexo init 初始化该文件夹
    npm install，安装所需要的组件
    输入hexo g，首次体验Hexo
    hexo s，开启服务器，访问该网址，正式体验Hexo
    npm update hexo -g #升级    


### 文章模板内容

    title: 使用Hexo搭建个人博客
    layout: post
    date: 2014-03-03 19:07:43
    comments: true
    categories: Blog
    tags: 
        - Testing
    keywords: Hexo, Blog
    description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
    
    <!--more--> 显示摘要   
    
    ![ico原来的样子](/assets/blogImg/xmas_ico0.jpg)   显示图片




### 基本命令使用   

    hexo n == hexo new "a new post"  新建文章，最好用双引号括起来
    
    hexo g == hexo generate     生成静态文件到public文件夹
    
    hexo s == hexo server    Server at localhost:4000，根目录为public
    
    hexo d == hexo deploy   部署到远程服务里，例如github
    
    hexo p == hexo publish  新建草稿draft
    
    hexo clean     清除缓存文件
    
    hexo new page "about"   生成 /source/about/index.md 文件   
    
    
### 服务使用
    
    hexo server    默认为动态监听

    hexo server -s   静态模式
    
    hexo server -p 5000  指定端口
    
    hexo server -i 10.20.62.123   指定IP