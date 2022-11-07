## spark 读取tfrecord
```scala
val hdfsPath="hdfs://xx/2022-07-06/03"
val tfrecordDF = spark.read.format("tfrecords").option("recordType", "Example").load(hdfsPath).repartition(1000)
print(tfrecordDF.columns)
```




