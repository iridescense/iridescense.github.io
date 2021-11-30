# GNN_Study

## Intro

- Node Degress

![image-20210829231953746](.\images\image-20210829231953746.png)

- Complete

![image-20210829232018201](.\images\image-20210829232018201.png)

## Tradition

### Node-level Tasks and Features

#### Node degree

![image-20210829233345012](.\images\image-20210829233345012.png)

#### Node centrality

**Engienvector centrality**:邻居多重要，我就有多重要(邻接矩阵特征向量表示)

![image-20210829233532486](.\images\image-20210829233532486.png)

**Betweenness centrality**: 重要的交通枢纽

![image-20210829233820621](.\images\image-20210829233820621.png)

**Closeness centrality**: 谁离所有点最近，谁就重要

![image-20210829234011101](.\images\image-20210829234011101.png)

#### Clustering coefficient

![image-20210829234106104](.\images\image-20210829234106104.png)

#### Graphlets

![image-20210829234343148](.\images\image-20210829234343148.png)

#### Summary

![image-20210829234420882](.\images\image-20210829234420882.png)

![image-20210829234455977](.\images\image-20210829234455977.png)

![image-20210829234514010](.\images\image-20210829234514010.png)

### Edge level

#### Distance-based feature

我们之间的最短距离

![image-20210829234902244](.\images\image-20210829234902244.png)

#### Local neighborhood overlap

大家都有的邻居

![image-20210829234945618](.\images\image-20210829234945618.png)

#### Global neighborhood overlap

在使用邻接矩阵的时候，给定特点给的K，将矩阵进行K次幂，得到的矩阵则代表各点之间经过多少次边能到达，beta为小数常数，K越高，权重越小

![image-20210829235114350](C:\Users\86153\AppData\Roaming\Typora\typora-user-images\image-20210829235114350.png

### Summary

![image-20210829235417686](.\images\image-20210829235417686.png)

### Graph

#### Graphlet Kernel

![image-20210830225725534](.\images\image-20210830225725534.png)

![image-20210830225110868](.\images\image-20210830225110868.png)

![image-20210830225936609](C:\Users\86153\AppData\Roaming\Typora\typora-user-images\image-20210830225936609.png)

![image-20210830230031570](.\images\image-20210830230031570.png)![image-20210830230050811](.\images\image-20210830230050811.png)

![image-20210830230227976](.\images\image-20210830230227976.png)

#### Weisfeiler-Lehman Kernel

![image-20210830230301812](.\images\image-20210830230301812.png)

![image-20210830230356945](.\images\image-20210830230356945.png)

![image-20210830230443931](.\images\image-20210830230443931.png)

![image-20210830230620489](.\images\image-20210830230620489.png)

![image-20210830230639206](.\images\image-20210830230639206.png)

![image-20210830230700211](.\images\image-20210830230700211.png)

![image-20210830230730294](.\images\image-20210830230730294.png)

### Summary

![image-20210830230755946](.\images\image-20210830230755946.png)

## Nodeemb

### Node embedding

#### Encoder and Decoder

