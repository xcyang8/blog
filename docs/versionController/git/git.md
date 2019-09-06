<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**目录**

- [常用命令](#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
- [高级用法](#%E9%AB%98%E7%BA%A7%E7%94%A8%E6%B3%95)
  - [勾子](#%E5%8B%BE%E5%AD%90)
  - [多次commit合并](#%E5%A4%9A%E6%AC%A1commit%E5%90%88%E5%B9%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### 常用命令
 - 从当前分支 创建新分支
    ```
    git checkout name
    ```
 - 从当前分支 创建新远程分支
    ```
    git chekcout -b name
    ```
 - 修改远程仓库地址  
    一天把github的项目 迁到gitee的私人库
    
    ```
    git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
    ```
    
 - 查看当前分支的远程分支地址
    ```
    git remote -v
    ```
 
 - 把本地库添加到远程库
    ```
    git remote add origin https://github.com/USERNAME/REPOSITORY.git
    ```
### 高级用法

#### 勾子
    //Todo待整理
    
#### 多次commit合并
```
git rebase -i HEAD~[number_of_commits]
```

我们先在git上提交两个修改。git log如下：  
```
commit 7b16b280f22fe4ff57c1879867a624f6f0f14398Author: pan Date:   Sun Apr 22 08:55:32 2018 +0800    update3
commit a7186d862b95efc5cc1d7c98277af4c72bac335dAuthor: pan Date:   Sun Apr 22 08:55:16 2018 +0800    update2
commit 16a9a4749f8ee25ab617c46925f57c2fa8a4937eAuthor: pan Date:   Sun Apr 22 08:54:55 2018 +0800    update1
```
合并3个成一个
```
git rebase -i HEAD~3
```
执行命令会提示：  
```
pick 16a9a47 update3 
pick a7186d8 update2
pick 7b16b28 update1 
# Rebase a9269a3..7b16b28 onto a9269a3 (3 commands) # # Commands: 
# p, pick = use commit 
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit## These lines can be re-ordered; they are executed from top to bottom.
## If you remove a line here THAT COMMIT WILL BE LOST.## However, if you remove everything, the rebase will be aborted.
## Note that empty commits are commented out~
含义：

选择pick操作，git会应用这个补丁，以同样的提交信息（commit message）保存提交选择reword操作，git会应用这个补丁，但需要重新编辑提交信息
选择edit操作，git会应用这个补丁，但会因为amending而终止
选择squash操作，git会应用这个补丁，但会与之前的提交合并
选择fixup操作，git会应用这个补丁，但会丢掉提交日志
选择exec操作，git会在shell中运行这个命令
```
保留最新一次，其他两次合并，然后保存退出
```
pick 16a9a47 update3 
s a7186d8 update2
s 7b16b28 update1
```
执行完后会提示合并后的提交信息，不修改则直接保存退出