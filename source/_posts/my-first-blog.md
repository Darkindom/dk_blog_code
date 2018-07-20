---
title: hexo+next搭建个人博客
date: 2018-06-20 16:14:20
tags:
- hexo
- next
categories:
- 编程工具
description: 这个博客的搭建使用的是静态博客搭建框架hexo，配与next作为博客主题，最后将博客放到github上。省去了申请域名等麻烦的工作，使得搭建博客变得轻松简单。
---
这个博客的搭建使用的是静态博客搭建框架hexo，配与next作为博客主题，最后将博客放到github上。省去了申请域名等麻烦的工作，使得搭建博客变得轻松简单。
> Hexo是一个快速、简洁且高效的博客框架。Hexo使用Markdown或其他渲染引擎解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

Next是hexo下的一个主题，在hexo中，阔以通过切换主题实现博客外观的改变。

# 安装
Hexo依赖
* Node.js
* git

需要电脑上先安装好Node.js和git
- - - 
然后是安装hexo
```
npm install -g hexo
```

安装完毕后，选择一个文件夹，执行`hexo init`指令生成建立博客所需要的文件
```
hexo init
```

接着是安装所需要的依赖
```
npm install
```

等待依赖安装的过程可以看一下hexo的一些常用命令，不需要记，后面用到了再来看就行
```
    hexo g      # hexo generate 命令的简写，用于生成静态文件
    hexo s      # hexo server 命令的简写，用于启动服务器进行本地预览
    hexo d      # hexo deploy 命令的简写，用于将本地文件发布到github上
    hexo n      # hexo new 命令的简写，用于新建一篇文章
    hexo clean  # 清除缓存文件（db.json）和已生成的静态文件（public）
```

在依赖安装完毕后，执行`hexo g`和`hexo s`，生成静态文件并启动服务器  
```
hexo g
hexo s
```

![hexos](/images/my-first-blog/my-first-blog-pic1.png)

可以看到博客已经运行在 localhost:4000 端口上了，打开浏览器访问该地址即可以看到我们的博客已经搭建起来了，在这里可以非常方便地进行本地预览。
更多细节可以参考[官方文档](https://hexo.io/zh-cn/)

# 主题
hexo默认使用的主题是landscape，我这里使用了最近比较流行的next主题。
hexo安装主题的过程十分简单，在目录下找到`themes`文件夹，将要使用的主题文件夹拷入其中，再稍微修改一下配置即可。

## 安装next
next有两种安装方式
第一种是直接使用 git 克隆到`themes`文件夹，之后也可以直接通过`git pull`进行更新
```
// 定位至themes文件夹目录下
git clone https://github.com/iissnan/hexo-theme-next
```
第二种是下载压缩包，然后解压至`themes`文件夹
[next版本发布页面](https://github.com/iissnan/hexo-theme-next/releases)

## 启用主题
把next放入`themes`文件夹后，找到**站点配置文件**（根目录下的`_config.yml`文件），将`theme`字段的值改为`next`。如图所示
![config](/images/my-first-blog/my-first-blog-pic2.png)

这时候执行`ctrl + C`中止本地服务器，然后通过`hexo clean`清除缓存后，再启动服务器，就可以看到博客的主题已经变成了next了。
```
hexo clean
hexo s
```
next有很多可以自行配置的设定，如主题设定，语言设定，菜单设定，侧栏设定等等，还有很多诸如评论系统，内容分享，数据统计等强大功能。
这里先埋个坑，不多赘述，详情可以前往[next官网](http://theme-next.iissnan.com/)查看，以后有时间再补充。

# 部署到github
## github仓库配置
本地预览得满意了，下一步当然就是部署到网上给别人观赏~
如果还未拥有github账号，就先去注册申请一个。

新建一个仓库，名字必须是`你的github账号名.github.io`
如图所示，我的账号名是Darkindom，所以我的仓库名相应的就是`Darkindom.github.io`(这里仓库名前面要和你的账号名一致)
<img src='/images/my-first-blog/my-first-blog-pic3.png' style='width: 400px;' alt='respository'/>

## 部署本地文件
找到**站点配置文件**（根目录下的`_config.yml`文件），将其中的deploy改成以下格式（如果没有该字段就新建一个）。

![deploy](/images/my-first-blog/my-first-blog-pic4.png)

这里如果是第一次使用github，或者更改过账号，可能需要重新配置一下SSH  
（因为之前使用公司的博客时，用的是另一个github账号，所以切换回来后部署的时候说权限错误，就需要重新配置SSH）

输入以下命令，如果提示要你输入的时候可以先输入回车，如果提示是否要覆盖原先SSH(y/n)输入y
```
ssh-keygen -t rsa -C 'your@email.com'
```
<!-- ![SSH-1](/images/my-first-blog/my-first-blog-pic5.png) -->
![SSH-2](/images/my-first-blog/my-first-blog-pic6.png)

接着输入以下命令
```
ssh-agent -s
```
![SSH-3](/images/my-first-blog/my-first-blog-pic7.png)
> 如果这一步出错，就输入以下命令
```
eval `ssh-agent -s`
ssh-add
```

接下来就可以把SSH拷贝出来，添加到github账户上了
```
cat ~/.ssh/id_rsa.pub
```
将控制台里的那一长串SSH拷贝，打开github账户
<img src='/images/my-first-blog/my-first-blog-pic8.png' style='width: 200px;' alt='ssh-setting'/>
![SSH-4](/images/my-first-blog/my-first-blog-pic9.png)
title随便起一个自己容易辨别的，key里粘贴刚刚复制的SSH

最后测试一下，
```
ssh -T git@github.com
```
显示这样就是SSH配置好了
![SSH-5](/images/my-first-blog/my-first-blog-pic10.png)

执行`hexo g`，`hexo d`部署到github上
```
hexo g
hexo d
```
如果报了`Error Deployer not found: git`的错
就安装`hexo-deployer-git`模块
```
npm install --save hexo-deployer-git
```
再执行`hexo d`,完成部署。
打开浏览器，输入地址，比如我的就是[https://darkindom.github.io/](https://darkindom.github.io/)

大功告成~！

# 发布文章
关于发布文章的可以看我的另一篇博客《使用hexo写文章》
这里再提一点，多人合作的话可以将博客的相关代码保存在另一个git仓库，这样每次发布或者修改文章了，都可以备份到另一个仓库里，然后给权限即可。

# 参考
[嘟嘟独立博客 hexo干活系列：（一）hexo+github搭建个人独立博客](http://tengj.top/2016/02/22/hexo1/)

| 作者：[Dk](https://darkindom.github.io)
