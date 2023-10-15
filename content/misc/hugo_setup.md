+++
title = 'Hugo_setup'
date = 2023-09-23T13:19:26+08:00
+++

hugo nexT主题：  https://github.com/hugo-next/hugo-theme-next/releases  
hugo工具：  https://github.com/gohugoio/hugo/releases  


## 创建站点  
hugo new site site_name  
cd site_name  
git init  
将主题hugo-theme-next放置到themes文件夹下  
拷贝主题中exampleSite 目录的config.yaml文件到根目录  
新建一篇blog：  
hugo new content_type/content_name.md

hugo server  
注意若 content_name.md 的参数 draft 为true，则执行上命令后文章并不显示

若要显示 draft 为 true 的草稿，则用下命令  
hugo server -D  
若要在之后网页中显示文章，则要把 draft 改为 false  

## GIT操作  

### 创建分支  
git branch -M main  
在git branch命令中，-M选项用于重命名分支。 当使用-M选项时，可以将分支重命名为指定的名称，即使已存在同名的分支也不会产生错误。 这将替换现有的同名分支。  
命令执行后，Git会将默认分支重命名为"main"。 如果之前没有名为"main"的分支，将创建该分支。 如果已经存在名为"main"的分支，则会覆盖该分支的内容。  

### 确定要推送的远程仓库  
git remote add origin https://github.com/<用户名>/<仓库名>.git
添加远程仓库的映射，远程仓库在本地的别名一般是origin  

从github远程仓库clone，默认情况下当前指针将位于`remotes/origin/HEAD`，是远程仓库引用的符号分支  
可以使用`git remote set-head origin -d`删除`origin/HEAD`符号引用  

### 推送到远程仓库
git push -u origin main
带上-u参数其实就相当于记录了push到远端分支的默认值，当下次要继续push到这个远端分支的时候推送命令可以简写成`git push`  

### 示例
git init  
git add README.md  
git commit -m "first commit"  
git branch -M main  
git remote add origin git@github.com:tangzhq/tangzhq.github.io.git //ssh方式  
git remote add origin https://github.com/tangzhq/tangzhq.github.io.git //http方式  
git remote set-url origin git@github.com:tangzhq/tangzhq.github.io.git //切换成ssh方式  
git push -u origin main  

### github配置

查看git config： git config \-\-global \-\-list

配置用户名和邮箱：  
git config \-\-global user.name "用户名"  
git config \-\-global user.email "邮箱地址"

新建ssh key：
ssh-keygen \-t rsa \-C "邮箱"

### 合并提交记录
git rebase \-i 用法:  
https://blog.csdn.net/the_power/article/details/104651772/  
https://www.jb51.net/article/191650.htm  
https://banbao991.github.io/archives/  
如果想把 rebase 之后的 main 分支推送到远程仓库，Git 会阻止你这么做，因为两个分支包含冲突。但你可以传入 \-force 标记来强行推送。  


## 标签页
添加tag方法：需要新增标签页
https://blog.csdn.net/weixin_48927364/article/details/123295436





