<h1 align="center">Spark Basics</h1>


## Description

In this lesson we will describe how Spark work. We will build our own distributed system.

## Pre-requirements

Spark, Hadoop, Java JDK.

## Теория о кластере

Для того чтобы можно было приступить к оптимизациям и утсройству самого Spark внутри, необходимо для начала ознакомиться с 
концепциями распределенной вычислительной системы, которой Spark и является.
По сути в устройстве системы лежит 6 понятий:

- Executor
- Worker
- Driver
- Master
- Cluster Manager
- Cluster 

Вот пример всей системы на картинке ниже.

<p align="center">
<img src="https://miro.medium.com/max/3334/1*9rdJjMwXXaBXDddxRLPFlw.jpeg" width="80%"></p>

Обо всём по порядку.

### Cluster

Кластером называют все вычислительные средства(будь то сервера или просто отдельно стоящие компьютеры), входящие в систему. 
A cluster is a set of tightly or loosely coupled computers connected through LAN (Local Area Network). The computers in the cluster are usually called nodes. 
Each node in the cluster can have a separate hardware and Operating System or can share the same among them. Resource (Node) management and task execution in the nodes 
is controlled by a software called Cluster Manager.

### Worker

Компьютер (сервер) ресурсы которого будут использоваться для работы Spark. Например в том же databricks при создании кластера вы видете выбор worker-node и после выбора
можно установить их количество. По сути машины объединенные по сети для совместной работы.

### Executor

Executor это ничто иное как процесс ранящийся в JVM(Java Virtual Machine). На каждом worker может быть несколько executors, о том как выбрать количество будет рассмотренно ниже.
Сам worker как вы понимаете ничего не делает, делают всю работу executors которые испольуют ресурсы worker. P.s. По сути worker железяка с ПО, а executor процесс выполняющий работу 
используя ресурсы железяки.

### Master

Машина с которой отправляется код на выполнение. По сути место где вы пишите код, ничего более.

### Driver

Driver это тоже процесс, который в зависимости от типа запуска кода может быть как на машине Master(client mode), так и на одной из worker-node(cluster mode).
Очевидно что продакшен cluster mode, ибо driver постоянно общается с executors, и если вы сидите в Минске а сервер в Москве то задержка будет больше чем время выполнения.

В driver живет main() метод, который и создаёт spark-context(Spark-session начиная с dataframe API), который и является логическим центром Spark. В его обязанности входит:

- Нарезает код на  job, stage, task(о том что это чуть ниже) 
- Создает Logical Plan, Physical Plan и т.д.(тоже чуть ниже объяснение)
- Координирует с cluster manager для отправки задач на executors для их выполнения 
- Отслеживает прогресс выполнения(что, где и на каком этапе ранится)

### Cluster manager

Существует несколько видов Cluster Manager:

- Spark Standalone Cluster Manager. Самый обычный и поставляется с самим спарком. Не требует настройки))
- Apache Mesos. Чуть сложнее и круче, но если предыдущий используется в тестовых целях, то этот я вообще не видел чтобы юзался.
- Hadoop YARN. До прихода на рынок k8s был одним из лучших решений, однако из-за особенностей устройства сейчас уже тоже почти не юзается.
- k8s. Самый лучший и чаще всего будет он.

Вообще Cluster Manager отвечает за выделяемые ресурсы. Если мы выполняем задачу в cluster режиме, то сначала это ПО(Помните cluster manager это ПО) поднимает driver, driver говорит
мол надо столько executors с такими-то ресурсами и кидает запрос на cluster manager, тот же идёт и поднимает всё что нужно. В Client режиме driver поднимается сам,
но вот executors всё ещё лежат на плечах cluster manager.

Вот красивый пример как это всё работает:

Working Process

spark-submit –master <Spark master URL> –executor-memory 2g –executor-cores 4 WordCount-assembly-1.0.jar

 
1) Let’s say a user submits a job using “spark-submit”.
2) “spark-submit” will in-turn launch the Driver which will execute the main() method of our code.
3) Driver contacts the cluster manager and requests for resources to launch the Executors.
4) The cluster manager launches the Executors on behalf of the Driver.
5) Once the Executors are launched, they establish a direct connection with the Driver.
6) he driver determines the total number of Tasks by checking the Lineage.
7) The driver creates the Logical and Physical Plan.
8) Once the Physical Plan is generated, Spark allocates the Tasks to the Executors.
9) Task runs on Executor and each Task upon completion returns the result to the Driver.
10) Finally, when all Task is completed, the main() method running in the Driver exits, i.e. main() method invokes sparkContext.stop().
11) Finally, Spark releases all the resources from the Cluster Manager.

## Практика кластер

Сейчас мы создадим свой кластер на своей машине.

Заходим в cmd(если конечно вы настроили Spark как в моём гайде) и прям там пишем 
```
spark-class org.apache.spark.deploy.master.Master
```
.
