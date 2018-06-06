---
title: 深度学习常用标签转换
date: 2018-06-06 21:25:51
tags: 标签转换
categories: 深度学习
---
### 说明

本文主要是对tencent 100k交通标志标记数据转换

临时写的，代码很乱，需要根据实际路径修改

数据下载： http://cg.cs.tsinghua.edu.cn/traffic-sign/tutorial.html

### 加载标签
```python
import json
import pylab as pl
import random
import numpy as np
import cv2
import anno_func
%matplotlib inline

datadir = "../../data/"

filedir = datadir + "/annotations.json"
ids = open(datadir + "/test/ids.txt").read().splitlines()

annos = json.loads(open(filedir).read())
```
### 分析结构
```python
imgid = random.sample(ids, 1)[0]
img=annos["imgs"][imgid]
print (img)
img=annos["types"]
print (img)
```
```
{u'path': u'test/2468.jpg', u'objects': [{u'category': u'pn', u'bbox': {u'xmin': 1343.2, u'ymin': 911.2, u'ymax': 964.0, u'xmax': 1396.0}, u'ellipse_org': [[1367.87, 911.552], [1395.46, 937.913], [1368.33, 963.195], [1343.97, 936.988], [1353.99, 958.571], [1387.91, 957.183]], u'ellipse': [[1369.0028076171875, 937.2377319335938], [51.76936721801758, 52.934967041015625], 139.54888916015625]}, {u'category': u'pn', u'bbox': {u'xmin': 1197.6, u'ymin': 940.0, u'ymax': 963.2, u'xmax': 1220.8}, u'ellipse_org': [[1209.01, 940.921], [1219.11, 951.016], [1209.21, 961.043], [1199.25, 951.626], [1201.96, 944.986], [1201.22, 956.775], [1217.55, 945.799]], u'ellipse': [[1209.0211181640625, 950.5414428710938], [19.53607940673828, 21.289642333984375], 153.71450805664062]}, {u'category': u'p11', u'bbox': {u'xmin': 229.6, u'ymin': 963.2, u'ymax': 1008.8000000000001, u'xmax': 270.4}, u'ellipse_org': [[248.171, 963.658], [269.073, 986.158], [249.236, 1008.39], [231.395, 984.827], [262.816, 1002.0], [236.987, 1001.87], [263.881, 971.513]], u'ellipse': [[249.62039184570312, 985.5843505859375], [37.6942024230957, 44.657630920410156], 177.4745330810547]}, {u'category': u'pl10', u'bbox': {u'xmin': 235.2, u'ymin': 1009.6, u'ymax': 1056.8, u'xmax': 279.2}, u'ellipse_org': [[254.683, 1009.92], [277.41, 1032.51], [255.785, 1055.1], [235.813, 1031.96], [240.496, 1047.8], [239.945, 1018.6], [270.386, 1047.11], [271.488, 1017.49]], u'ellipse': [[255.57847595214844, 1031.8031005859375], [41.567535400390625, 45.633445739746094], 3.8224732875823975]}], u'id': 2468}
[u'i1', u'i10', u'i11', u'i12', u'i13', u'i14', u'i15', u'i2', u'i3', u'i4', u'i5', u'il100', u'il110', u'il50', u'il60', u'il70', u'il80', u'il90', u'io', u'ip', u'p1', u'p10', u'p11', u'p12', u'p13', u'p14', u'p15', u'p16', u'p17', u'p18', u'p19', u'p2', u'p20', u'p21', u'p22', u'p23', u'p24', u'p25', u'p26', u'p27', u'p28', u'p3', u'p4', u'p5', u'p6', u'p7', u'p8', u'p9', u'pa10', u'pa12', u'pa13', u'pa14', u'pa8', u'pb', u'pc', u'pg', u'ph1.5', u'ph2', u'ph2.1', u'ph2.2', u'ph2.4', u'ph2.5', u'ph2.8', u'ph2.9', u'ph3', u'ph3.2', u'ph3.5', u'ph3.8', u'ph4', u'ph4.2', u'ph4.3', u'ph4.5', u'ph4.8', u'ph5', u'ph5.3', u'ph5.5', u'pl10', u'pl100', u'pl110', u'pl120', u'pl15', u'pl20', u'pl25', u'pl30', u'pl35', u'pl40', u'pl5', u'pl50', u'pl60', u'pl65', u'pl70', u'pl80', u'pl90', u'pm10', u'pm13', u'pm15', u'pm1.5', u'pm2', u'pm20', u'pm25', u'pm30', u'pm35', u'pm40', u'pm46', u'pm5', u'pm50', u'pm55', u'pm8', u'pn', u'pne', u'po', u'pr10', u'pr100', u'pr20', u'pr30', u'pr40', u'pr45', u'pr50', u'pr60', u'pr70', u'pr80', u'ps', u'pw2', u'pw2.5', u'pw3', u'pw3.2', u'pw3.5', u'pw4', u'pw4.2', u'pw4.5', u'w1', u'w10', u'w12', u'w13', u'w16', u'w18', u'w20', u'w21', u'w22', u'w24', u'w28', u'w3', u'w30', u'w31', u'w32', u'w34', u'w35', u'w37', u'w38', u'w41', u'w42', u'w43', u'w44', u'w45', u'w46', u'w47', u'w48', u'w49', u'w5', u'w50', u'w55', u'w56', u'w57', u'w58', u'w59', u'w60', u'w62', u'w63', u'w66', u'w8', u'wo', u'i6', u'i7', u'i8', u'i9', u'ilx', u'p29', u'w29', u'w33', u'w36', u'w39', u'w4', u'w40', u'w51', u'w52', u'w53', u'w54', u'w6', u'w61', u'w64', u'w65', u'w67', u'w7', u'w9', u'pax', u'pd', u'pe', u'phx', u'plx', u'pmx', u'pnl', u'prx', u'pwx', u'w11', u'w14', u'w15', u'w17', u'w19', u'w2', u'w23', u'w25', u'w26', u'w27', u'pl0', u'pl4', u'pl3', u'pm2.5', u'ph4.4', u'pn40', u'ph3.3', u'ph2.6']
```

### 转换txt
```python
cnt=0
resultlistfile=open('test.txt','w')
idsfile=open('ids.txt','w')
for imgid in ids:
    if int(imgid)<40000:
        idsfile.write(imgid+'\n')
        img=annos["imgs"][imgid]
        cnt+=1
        for obj in img['objects']:
            box = obj['bbox']
            ss = obj['category']
            resstr=imgid+' '+ss+' '+str(box['xmin'])+' '+str(box['ymin'])+' '+str(box['xmax'])+' '+str(box['ymax'])+' 0 '+str(cnt)
            resultlistfile.write(resstr+'\n')
            print (resstr)
        
idsfile.close()
resultlistfile.close()
```
```

10056 pne 414 877 431 909 0 1
10056 i5 452 886 468 916 0 1
10056 pne 1215 928 1237 950 0 1
10056 i5 1274 927 1294 949 0 1
10056 pne 2016 910 2032 934 0 1
10063 pr20 1637 949 1678 1002 0 2
10063 p27 1371 701 1426 759 0 2
10063 po 1482 692 1543 753 0 2
10063 pl40 1370 774 1422 834 0 2
10063 ph4.5 1480 765 1543 828 0 2
10128 pl40 1349 539 1450 643 0 3
10128 p11 1476 545 1570 650 0 3
10128 pn 1584 562 1684 662 0 3
1012 pl30 645 516 735 603 0 4
1012 pm20 742 512 837 600 0 4
1012 p26 843 487 938 572 0 4
1012 ph4.5 531 752 618 840 0 4
1012 p9 529 847 621 941 0 4
```
### 转换json
```python
resultcont=open('test.txt','r')
#print('namelist',resultcont)
perid=''
imgid=''
# types=["1","2","3","4","5","6","7"]
# trainotest='train'
objs=[]
results = {}
for resultline in resultcont.read().split('\n'):
    if resultline:
        ressplit=resultline.split(' ')
        if (not perid==ressplit[0]):
            perid=ressplit[0]
            
            if (not imgid == ''):
                #print(objs)
                results[imgid] = {"objects":objs,"id":imgid,"path":""+trainotest+"/"+imgid+".jpg"}
            objs=[]
        mobj = {"bbox":dict(zip(["xmin","ymin","xmax","ymax"],
                                [float(ressplit[2]),float(ressplit[3]),float(ressplit[4]),float(ressplit[5])])), 
                    "category":str(ressplit[1])}
        objs.append(mobj)
        #print(ressplit[2:6])
        imgid=ressplit[0]
results[imgid] = {"objects":objs,"id":imgid,"path":""+trainotest+"/"+imgid+".jpg"}

results_annos = {"imgs":results,"types":types}   
print (results_annos)
open("../../Src1/annotations.json", "w").write(json.dumps(results_annos))
```
### json转voc
```python
import json
import pylab as pl
import random
import numpy as np
import cv2
import anno_func
%matplotlib inline

datadir = "../../Src1/"
trainotest='train'
filedir = datadir + "/annotations.json"
ids = open(datadir + "/"+trainotest+"/ids.txt").read().splitlines()

annos = json.loads(open(filedir).read())

cnt=0
resultlistfile=open('test.txt','w')
for imgid in ids:
    if int(imgid)<40000:
        img=annos["imgs"][imgid]
        cnt+=1
        node_root = Element('annotation')

        node_folder = SubElement(node_root, 'folder')
        node_folder.text = 'train'

        node_filename = SubElement(node_root, 'filename')
        node_filename.text = str(imgid)+'.jpg'
        
        node_filename = SubElement(node_root, 'path')
        node_filename.text = datadir+trainotest+"/"+str(imgid)+'.jpg'

        node_size = SubElement(node_root, 'size')
        node_width = SubElement(node_size, 'width')
        node_width.text = '2048'

        node_height = SubElement(node_size, 'height')
        node_height.text = '2048'

        node_depth = SubElement(node_size, 'depth')
        node_depth.text = '3'
        
        for obj in img['objects']:
            box = obj['bbox']
            ss = obj['category']
            str(box['xmin'])+' '+str(box['ymin'])+' '+str(box['xmax'])+' '+str(box['ymax'])+' 0 '+str(cnt)
            

            node_object = SubElement(node_root, 'object')
            node_name = SubElement(node_object, 'name')
            node_name.text = ss
            node_difficult = SubElement(node_object, 'difficult')
            node_difficult.text = '0'
            node_bndbox = SubElement(node_object, 'bndbox')
            node_xmin = SubElement(node_bndbox, 'xmin')
            node_xmin.text = str(int(box['xmin']))
            node_ymin = SubElement(node_bndbox, 'ymin')
            node_ymin.text = str(int(box['ymin']))
            node_xmax = SubElement(node_bndbox, 'xmax')
            node_xmax.text = str(int(box['xmax']))
            node_ymax = SubElement(node_bndbox, 'ymax')
            node_ymax.text = str(int(box['ymax']))

        xml = tostring(node_root, pretty_print=True)  #格式化显示，该换行的换行
        dom = parseString(xml)
            #print xml
        xmlfile=open(datadir+"xml/"+imgid+'.xml','w')
        xmlfile.write(xml)

```
