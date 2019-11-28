---
title: yolo训练经验
date: 2018-09-17 19:25:51
tags: yolo
categories: 深度学习
---
yolo太难训练了啊，动不动就nan，超参选择对模型是否收敛影响挺大的，本文分享我在训练yolo的一些经验

<!-- more -->

## YOLOv3为例

先说yolov3首先要有张8GB以上的显卡，否则很难收敛。tiny-yolov3建议4G以上显存。

将batch设为64，subdivisions设为16，真实的batchsize为batch/subdivisions=4。显存主要是和真实的batchsize挂钩，设置大了要担心显存是否够用，但训练初期能够很好的收敛。

learning_rate设置过大训练会很快发散，在fine_tune模型时learning_rate为配置文件所设置的值，不是fine_tune模型时learning_rate为最大的学习率，训练过程真实的学习率会慢慢变大直到等于所设值。在训练一段时间后，loss值在很长的一段时间都在某个值附近徘徊，需要将learning_rate设置更小的值。总结learning_rate的值应该先增大再减小，可以通过steps和scales配置比率。

训练观察是否收敛，loss很快变小再在某个值附近徘徊；obj先变小后变大最后趋近1；召回率.5R会变大直到1，接着.75R也会有很多1；class和IOU会慢慢接近1。

训练测试
```
.\darknet detector map .\data\voc.data .\cfg\yolov3.cfg .\backup\yolov3_271604.weights
```
结果包含每个类别的AP值，mAP值、精度、召回率、TP数量、FP数量、FN数量、平均IOU等。

模型输入图像尺寸和mask、anchors参数要对应，根据训练数据实际框大小的分布来确定值。在训练时候不会在某个尺度的框一直出现count:0的结果，才能最大化利用其尺度信息。

如果觉得三个scale不够用，可以将最后增加一个尺度，模仿原始模型文件写即可，注意合并要用第11层。mask、anchors参数也要添加更多的数据，9->12。


声明：如有错误或者侵权请邮箱联系我