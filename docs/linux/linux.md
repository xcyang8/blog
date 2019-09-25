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