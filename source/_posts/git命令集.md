---
title: git命令集
date: 2018-07-19 17:05:07
tags: 
- git
categories:
- 编程工具
description: 一些git命令的笔记，本文仅作为&laquo;git常用命令&raquo;的补充
---

# 本地
git commit    提交记录
git log    查看记录
git branch xxx    新建分支xxx
    git checkout xxx    切换到xxx分支    git checkout -b xxx 新建xxx并切换到这个分支
    git branch -f master xxx    强制把master分支指向xxx
    git branch -d <branch-name>    删除分支
git merge yyy    把yyy合并到当前分支
git rebase xxx    取出提交记录，并“复制”它们，在另一个地方xxx逐个放下去，可以创造更线性的提交历史（原来的节点依然存在）

git checkout C1    分离HEAD到C1            cart.git/HEAD查看HEAD指向，git symbolic-ref HEAD查看HEAD指向的引用
git checkout HEAD^    ^父节点， ~n往上n个父节点

git reset HEAD~1    回退，同时撤销的节点在本地就不存在了。主要用于本地
git revert HEAD       回退，在撤销的提交记录后面多了一个新提交。主要用于远程

git cherry-pick <提交号>     将一些提交复制到当前所在的位置（HEAD）
git cherry-pick C2 C4
        注意 cherry-pick 不能取上游的节点

git rebase -i    交互式rebase    
git rebase -i HEAD~4    

git tag <tag name> <branch name>
git tag v1 C1        让标签v1指向提交记录C1，如果v不指定提交记录，则以HEAD所指向的位置
git describe <ref>    <ref>可以是任何能被git识别成提交记录的引用，如果没有指定的话 就是HEAD
    输出结果    <tag>_<numCommits>_g<hash>
                        tag表示离ref最近的标签，numCommits表示 ref和tag相差多少提交记录，hash表示ref的哈希值前几位
                        若ref提交记录上有某个标签时，只输出标签名称
    git bisect 查找产生BUG的提交记录的指令


# 远程
origin/master    origin即远程分支名字，master为本地分支。在检出远程分支时 会自动进入分离HEAD状态。
git clone    
git fetch    获取数据    1.从远程仓库下载本地仓库缺失的提交记录；2.更新远程分支指针（比如origin/master）到最新状态
                    fetch只是把远程的数据下载下来，并不会修改本地的文件
git pull    相当于 git fetch; git merge origin/master;
                也就是 git fetch 和 git merge <just-fetched-branch> 的缩写
git push    将你的变更上传到指定的远程仓库，并在远程仓库上合并你的新提交记录。
                不带任何参数时的行为与git的一个名为 push.default 的配置有关。

git pull --rebase     相当于 git fetch; git rebase
git pull; git merge    和上面效果一样，不过上面提交树干净，而下面保留了所有提交历史

master 和 origin/master 的关联关系是由分支的 remote tracking属性决定的，master被设定为跟踪 origin/master

远程跟踪    让任意分支跟踪 origin/master，该分支就会像master分支一样得到隐含的push目的地以及merge的目标。意味着你就可以在分支 totallyNotMaster 上执行 git push
两种方法设置这种属性：
    1.git checkout -b totallyNotMaster  origin/master        创建一个名为 totallyNotMaster的分支，让它跟踪origin/master
    2.git branch -u origin/master foo              这样foo就会跟踪origin/master，如果当前就在foo分支上，还可以省略foo

push参数
git push <remote> <place>
git push origin master    切到本地仓库的 master 分支，获取所有提交，再到远程仓库 origin 中找到 master 分支，将远程仓库中没有的提交记录都添加上去
            通过 place 参数告诉git提交记录来自于 master，要推送到远程仓库中的 master，实际就是要同步的两个仓库的位置
            因为已经通过指定参数告诉了git所有它需要的信息，所以它就忽略了我们所检出的分支的属性
    place    
    如果要同时指定源和目的地的<place>的话，只需要用冒号：将二者连起来即可
    git push origin <source>:<destination>
    git push origin foo^:master    source可以是任何git可以识别的位置！    如果目的分支不存在，git会在远程仓库中以目的分支的名字创建这个分支

fetch参数
push的参数都可以用在fetch上。只不过方向是相反的（push上传，fetch下载）
git fetch origin foo    git会到远程仓库的foo分支上，然后获取所有本地不存在的提交，放到本地的o/foo上

没有source
在push和fetch时可以不指定 source，方法是仅保留冒号和destination的部分，source部分留空
git push origin :side        删除远程远程仓库的side分支
git fetch origin :bugFix     在本地新建一个 bugFix分支

pull参数
pull就是fetch 和 merge的缩写，所以
    git pull origin foo         就相当于         git fetch origin foo; git merge o/foo
    git pull origin bar~1:bugFix   相当于   git fetch origin bar~1:bugFix; git merge bugFix
    git pull 唯一关注的就是提交最终合并到哪里，也就是fetch提供的 destination参数


| 作者：[Dk](https://github.com/Darkindom)
