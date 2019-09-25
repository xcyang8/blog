<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**目录**

- [Docker](#docker)
  - [搭建](#%E6%90%AD%E5%BB%BA)
  - [常用命令](#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
  - [maven插件构建镜像](#maven%E6%8F%92%E4%BB%B6%E6%9E%84%E5%BB%BA%E9%95%9C%E5%83%8F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Docker

### 搭建
>mac  `https://download.docker.com/mac/stable/Docker.dmg`  

>下载镜像 `http://hub-mirror.c.163.com`

### 常用命令
```
docker run                              启动容器
    -p 80:8080                          把本机端口映射到容器内的8080端口
    --link <name or id>:alias           连接另外一个容器
    -v mydir:imagedir                   挂在本地文件或者目录到容器内
docker stop                             关闭容器

docker image rm -f id                   强制删除image

docker exec -it id                  在运行的容器中执行命令
    OPTIONS说明：
    
    -d :分离模式: 在后台运行
    
    -i :即使没有附加也保持STDIN 打开
    
    -t :分配一个伪终端

docker logs --since 30m id              查看近30分钟内的日志

docker inspect id                       查看挂载文件
--------------------------------------------------------------------

docker 查看某个images所有tag

curl https://registry.hub.docker.com/v1/repositories/foxiswho/rocketmq/tags\
| tr -d '[\[\]" ]' | tr '}' '\n'\
| awk -F: -v image='foxiswho/rocketmq' '{if(NR!=NF && $3 != ""){printf("%s:%s\n",image,$3)}}'

```

### maven插件构建镜像


### docker-compose

#### 安装
下载包
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /mydata/docker-compose

将可执行权限应用于二进制文件：
sudo chmod +x /mydata/docker-compose

软连接（等于加入环境变量）
ln -s /mydata/docker-compose /usr/local/bin/docker-compose

检查安装成功
docker-compose --version
#### 环境
在docker-compose.yml同级目录创建.dev文件
```
URL=/Users/mac/docker-deploy
```

然后 docker-compose.yml 中配置${URL}

#### 常用命令
```
docker-compose up -d nginx                          构建建启动nignx容器

docker-compose exec nginx bash                      登录到nginx容器中
    
docker-compose down                                 删除所有nginx容器,镜像

docker-compose ps                                   显示所有容器

docker-compose restart nginx                        重新启动nginx容器

docker-compose run --no-deps --rm php-fpm php -v    在php-fpm中不启动关联容器，并容器执行php -v 执行完成后删除容器

docker-compose build nginx                          构建镜像 。        

docker-compose build --no-cache nginx               不带缓存的构建。

docker-compose logs  nginx                          查看nginx的日志 

docker-compose logs -f nginx                        查看nginx的实时日志
 
docker-compose config  -q                           验证（docker-compose.yml）文件配置，当配置正确时，不输出任何内容，当文件配置错误，输出错误信息。 

docker-compose events --json nginx                  以json的形式输出nginx的docker日志

docker-compose pause nginx                          暂停nignx容器

docker-compose unpause nginx                        恢复ningx容器

docker-compose rm nginx                             删除容器（删除前必须关闭容器）

docker-compose stop nginx                           停止nignx容器

docker-compose start nginx                          启动nignx容器
```
