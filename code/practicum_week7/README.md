
<br />
<div align="center">

<h3 align="center">Spark Big Data</h3>

  <p align="center">
    Pengumpulan tugas Spark Big Data minggu 7
  </p>
</div>

## Memulai Spark
![image](https://user-images.githubusercontent.com/86558365/228115462-3cb62086-c4e5-4cb0-ba3b-e434b9aef702.png)
di sini saya menggunakan Spark versi 3.3.2 yang dijalankan di dalam container dalam docker

## Spark-shell
```sh
import sys.process._
val res = "ls /tmp" !
println(res)
```
![image](https://user-images.githubusercontent.com/86558365/228116243-4f9be6e9-c81d-4df7-94c5-ff262b245b68.png)
![image](https://user-images.githubusercontent.com/86558365/228116351-fbaa226e-3161-4ab1-8c36-520a2c154cdc.png)

## Memulai PySpark
### Code 1
```sh
pyspark
```
![image](https://user-images.githubusercontent.com/86558365/228116582-2777f2fb-2883-4e70-be92-23c73d14d45c.png)
```sh
myaccum = sc.accumulator(0)
myrdd = sc.parallelize(range(1,100))
myrdd.foreach(lambda value: myaccum.add(value))
print (myaccum.value)
```
![image](https://user-images.githubusercontent.com/86558365/228116918-bd1a58fa-a9f6-41e8-a17d-be89a178aa3f.png)

untuk menampilkan jobs yang telah dilakukan bisa dilihat di Spark Jobs
![image](https://user-images.githubusercontent.com/86558365/228117192-8132132a-40fe-4977-9da9-d3d423fa1cfc.png)

Penjelasan code
```sh
sc          : Sebuah SparkContext merepresentasikan koneksi               ke cluster Spark, dan dapat digunakan untuk                 membuat variabel RDD dan broadcast pada                     cluster tersebut.
accumulator : 
parallelize : 
lambda      : 
value       : 
```

## Tugas Praktikum
### Code 2
```sh
from pyspark import *
# sc = SparkContext()
sc = SparkContext.getOrCreate();

broadcastVar = sc.broadcast(list(range(1, 100)))
# broadcastVar.value
print (broadcastVar.value)
```
![image](https://user-images.githubusercontent.com/86558365/228117652-954954bd-9868-48bc-b5dd-0656e9afc907.png)

### Code 3
```sh
from pyspark import *
# sc = SparkContext()
sc = SparkContext.getOrCreate();

# Get the lines from the textfile, create 4 partitions
access_log = sc.textFile("code/practicum_week7/data/README.md", 4)

#Filter Lines with ERROR only
error_log = access_log.filter(lambda x: "Spark" in x)

# Cache error log in memory
cached_log = error_log.cache()

# Now perform an action -  count
print ("Total number of Spark records are %s" % (cached_log.count()))

# Now find the number of lines with 
print ("Number of product pages visited that have Spark is %s" % (cached_log.filter(lambda x: "product" in x).count()))
```
![image](https://user-images.githubusercontent.com/86558365/228118262-aa8971a7-ab64-43a1-a037-b2f7be7f13dd.png)

### Code 4
```sh
from pyspark import *
# sc = SparkContext()
sc = SparkContext.getOrCreate();

mylist = ["my", "pair", "rdd"]
myRDD = sc.parallelize(mylist)
myPairRDD = myRDD.map(lambda s: (s, len(s)))
# myPairRDD.collect()
# myPairRDD.keys().collect()
# myPairRDD.values().collect()
print (myPairRDD.collect())
print (myPairRDD.keys().collect())
print (myPairRDD.values().collect())
```
![image](https://user-images.githubusercontent.com/86558365/228118486-cc85b369-25da-4ac4-864f-752d87ae6b42.png)

### Code 5
```sh
from pyspark import *
# sc = SparkContext()
sc = SparkContext.getOrCreate();

mylist = ["my", "pair", "rdd"]
myRDD = sc.parallelize(mylist)
myPairRDD = myRDD.map(lambda s: (s, len(s)))
# myPairRDD.collect()
# myPairRDD.keys().collect()
# myPairRDD.values().collect()
print (myPairRDD.collect())
print (myPairRDD.keys().collect())
print (myPairRDD.values().collect())
```
![image](https://user-images.githubusercontent.com/86558365/228118657-89af497d-e1ab-42a3-a63a-cad4b2a15688.png)

### Code 6
```sh
from pyspark import *
# sc = SparkContext()
sc = SparkContext.getOrCreate();

from operator import add
lines = sc.textFile("code/practicum_week7/data/README.md")
counts = lines.flatMap(lambda x: x.split(' ')) \
              .map(lambda x: (x, 1)) \
              .reduceByKey(add)
output = counts.collect()
for (word, count) in output:
    print("%s: %i" % (word, count))
```
![image](https://user-images.githubusercontent.com/86558365/228118841-e4ef659b-bf45-4b0d-af8b-3c9fbfff74e9.png)
atau untuk lebih lengkapnya bisa dilihat pada teks berikut:



