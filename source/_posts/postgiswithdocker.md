---
title: 使用Docker部署PostgreSQL数据库、PostGIS插件
date: 2019-05-30 12:13:01
tags:
	- PostgreSQL
	- PostGIS
	- Docker
---

回顾自己写的文章，讲如何部署PostgreSQL和PostGIS有好几篇，包括[如何在不同的操作系统下部署PostgreSQL数据库](http://www.baiguangnan.com/2018/01/31/postgis/)，[如何配置PostgreSQL数据库](http://www.baiguangnan.com/2018/09/29/usenodemapnikwithpostgisoncentos/)，[如何迁移PostgreSQL数据库存储目录位置](http://www.baiguangnan.com/2018/12/29/centospostgresqlpgdata/)，[如何部署PostGIS插件，如何更新PostGIS插件](http://www.baiguangnan.com/2019/01/03/centosyumupdatepostgiscrash/)，[如何编译带ST_AsMVT函数的PostGIS插件](http://www.baiguangnan.com/2019/01/22/centospostgismvt/)等，最近在使用Docker，所以找了一下PostgreSQL和PostGIS的Docker镜像，发现上面说的那些只需要一个命令就可解决。。

#### 选择Docker镜像
在Docker Hub和网上搜索了一下，[mdillon/postgis](https://hub.docker.com/r/mdillon/postgis)是大家建议使用的，针对我们现在使用的PostgreSQL和PostGIS的版本，我选择了`10-alpine`镜像，总大小只有48M。

#### 启动容器
镜像pull到本地之后(不pull在启动的时候发现本地没有也会从Docker Hub上先下载再启动)，启动镜像：
```bash
docker run --name postgresql -d -p 5432:5432 -v /home/pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=dlwy mdillon/postgis:10-alpine
```

其中：
--name为设置容器名称
-d为设置容器在后台运行
-p为设置端口映射
-v为设置数据库存储目录映射，可以将数据库存储目录保存在主机目录，防止容器被删除之后数据库中的数据丢失
-e为设置数据库参数，这里设置的是数据库管理员密码

#### Docker部署如何解决之前的问题
1.部署和配置PostgreSQL、PostGIS：镜像中内置的PostgreSQL数据库配置文件已修改，支持局域网访问。内置了PostGIS插件。
2.更改PostgreSQL数据库存储目录位置，只需要通过-v指定目录映射即可。
3.支持ST_AsMVT函数，镜像中的PostGIS已经是基于protobuf-c编译的了，可以支持ST_AsMVT函数。
4.升级PostgreSQL、PostGIS版本，只需要pull最新版本的镜像，删除旧版本容器，启动新容器即可。

#### One More Thing
除了之前的问题之外：
1.使用Docker部署还可以在性能比较好的机器上部署多套数据库，通过-p端口号映射进行区分，部署的PostgreSQL数据库版本也可以不同，为以后的数据库负载均衡、读写分离、主从备份等创造条件。
2.通过Docker-Compose或者k8s进行容器编排，方便数据库、服务、前端等快速部署，减少运维人员使用难度。