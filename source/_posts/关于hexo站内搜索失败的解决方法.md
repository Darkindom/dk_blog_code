---
title: 关于hexo站内搜索失败的解决方法
date: 2018-08-03 18:20:02
tags:
- hexo
- markdown
categories:
- 编程工具
description: 在使用hexo的站内搜索时，经常出现，点击搜索按钮后，一直处于loading状态，经查阅资料后发现是因为在写markdown文件的时候出现了一些奇怪的ASCII码，比如表示退格键的BS。
---

## 搭建站内搜索
在搭建hexo的过程中，想实现站内搜索的功能，从官网找到如下步骤：
![local-search](/images/search-problem/local-search.png)

## 出现问题v
跟着上面步骤做完，发现点击搜索按钮，页面就一直处于loading的状态。

通过查阅资料后得知，是由于我们的文章markdown文件中，出现了一些奇怪的ASCII码，比如表示退格键的BS：
![ASCII-bs](/images/search-problem/ASCII-bs.png)
可以看到，代码中出现了一个很小的BS，
通过光标在文章中移动，可以很明显地感觉到光标在该字符处需要移动两次，但单从外观上几乎难以看出来，因为编辑器显示的时候不会将其显示出来：
![bs-error](/images/search-problem/bs-error.png)

这个就是导致我们站内搜索一直失败的罪魁祸首。

## 解决问题
那么如何解决这个问题呢？
### 手动删除
在每一篇文章中，通过替换的方法将其替换为空。

### Remove backspace control character
使用插件 [Remove backspace control character](https://marketplace.visualstudio.com/items?itemName=satokaz.vscode-bs-ctrlchar-remover)

安装好该插件后，通过 `Format Document`（Mac中通过Command + Shift + P 唤起，然后输入Format Document）的命令将文章中的异常字符去除，再保存

这样就可以正常使用站内搜索啦~！

参考: [vscode控制字符引起的问题以及解决思路](https://wdd.js.org/vscode-control-characters-problem.html)

| 作者：[Dk](https://darkindom.github.io)