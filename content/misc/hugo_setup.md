+++
title = 'Hugo_setup'
date = 2023-09-23T13:19:26+08:00
+++

[TOC]
<!--[toc]--> 

hugo nexT主题：https://github.com/hugo-next/hugo-theme-next/releases  
hugo工具：


## 创建站点：
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

## 创建分支
git branch -M main

## 确定要推送的远程仓库  
git remote add origin https://github.com/<用户名>/<仓库名>.git

## 推送到远程仓库
git push -u origin main

## git配置

查看git config： git config --global --list

配置用户名和邮箱：
git config --global user.name 
git config --global user.email

新建ssh key：
ssh-keygen -t rsa -C "邮箱"

git init
git add README.md
git commit -m "first commit"
git branch -M main
git@github.com:tangzhq/tangzhq.github.io.git //ssh方式
git remote add origin https://github.com/tangzhq/tangzhq.github.io.git //http方式
git remote set-url origin git@github.com:tangzhq/tangzhq.github.io.git //切换成ssh方式
git push -u origin main