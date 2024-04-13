# Git

## 1.安装

### 1.1.官网

[Git (git-scm.com)](https://git-scm.com/)

### 1.2.安装教程

[Git的安装与配置教程_git安装及配置教程-CSDN博客](https://blog.csdn.net/weixin_53415203/article/details/136083804?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPaidSort-1-136083804-blog-131104523.235%5Ev43%5Epc_blog_bottom_relevance_base2&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPaidSort-1-136083804-blog-131104523.235%5Ev43%5Epc_blog_bottom_relevance_base2&utm_relevant_index=1)

## 2.常用命令

### 2.1.初始化仓库

#### git init

### 2.2.将当前目录下的所有文件和文件夹添加到暂存区

#### git add .

### 2.3.将暂存区中的文件提交到本地仓库，并添加提交信息为"first"

#### git commit -m "first"

### 2.4.查看提交历史（按q退出查看）

#### git log

### 2.5.添加远程仓库名称为origin的远程仓库地址url

#### git remote add origin url

### 2.6.将本地分支main与远程仓库origin的main分支关联

#### git push -u origin main:main

### 2.7.拉取远程仓库名称为origin的main分支内容到本地

#### git pull origin main

### 2.8.查看远程仓库的地址

#### git remote -v

### 2.9.将远程仓库的名称从"origin"改为"gitee"

#### git remote rename origin gitee

### 2.10.切换到名为"master"的分支

#### git checkout master

### 2.11.创建并切换到名为"main"的新分支

#### git checkout -b main

## 3.提交代码到github/gitee常用的方式（我的工作流）

### 先在github/gitee创建远程仓库，复制远程仓库的url

### git clone url

### git add .

### git commit -m "first"

### git push -u origin main/master

## 4.在本地创建本地仓库做版本控制（我的工作流）

### git init

### git add .

### git commit -m "first"

### git push -u origin main

## 5.已将代码上传至gitee想将代码上传到github

### 需要先在本地创建main分支并切到main分支

#### git checkout -b main

### 在github创建仓库，复制url

### 本地仓库添加名称为 github的远程仓库

#### git remote add github url

### 设置本地仓库连接远程仓库地址

#### git remote set-url github url

### 拉取远程仓库代码

#### git pull origin main --allow-unrelated-histories

### 将代码提交一次到main分支

#### git add .

#### git commit -m "first"

### 通过main分支推送代码到github

#### git push -u github main:main























