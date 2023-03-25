---
title: Github + Hexo 的个人博客部署
date: 2023-03-17 23:24:49
tags:
  blog
---
一、所需环境
====


**这是基于Github和Hexo框架所构成的个人博客，考虑是否会出Gitee版，应该会出在自己服务器上架设的版本**，会把每个点的原因尽力讲清楚而不是单纯的操作指南（不然和只会执行指令的机器有什么区别呢？）
1.远程类：git（代码管理工具，可以用其推送至远程仓库等）
2.脚本类：Node.js（脚本语言）
3.环境框架：Hexo（博客框架，使用Markdown格式书写）
4.文本编辑器：可以编辑md文件的，待补充，暂时使用的是VScode
5.其他：


二、git、nodejs等环境的安装
====

1.安装：
---

git网址：https://git-scm.com/download/win
nodejs网址：https://nodejs.org/en
检查是否安装成功：在命令行（Win+R，输入cmd）中输入`git -version`（-v 也行），若出现（补张图）版本号，git安装成功；同样是命令行输入`node -v`，出现版本号就成功了；不知道是否需要手动添加环境变量，貌似不用；

2.Git的配置：
---

（1）配置用户名：git config --global user.name "此处写用户名"
（2）配置邮箱：git config --global user.name"此处写邮箱"
（3）查找.gitconfig文件：位置一般在C:/Users/[username]/.gitconfig；编辑器打开看\[user\]栏是否出现设置的用户名和邮箱账号(补图)
（4）建立项目：（创建GitHub账户略）新建一个名为：用户名.github.io的仓库(**必须是此格式！！**)（补个图）；邮箱需要验证才能正常使用github功能
（5）ssh密钥登陆：此方式相比用户名和密码登陆保密性高，更实用；①创建：`ssh-keygen-t rsa -C "xxxx@xxx.com"`；②（补图）可以考虑额外的措施，一般直接回车3次确认；③生成的文件为 id_rsa id_rsa.pub 存放在 C:\Users\admin.ssh 文件夹下；④添加 ssh key：打开上述文件或者在git中输入：`cat ~/.ssh/id_rsa.pub`查询key，然后进入：settings\deploy keys\add deploy key(补图) Title：最好填写和终端有关的信息，方便区分；Key：复制上述查询内容

3.安装Hexo框架：
（1）初始化：命令行安装（**不是Git！！！**）npm(Node.js 包管理工具)输入：`npm install `**新版Nodejs已经集成了，无需额外安装！！！**；测试是否安装完成：命令行中输入 `npm -v`，出现版本号就说明安装成功
（2）命令行安装Hexo：基于上一步的npm，命令：`npm install -g hexo -cli`；安装完成后输入`hexo -v`，出现版本行则安装成功
（3）本地硬盘新建文件夹（**记住这个位置**），作为存放代码的地方；
（4）在（3）中创建的文件夹右键鼠标，点击 Git Bash；执行如下命令：
`hexo init` ：初始化命令，自动下载文件到此目录；
`hexo g`：生成命令，完成框架； 
`hexo s`：启动本地预览服务，打开链接 http://localhost:4000  可看到初始化是否成功；若加载不出来，考虑一下是否是4000端口被占用了，若是则关闭一下

三、本地项目的构建
=====


四、推送到Github
======

**先要在Github上建立项目！**建立了项目之后再生成ssh密钥并登录（见上文）


1.补充：
---

在此目录下安装hexo-deployer-git插件：`npm install hexo-deployer-git --save`；编辑目录下的_config.yml文件，修改配置文件（此处有问题，自己）

2.建立分支：
---

利用分支作为修改文件，master作为访问文件，方便多处终端修改
注意：克隆远程仓库到本地时不要账户名，直接使用：`git clone git@github.com:xxxxxx/xxxxxx.git`即可

3.配置：
---

修改目录下的_cofig.yml文件，在文件末尾修改配置文件：
```
deploy:
  type: 'git'
  repo:https://github.com/用户名/项目名.git 或者 git@github.com:用户名/项目名.git//(http或者ssh都可以)
  branch: main  //也可以单独设置个分支专用于更新
```

4.推送：
---
（1）仅修改文章内容：
`hexo clean`：清除本地项目
`hexo g`：重新生成
`hexo s`：开启本地预览
`hexo d`：推送到Github
浏览器输入相对的域名，查看是否更新！若有页面无更新，考虑多用几次CTRL + F5 刷新（一次不一定刷新的出来）
（2）如果是多终端同步：
注意git仓库内容的同步
`git add .`：检查本地文件的变动
`git commit -m '注释内容'`：给推送文件写上注释，养成好习惯，更新啥就写啥，方便后续管理
`git push`:推送到github，注意这里改变的是源文件内容

五、博客本身的优化
=======

1.主题问题：
---

使用分支更新的方式会导致不能直接在分支目录中从仓库 `git clone`想要的主题，只能先从其他地方clone过来，主要原因是直接clone会生成.git目录，一个git仓库中不能包含另一个git仓库

2.图片上传问题：
---

若使用markdown格式，就只能本地图片位置目录或者网页链接，不能直接放在md文件里；
需要修改_config.yml，将`post_asset_folder: false`改为`ture`，此方式不好用！
插件方法：hexo-renderer-marked，用命令`npm install hexo renderer-marked`安装，再在_config.yml更改配置：
`post_asset_folder: true`
`marked:`
  `prependRoot: true`
  `postAsset: true`
更方便的方法：配合markdown编辑器

3.更新问题：
---

更新速度偏慢；自动更新检测：；

4.字体问题：
---

想用：https://github.com/lxgw/LxgwWenKai-Screen 作为网页的显示字体
方法：https://github.com/chawyehsu/lxgw-wenkai-webfont

六、博客本身的结构设计（待补充）
=====

1.主题页面
---

2.标签页
---

3.分类页
---

4.图库
---

5.子页面
---

6.404页面
---


七、注意事项
======

1.同步和更新的区别：
---

同步指的是不同终端的原始文件的同步，使用的命令依次为：`git add .`, `git commit -m '(更新备注)'`，`git push`；每次要更新内容时，都要从仓库拉取源代码：`git pull`；保证文件是最新内容，养成良好的习惯。
而更新指的是网页内容本身的更新，使用的是：`hexo clean`&`hexo g`，`hexo d`；若要预览则在生成后输入`hexo s`；
注意：git pull的时候注意系统提示的本地文件是否改动：
error: Your local changes to the following files would be overwritten by merge:
        source/_posts/Github-Hexo-xxxx.md
Please commit your changes or stash them before you merge.
方法：①放弃本地修改（常用）：
输入`git reset --hard`,`git pull`，本地代码被丢弃，不可找回；
②保存当前工作进度：
`git stash`:保存当前工作进度，将所有未提交的修改（工作区与暂存区）保存至堆栈中,同样可用`git stash save`，区别是可以写注释
`git pull`:拉取仓库内容至本地,
`git stash pop`：将本地栈中的内容pop到本地，会将stash列表中的信息删除；而使用`git stash apply`则会保留；
`git stash list`：存储到本地栈顶后，使用此命令查看本地存储的stash日志
`git stash clear`：清空Git栈，原来stash的节点都会被清除
所以要要养成好习惯，每次修改完成后及时推送，不然会出现此问题；


八、后续更新
=======

1.如何写一篇博客
---
2.自己主题的缝合与魔改
---
3.代码相关知识
---
Reference
=======

https://blog.csdn.net/qq_44732432/article/details/124714408
https://www.jianshu.com/p/36564cf5191a
https://hexo.io/zh-cn/docs/github-pages
https://www.dhndzwxj.top/584278389.html
https://www.bilibili.com/video/BV1ts4y1f7Gu/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click
https://www.jianshu.com/p/0b1fccce74e0