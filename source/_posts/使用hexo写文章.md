---
title: 使用hexo写文章
date: 2018-06-20 16:14:21
tags: 
- hexo
- markdown
categories:
- 编程工具
description: 本文主要简单介绍了如何使用hexo写文章，以及使用markdown写文章时可能遇到的一些问题，建议结合***一起食用味道更佳。
---

## 正文
这里分为两部分，一部分是博客源码，另一部分是操作博客。  
博客源码存放在github上，相当于博客的砖砖瓦瓦；hexo则是操作工具，用于将这些砖瓦建起来。

### 博客源码
博客源码一般通过git来进行操作管理。  
每次写博客前记得pull，写完博客发布后别忘了push！

### 操作博客
#### 安装hexo

    npm install -g hexo
hexo安装完成后，安装依赖包

    npm install

这里是hexo的一些常用命令，不需要记，后面用到了再来看就行  

```
    hexo g      # hexo generate 命令的简写，用于生成静态文件
    hexo s      # hexo server 命令的简写，用于启动服务器进行本地预览
    hexo d      # hexo deploy 命令的简写，用于将本地文件发布到github上
    hexo n      # hexo new 命令的简写，用于新建一篇文章
    hexo clean  # 清除缓存文件（db.json）和已生成的静态文件（public）
```
执行以下命令在本地启动

    hexo s
然后通过浏览器访问[http://localhost:4000](http://localhost:4000)，可以看到博客已经成功运行起来了。这是在本地运行的博客，也就是hexo的本地预览功能。  
接下来我们来把它挂载到github，让更多人喜欢工作，着迷工作，为工作疯狂。
先生成静态文件，执行以下命令

    hexo g
接着发布到git上

    hexo d
就可以在github上看到我们的博客了，比如我自己的博客地址就是[地址点这](https://darkindom.github.io/)进入

##### 新建文章
运行新建文章的命令

    hexo n "文章名字"
可以看到在本地的 ./source/_post/文件夹中已经新生成了一个md文件，hexo中文章是用markdown来写的  
```
title: my new post #可以改成中文的，如“新文章”
date: 2018-06-20 17:16:15 #发表日期，一般不改动
categories: blog #文章文类
tags: [文章] #文章标签，多于一项时用这种格式，只有一项时使用tags: blog
---
#这里是正文，用markdown写
```

##### 插入图片
markdown中插入图片的语法

    ![Alt text](image path)

1. 对于网上图片，只需要将图片的链接地址写入`image path`即可；
2. 对于本地图片，需要将图片存入`/source/images/`文件夹中，再将相对地址`/images/图片名称` 写入`image path`，例如

```
![Dk](/images/WechatIMG2.jpeg)
```
![aa](/images/WechatIMG2.jpeg)

3. 由于markdown没有控制图片大小的语法，所以控制图片大小要用 `<img>` 标签实现，例如

```
<img src='/images/WechatIMG2.jpeg' style='width: 200px;'/>
```

<img src='/images/WechatIMG2.jpeg' style='width: 200px;'/>

##### 发布文章
写完之后通过

    hexo s
在本地预览效果，满意之后再发布到git上，但是这里要注意，最好在前面再加上hexo clean以清除缓存文件和已生成的静态文件，避免没法正常提交新文件的错误。

    hexo clean
    hexo d
    hexo g
将文章同步到github上。  
最后记得`git push`将改动同步到源码上~

#### 三、参考
[嘟嘟独立博客 hexo干活系列：（一）hexo+github搭建个人独立博客](http://tengj.top/2016/02/22/hexo1/)


| 作者：[Dk](https://darkindom.github.io)