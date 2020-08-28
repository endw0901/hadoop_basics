# test1
https://www.udemy.com/course/complete-cca-175-hadoop-spark-developer-with-practice-test/learn/quiz

## q2

```
val cusdf = spark.read.format("csv").load("/user/testdata/t1q2_customer.csv")
cusdf.printSchema()
val cusdf2 = cusdf.select(col("_c0").as("customer_id"),col("_c1").as("customer_fname"))

val orddf1 = spark.read.format("csv").load("/user/testdata/t1q2_order.csv").select(col("_c2").as("customer_id"),col("_c3").as("status"))
val orddf2 = orddf1.filter($"status"==="COMPLETE")
val orddf3 = orddf2.groupBy("customer_id").count.where("count > 4")
val orddf4 = orddf3.join(cusdf2,"customer_id").sort("count")

// q2 output フォルダがないと作成してくれる
val orddf5 = orddf4.write.mode("overwrite").json("/user/output")

// spark-shell上でコマンドライン :vmローカル
:sh ls

// spark-shell上でコマンドライン :hdfs
:sh hdfs dfs -ls /user
resxxx.lines foreach println

```

## q3
- createOrReplaceTempView
https://intellipaat.com/community/12213/how-does-createorreplacetempview-work-in-spark

```
// data upload

//
val pricedf = spark.read.format("csv").option("header","true").option("inferSchema","true").load("/user/testdata/t1q3_price.csv")
pricedf.createOrReplaceTempView("product")
spark.table("product").count

val maxdf1 = spark.sql("select CONCAT(category,'|',max(price) AS maxprice) from product group by category order by max(price) desc")

maxdf1.coalesce(1).write.mode("overwrite").option("compression","gzip").format("text").save("/user/output")

// output確認
:sh hdfs dfs -ls /user/output
resxx.lines foreach println

:sh hdfs dfs -cat /user/output/part-00000-9112568a-8bcc-4a88-86c6-52fc355dc5ad-c000.txt.gz

```


## q4

- できない・・・

```
val schema = Seq("cusid","city")
val citydata4 = spark.read.option("sep","\t").csv("/user/testdata/t1q4_tab.csv").toDF(newColumns:_*)

val citydata4 = spark.read.format("csv").option("header","true").option("sep","\t").option("inferSchema","true").load("/user/testdata/t1q4_tab.csv")



citydata4.createOrReplaceTempView("cityview")
val citydf = spark.sql("select userid, city from cityview where city = 'Caguas'")

citydf.write.mode("overwrite").option("compression","deflate").format("csv").save("/user/output")
```

## q5

- avroの代わりにcsvを読む

```
val datadf = spark.read.format("csv").option("header","true").load("/user/testdata/t1q2_customer.csv")

// withColumn 新しいfield作成(同名の場合置き換え)
val datadf2 = datadf.withColumn("customer_fname",substring(col("customer_fname"),0,3))

// list:xをタブ区切りのデータに置き換え
val datadf3 = datadf2.map(x => x.mkString("\t"))

datadf3.write.mode("overwrite").option("compression","bzip2").format("text").save("/user/output")
```

## q6

- parquetの代わりにcsvを読む

```
val datadf1 = spark.read.format("csv").option("header","true").option("inferSchema","true").load("/user/testdata/t1q3_price.csv")
datadf1.createOrReplaceTempView("priceview")
val datadf2 = spark.sql("select category, price from priceview where price > 100")

datadf2.write.mode("overwrite").option("compression","uncompressed").parquet("/user/output/")

```
