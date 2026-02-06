# 📘 PySpark Syntax Bible (Interview Ready)

This file contains **PySpark syntax patterns only**.
Use it for **revision, interviews, and quick recall**.

---

## 1️⃣ Spark Session

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("app") \
    .getOrCreate()
2️⃣ Read Files
CSV
spark.read.csv(path, header=True, inferSchema=True)

JSON
spark.read.json(path)

Parquet
spark.read.parquet(path)

ORC
spark.read.orc(path)

Delta
spark.read.format("delta").load(path)

3️⃣ Write Files
CSV
df.write.mode("overwrite").option("header",True).csv(path)

Parquet
df.write.mode("overwrite").parquet(path)

Delta
df.write.format("delta").mode("overwrite").save(path)

4️⃣ Basic DataFrame Operations
df.show()
df.printSchema()
df.count()
df.columns
df.describe().show()

5️⃣ Select / Filter / WithColumn
from pyspark.sql.functions import col

df.select("col1","col2")
df.filter(col("age") > 30)
df.withColumn("new_col", col("a") + col("b"))
df.drop("col")
df.withColumnRenamed("old","new")

6️⃣ Transformations
df.distinct()
df.dropDuplicates()
df.dropDuplicates(["id"])
df.sort("col")
df.orderBy(col("col").desc())

7️⃣ Aggregations
df.groupBy("col").count()
df.groupBy("col").agg({"amount":"sum"})
df.groupBy("col").sum("amount")
df.groupBy("col").avg("amount")

8️⃣ Joins
df1.join(df2, "id", "inner")
df1.join(df2, "id", "left")
df1.join(df2, "id", "right")
df1.join(df2, "id", "full")
df1.join(df2, "id", "left_semi")
df1.join(df2, "id", "left_anti")

9️⃣ Broadcast Join
from pyspark.sql.functions import broadcast

df1.join(broadcast(df2), "id")

🔟 map vs flatMap (RDD)
rdd.map(lambda x: x*2)
rdd.flatMap(lambda x: x.split(","))

1️⃣1️⃣ reduceByKey vs groupByKey
rdd.reduceByKey(lambda a,b: a+b)
rdd.groupByKey().mapValues(sum)

1️⃣2️⃣ Narrow vs Wide (Examples)
df.map()
df.filter()

df.groupBy()
df.join()

1️⃣3️⃣ Cache & Persist
df.cache()
df.persist()
df.unpersist()

1️⃣4️⃣ Accumulators
acc = spark.sparkContext.accumulator(0)
rdd.foreach(lambda x: acc.add(1))

1️⃣5️⃣ Broadcast Variables
bc = spark.sparkContext.broadcast(data)
bc.value

1️⃣6️⃣ Window Functions
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number

w = Window.partitionBy("id").orderBy("date")
df.withColumn("rn", row_number().over(w))

1️⃣7️⃣ UDF
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

my_udf = udf(lambda x: x.upper(), StringType())
df.withColumn("new", my_udf(col("name")))

1️⃣8️⃣ Handling Nulls
df.fillna(0)
df.dropna()
df.dropna(subset=["col"])

1️⃣9️⃣ Date Functions
from pyspark.sql.functions import current_date, current_timestamp

df.withColumn("today", current_date())
df.withColumn("now", current_timestamp())

2️⃣0️⃣ Read Without Schema (Infer)
spark.read.option("inferSchema","true").csv(path)

2️⃣1️⃣ Convert RDD ↔ DataFrame
df = rdd.toDF()
rdd = df.rdd

2️⃣2️⃣ Temp View & Global Temp View
df.createOrReplaceTempView("view")
df.createOrReplaceGlobalTempView("gview")

spark.sql("select * from view")
spark.sql("select * from global_temp.gview")

2️⃣3️⃣ Delta Lake Operations
UPDATE
from delta.tables import DeltaTable

dt = DeltaTable.forPath(spark, path)
dt.update("id=1", {"amount":"100"})

DELETE
dt.delete("status='Cancelled'")

MERGE
dt.alias("t").merge(
    source.alias("s"),
    "t.id=s.id"
).whenMatchedUpdateAll() \
 .whenNotMatchedInsertAll() \
 .execute()

2️⃣4️⃣ Delta Time Travel
spark.read.format("delta").option("versionAsOf", 1).load(path)
spark.read.format("delta").option("timestampAsOf","2024-01-01").load(path)

2️⃣5️⃣ OPTIMIZE & VACUUM (SQL)
OPTIMIZE delta.`path`
ZORDER BY (col)

VACUUM delta.`path` RETAIN 168 HOURS

2️⃣6️⃣ Streaming (Autoloader)
spark.readStream \
 .format("cloudFiles") \
 .option("cloudFiles.format","json") \
 .load(path)

Write Stream
df.writeStream \
 .format("delta") \
 .option("checkpointLocation", chk) \
 .start(path)

2️⃣7️⃣ Checkpointing
spark.sparkContext.setCheckpointDir("/tmp/chk")
df.checkpoint()

2️⃣8️⃣ Repartition vs Coalesce
df.repartition(10)
df.coalesce(5)

2️⃣9️⃣ Spark Submit Parameters
--executor-memory
--executor-cores
--num-executors
--driver-memory
--conf

3️⃣0️⃣ Explain Plan
df.explain()
df.explain(True)

3️⃣1️⃣ Partitioning
df.write.partitionBy("date").parquet(path)

3️⃣2️⃣ Skew Handling (Salting)
df.withColumn("salt", rand())

3️⃣3️⃣ Stop Spark
spark.stop()

# 📘 PySpark Syntax Bible (100% Complete – Databricks & Interviews)

This document contains **all important PySpark syntaxes** used in:
- Databricks
- Data Engineering
- Spark interviews
- Real-world pipelines

---

## 1️⃣ Spark Session & Context

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MyApp") \
    .enableHiveSupport() \
    .getOrCreate()

sc = spark.sparkContext
2️⃣ Reading Files (ALL WAYS)
CSV
spark.read.csv(path)
spark.read.option("header",True).csv(path)
spark.read.option("inferSchema",True).csv(path)
spark.read.schema(schema).csv(path)
JSON
spark.read.json(path)
spark.read.option("multiLine",True).json(path)
Parquet
spark.read.parquet(path)
ORC
spark.read.orc(path)
Avro
spark.read.format("avro").load(path)
Delta
spark.read.format("delta").load(path)
Text
spark.read.text(path)
3️⃣ Writing Files
df.write.csv(path)
df.write.json(path)
df.write.parquet(path)
df.write.orc(path)
df.write.format("delta").save(path)
Modes
.mode("overwrite")
.mode("append")
.mode("ignore")
.mode("error")
4️⃣ Schema Definition
from pyspark.sql.types import *

schema = StructType([
    StructField("id", IntegerType(), True),
    StructField("name", StringType(), True)
])

spark.read.schema(schema).csv(path)
5️⃣ Basic DataFrame Operations
df.show()
df.show(5)
df.printSchema()
df.count()
df.columns
df.dtypes
df.describe().show()
6️⃣ Select, Filter, Columns
from pyspark.sql.functions import col

df.select("col1","col2")
df.select(col("col1").alias("c1"))
df.filter(col("age") > 30)
df.where("age > 30")
7️⃣ Column Operations
df.withColumn("new", col("a") + col("b"))
df.withColumnRenamed("old","new")
df.drop("col")
df.cast("int")
8️⃣ Conditional Logic
from pyspark.sql.functions import when

df.withColumn(
    "status",
    when(col("age") > 18, "Adult").otherwise("Minor")
)
9️⃣ String Functions
from pyspark.sql.functions import *

upper(col("name"))
lower(col("name"))
length(col("name"))
concat(col("a"),col("b"))
substring(col("name"),1,3)
🔟 Date & Timestamp Functions
current_date()
current_timestamp()
to_date(col("dt"))
datediff(col("d1"), col("d2"))
add_months(col("d"),1)
1️⃣1️⃣ Null Handling
df.fillna(0)
df.fillna({"age":0,"name":"NA"})
df.dropna()
df.dropna(subset=["age"])
1️⃣2️⃣ Aggregations
df.groupBy("col").count()
df.groupBy("col").sum("amount")
df.groupBy("col").avg("amount")
df.groupBy("col").agg({"amount":"sum"})
1️⃣3️⃣ Sorting
df.sort("col")
df.orderBy(col("col").desc())
1️⃣4️⃣ Joins (ALL TYPES)
df1.join(df2,"id","inner")
df1.join(df2,"id","left")
df1.join(df2,"id","right")
df1.join(df2,"id","full")
df1.join(df2,"id","left_semi")
df1.join(df2,"id","left_anti")
1️⃣5️⃣ Broadcast Join
from pyspark.sql.functions import broadcast
df1.join(broadcast(df2),"id")
1️⃣6️⃣ Window Functions (ALL)
from pyspark.sql.window import Window
from pyspark.sql.functions import *

w = Window.partitionBy("id").orderBy("date")

row_number().over(w)
rank().over(w)
dense_rank().over(w)
lag("amount").over(w)
lead("amount").over(w)
1️⃣7️⃣ Array & Struct Functions
explode(col("arr"))
size(col("arr"))
array_contains(col("arr"),"x")
col("struct.field")
1️⃣8️⃣ JSON Functions
from pyspark.sql.functions import *

get_json_object(col("json"), "$.field")
from_json(col("json"), schema)
to_json(col("struct"))
1️⃣9️⃣ RDD Operations
rdd = df.rdd
df = rdd.toDF()

rdd.map()
rdd.flatMap()
rdd.filter()
rdd.reduceByKey()
rdd.groupByKey()
2️⃣0️⃣ Cache & Persist
df.cache()
df.persist()
df.unpersist()
2️⃣1️⃣ Accumulators & Broadcast
acc = sc.accumulator(0)
acc.add(1)

bc = sc.broadcast(data)
bc.value
2️⃣2️⃣ Repartition & Coalesce
df.repartition(10)
df.coalesce(5)
2️⃣3️⃣ Explain Plan
df.explain()
df.explain(True)
2️⃣4️⃣ Temp Views
df.createOrReplaceTempView("view")
df.createOrReplaceGlobalTempView("gview")

spark.sql("select * from view")
spark.sql("select * from global_temp.gview")
2️⃣5️⃣ Delta Lake Operations
UPDATE
from delta.tables import DeltaTable

dt = DeltaTable.forPath(spark, path)
dt.update("id=1", {"amount":"100"})
DELETE
dt.delete("status='Cancelled'")
MERGE
dt.alias("t").merge(
    src.alias("s"),
    "t.id=s.id"
).whenMatchedUpdateAll() \
 .whenNotMatchedInsertAll() \
 .execute()
2️⃣6️⃣ Delta Time Travel
spark.read.format("delta").option("versionAsOf",1).load(path)
spark.read.format("delta").option("timestampAsOf","2024-01-01").load(path)
2️⃣7️⃣ OPTIMIZE & VACUUM
OPTIMIZE delta.`path`
ZORDER BY (col)

VACUUM delta.`path` RETAIN 168 HOURS
2️⃣8️⃣ Streaming & Autoloader
spark.readStream \
 .format("cloudFiles") \
 .option("cloudFiles.format","json") \
 .load(path)
df.writeStream \
 .format("delta") \
 .option("checkpointLocation", chk) \
 .start(path)
2️⃣9️⃣ Checkpointing
sc.setCheckpointDir("/tmp/chk")
df.checkpoint()
3️⃣0️⃣ AQE & Spark Configs
spark.conf.set("spark.sql.adaptive.enabled","true")
spark.conf.set("spark.sql.shuffle.partitions","200")
3️⃣1️⃣ Partitioning on Write
df.write.partitionBy("date").parquet(path)
3️⃣2️⃣ File Utilities (Databricks)
dbutils.fs.ls(path)
dbutils.fs.rm(path, recurse=True)
dbutils.fs.mkdirs(path)
3️⃣3️⃣ Stop Spark
spark.stop()
