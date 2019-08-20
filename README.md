<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**目录**

- [Blog](#blog)
  - [服务启动](#%E6%9C%8D%E5%8A%A1%E5%90%AF%E5%8A%A8)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Blog

### 服务启动
   
   前提安装Node.js 和 docsify
   
   ```
   docsify serve ./index
   ```
   
   或者运行bat脚本
   
### 更新目录
  将pre-commit 复制到当前项目内`.git\hooks`下  
  代码提交时 自动更新 Home.md,README.md,docs下所有文件
  使用的是[doctoc](https://github.com/thlorenz/doctoc)