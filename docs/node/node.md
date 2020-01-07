<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**目录**

- [node.js](#nodejs)
  - [异常](#%E5%BC%82%E5%B8%B8)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## node.js

### 异常

提示无权限
```
sudo npm i docsify-cli -g  提示无权限

gyp ERR! stack Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/docsify-cli/node_modules/fsevents/.node-gyp'
```

手动更改npm的默认目录
[解决方法](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)