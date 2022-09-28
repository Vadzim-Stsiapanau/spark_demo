<h1 align="center">Spark Basics</h1>


## Description

In this lesson we will describe how Spark work. We will build our own distributed system.

## Pre-requirements

Spark, Hadoop, Java JDK.

## Spark

Для того чтобы можно было приступить к оптимизациям и утсройству самого Spark внутри, необходимо для начала ознакомиться с 
концепциями распределенной вычислительной системы, которой Spark и является.
По сути в устройстве системы лежит 6 понятий:
- Executor
- Worker
- Driver
- Master
- Cluster Manager
- Cluster 

Обо всё по порядку.

### Cluster

Кластером называют все вычислительные средства(б), входящие в систему. 
Вот пример всей системы на картинке ниже.

<p align="center">
<img src="https://miro.medium.com/max/3334/1*9rdJjMwXXaBXDddxRLPFlw.jpeg" width="80%"></p>
