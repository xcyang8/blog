<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**目录**

- [linux](#linux)
  - [小技巧](#%E5%B0%8F%E6%8A%80%E5%B7%A7)
  - [日志技巧](#%E6%97%A5%E5%BF%97%E6%8A%80%E5%B7%A7)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## linux

### 小技巧
1. centos7 机器重命名
> 用于区分不同系统

```
# hostname NEW_NAME   
<这种方法只对当前系统有效，重启后无效>

# hostnamectl set-hostname NEW_NAME
<设定主机名，永久有效>
```


### 日志技巧

1. 筛选 含有xcy
```
cat info.log | grep 'xcy'
```