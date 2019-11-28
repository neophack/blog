---
title: 一种便携式容器
date: 2019-11-23 20:53:11
tags: [容器,docker,singularity]
categories: 深度学习
---
内网环境安装docker和nvidia docker繁琐，本文介绍一种便携式容器singularity，安装简便（主要是2版本），仓库就可以直接安装。本文并介绍如何将docker镜像转换到singularity部署。

<!-- more -->
# 基本使用

```
USAGE: singularity [global options...] <command> [command options...] ...

GLOBAL OPTIONS:
    -d|--debug    打印调试信息
    -h|--help     显示使用帮助
    -s|--silent   仅打印错误
    -q|--quiet    关闭输出信息
       --version  显示应用版本
    -v|--verbose  啰嗦模式
    -x|--sh-debug 打印shell调试信息

GENERAL COMMANDS:
    help       Show additional help for a command or container    
    selftest   Run some self tests for singularity install         

CONTAINER USAGE COMMANDS:
    exec       执行容器中的命令     
    run        运行容器中预设命令  
    shell      在容器中运行shell    
    test       运行容器中test脚本   

CONTAINER MANAGEMENT COMMANDS:
    apps       List available apps within a container       
    bootstrap  *Deprecated* use build instead     
    build      Build a new Singularity container 
    check      Perform container lint checks 
    inspect    Display container's metadata          
    pull       Pull a Singularity/Docker container to $PWD  

COMMAND GROUPS:
    image      Container image command group 
    instance   Persistent instance command group      


CONTAINER USAGE OPTIONS:
    see singularity help <command>

For any additional help or support visit the Singularity
website: http://singularity.lbl.gov/

```
# 转换docker镜像到singularity

转换支持本地docker镜像，查看本地镜像列表通过`docker images`命令，其中quay.io/singularity/docker2singularity版本根据实际使用版本修改
```bash
mkdir /tmp/test
# convert ubuntu:14.04
docker run -v /var/run/docker.sock:/var/run/docker.sock \
-v /tmp/test:/output \
--privileged -t --rm \
quay.io/singularity/docker2singularity:v2.4 \
ubuntu:14.04
# convert neo-ai
docker run -v /var/run/docker.sock:/var/run/docker.sock \
-v /tmp/test:/output \
--privileged -t --rm \
quay.io/singularity/docker2singularity:v2.4 \
registry.cn-shenzhen.aliyuncs.com/neoneone/neo-ai

```

singularity运行镜像内jupyter程序
```bash
LANGUAGE=en sudo singularity run --nv registry.cn-shenzhen.aliyuncs.com_neoneone_neo-ai-2019-11-23-331a86220733.simg 
```