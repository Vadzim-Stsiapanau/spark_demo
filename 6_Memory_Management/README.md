<h1 align="center">Memory management</h1>


## Description

Здесь вы познакомитесь с тем как менеджить память в спарке, ну и вообще как она устроена.


## Spark Memory

На самом деле, разработчики спарка очень крутые ребята и дефолтные конфиги которые они предоставляют для менеджмента памяти вполне себе крутые и их зачастую трогать
не надо. Тем не менее ситуация меняется, и вы даже можете найти пару статеек на хабре где люди рассказывали как добивались неимоверной производительности от своих 
кластеров в том числе и за счёт менеджмента памяти. Поэтому скажем так: для обывателя вещь бесполезная, но на собесе спросить могут, поэтому приступим.

Вообще рассказать лучше чем в этой статье я не смогу, поэтому удачи при прочтении: 
https://www.bigdataschool.ru/blog/jvm-spark-memory-types-and-configurations.html?ysclid=l8zpaz0ekr494721682.

Вот ещё одна статья, которая только про память в JVM(в ней чуть более подробно что хранится именно в сегментах памяти JVM):
https://medium.com/analytics-vidhya/apache-spark-memory-management-49682ded3d42.