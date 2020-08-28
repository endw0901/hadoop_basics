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

```
// data upload
:sh hdfs dfs -put ~/spark-dataset/t1q3_price.csv /user/testdata
:sh hdfs dfs -ls /user/testdata
resxx.lines foreach println
//

```
