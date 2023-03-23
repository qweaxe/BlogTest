---
title: Github + Hexo 的个人博客部署
date: 2023-03-17 23:24:49
tags:
---
一、所需环境
====

**这是基于Github和Hexo框架所构成的个人博客**，测试下字体大小
1.远程类：git
2.脚本类：Node.js
3.环境框架：Hexo
4.文本编辑器：可以编辑md文件的，待补充，暂时使用的是VScode
5.其他：

二、git、nodejs等环境的安装
====

1.安装：
git网址：https://git-scm.com/download/win
nodejs网址https://nodejs.org/en
2.Git的配置：
（1）配置用户名：git config --global user.name "此处写用户名"
（2）配置邮箱：git config --global user.name"此处写邮箱"
（3）查找.gitconfig文件：位置一般在C:/Users/[username]/.gitconfig
（4）ssh密钥登陆：此方式保密性高，更实用

3.安装Hexo框架：
（1）初始化：命令行安装（**不是Git！！！**）npm(Node.js 包管理工具)
（2）命令行安装Hexo：基于上一步的npm，命令：`npm install -g hexo -cli`
（3）本地硬盘新建文件夹，作为存放代码的地方：
（4）在（3）中创建的文件夹右键鼠标，点击 Git Bash；执行如下命令：`hexo init` ；`hexo g`； `hexo s`；

三、本地项目的构建
=====


四、推送到Github
======

1.建立项目：
**先要在Github上建立项目！**建立了项目之后再生成ssh密钥并登录
2.补充：
在此目录下安装hexo-deployer-git插件：`npm install hexo-deployer-git --save`；编辑目录下的_config.yml文件，修改配置文件（此处有问题，自己）
3.建立分支：
利用分支作为修改文件，master作为访问文件，方便多处终端修改
注意：克隆远程仓库到本地时不要账户名，直接使用：`git clone git@github.com:xxxxxx/xxxxxx.git`即可
4.推送：
`hexo clean`：清除本地项目
`hexo g`：重新生成
`hexo s`：开启本地预览
`hexo d`：推送到Github

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

六、注意事项
======

1.同步和更新的区别：
---

同步指的是不同终端的原始文件的同步，使用的命令为：`git add .`, `git commit -m '(更新备注)'`，`git push`；每次要更新内容时，都要从仓库拉取源代码：`git pull`；保证文件是最新内容，养成良好的习惯。
而更新指的是网页内容本身的更新，使用的是：`hexo clean`&`hexo g`，`hexo d`；若要预览则在生成后输入`hexo s`；

Reference
=======

>https://blog.csdn.net/qq_44732432/article/details/124714408
>https://www.jianshu.com/p/36564cf5191a
>https://hexo.io/zh-cn/docs/github-pages
>https://www.dhndzwxj.top/584278389.html
>https://www.bilibili.com/video/BV1ts4y1f7Gu/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click
>https://www.jianshu.com/p/0b1fccce74e0