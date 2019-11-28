---
title: 深度学习环境镜像
date: 2019-11-27 22:13:11
tags: [镜像,docker]
categories: 深度学习
---
随着深度学习新技术的出现，cuda版本越来越高，升级导致之前编译过的程序不可用。本文介绍个人修改的docker，基于nvidia的tensorrt镜像cuda10.1版本，添加opencv+contrib3.4.8，添加常用机器学习框架，添加simpledet版的mxnet等。

<!-- more -->
# 介绍

理想的深度学习开发和导出工具，这个项目是为了创建一个新的开发环境，使用docker开发数据科学中的人工智能模型，特别是计算机视觉。

# pull镜像

镜像地址
[https://hub.docker.com/r/neoneone/neo-ai](https://hub.docker.com/r/neoneone/neo-ai)

## 使用docker
```
docker pull neoneone/neo-ai
# mirror
docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/neo-ai
```

## 使用singularity
```
singularity pull docker://neoneone/neo-ai
# mirror
singularity pull docker://registry.cn-shenzhen.aliyuncs.com/neoneone/neo-ai
```

# 运行镜像

docker运行

```
# 运行镜像
docker run -it --rm --runtime=nvidia -v $(pwd):/workspace -w /workspace -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -p 8888:8888 -p 6006:6006 registry.cn-shenzhen.aliyuncs.com/neoneone/neo-ai:latest
# 查看运行的容器
docker ps -a
# shell访问容器
docker exec -it a1d1dd53a7ba /bin/bash
```
singularity运行
```
singularity shell --nv registry.cn-shenzhen.aliyuncs.com_neoneone_neo-ai-2019-11-23-331a86220733.simg 
```

# 测试程序yolo

代码地址[https://github.com/NVIDIA-AI-IOT/deepstream_reference_apps/tree/restructure/yolo](https://github.com/NVIDIA-AI-IOT/deepstream_reference_apps/tree/restructure/yolo)

在`yolo/apps/trt-yolo`目录下的CMakeLists.txt的36行修改cuda版本
```cmake
find_package(CUDA 10.1 EXACT REQUIRED cudart cublas curand)
```

在`data/test_images.txt`修改测试图片路径

执行`prebuild.sh`安装依赖和下载模型

yolo目录下执行
```shell
$ cd apps/trt-yolo
$ mkdir build && cd build
$ cmake -D CMAKE_BUILD_TYPE=Release .. 
$ make && cp trt-yolo-app ../../..
$ cd ../../../
$ trt-yolo-app --flagfile=config/yolov3-tiny.txt

```

测试输出
```
Building complete!
Serializing the TensorRT Engine...
Serialized plan file cached at location : data/yolov3-kFLOAT-kGPU-batch4.engine
Loading TRT Engine...
UNKNOWN: Deserialize required 1818080 microseconds.
Loading Complete!
Total number of images used for inference : 6
[======================================================================] 100 %
Network Type : yolov3 Precision : kFLOAT Batch Size : 4 Inference time per image : 75.239 ms
```

# 备份恢复docker镜像

查看本地镜像
```
docker images
```

备份

```
docker save -o ~/container-backup.tar container-backup
```

恢复
```
docker load -i ~/container-backup.tar
```
