---
title: "Docker Swarm 集群创建过程" 
date: 2019-05-11
lastmod: 2019-05-11
draft: false
categories: ["Clavicula Salomonis"]
tags: ["Docker"]
author: "sherry"
---
Docker Swarm 入门用到的命令不多，初始化的时候提示写的就很详细，一步步下来感觉用起来很方便。

## 初始化

```bash
$ sudo docker swarm init
```

<!--more-->

执行上句后，会有类似如下的东西出现：

> Swarm initialized: current node (xxxxxxxxxxxxxxxxxxxxxxxxx) is now a manager.  
To add a worker to this swarm, run the following command:  
    docker swarm join --token xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx [ip]:2377  
To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

## 加入集群

直接复制初始化后显示的命令，在同一网段的另一台主机上输入即可。如果成功的话，会出现如下反馈：

> This node joined a swarm as a worker.

如果不小心关掉了 terminal ，还可以通过以下命令看到 token：

```bash
$ sudo docker swarm join-token worker
```

还可以查看添加管理节点到集群的 token：

```bash
$ sudo docker swarm join-token manager
```

## 查看

查看集群节点列表：

```bash
$ sudo docker node ls
```

查看集群上的服务：

```bash
$ sudo docker service ls
```

## 终止集群

``` bash
$ sudo docker swarm leave --force
```

## 参考资料

1. [docker docs part 3 & part 4](https://docs.docker.com/get-started/part3/)