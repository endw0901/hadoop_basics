# test2
https://www.udemy.com/course/complete-cca-175-hadoop-spark-developer-with-practice-test/learn/quiz

## q1

- case classを使う

https://docs.scala-lang.org/ja/tour/unified-types.html


```
// case class 定義
case class Customer(custId:String,fname:String,lname:String,city:String)
case class Order(ordId:String,custId:String)
case class OrderItems(ordId:String,order_item_subtotal:Float)

// タブ区切りテキストを定義クラスに変換
val cusdf = spark.read.textFile("/user/testdata/t2q1_customer.txt").map(x => x.split("\t")).map(c => Customer(c(0),c(1),c(2),c(6)))
val orddf = spark.read.textFile("/user/testdata/t2q1_order.txt").map(x => x.split("\t")).map(o => Order(o(0),o(2)))
val itemdf = spark.read.textFile("/user/testdata/t2q1_item.txt").map(x => x.split("\t")).map(i => OrderItems(i(1),i(4).toFloat))

// 集計
val itemsum = itemdf.select("ordId","order_item_subtotal").groupBy($"ordId").agg(sum($"order_item_subtotal") as "order_amount")
val ordjoindf = orddf.join(itemsum,"ordId").where("order_amount > 200")
val ordcusdf = cusdf.join(ordjoindf,"custId").select("fname","lname","city","order_amount")

ordcusdf.write.mode("overwrite").option("header","true").csv("/user/output")


```

## q3

- case classを使う

https://docs.scala-lang.org/ja/tour/unified-types.html


```
// case class 定義
case class Customer(custId:String,fname:String,lname:String,city:String)
val cusdf2 = spark.read.textFile("/user/testdata/t2q1_customer.txt").map(x => x.split("\t")).map(c => Customer(c(0),c(1),c(2),c(6)))

spark.sqlContext.setConf("hive.exec.dynamic.partition","true")
spark.sqlContext.setConf("hive.exec.dynamic.partition.mode","nonstrict")

cusdf2.createOrReplaceTempView("cityview2")
val cusdf3 = spark.sql("select city, count(custId) from cityview2 groupby city")
cusdf3.write.mode("overwrite").format("hive").option("compression","gzip").save("/user/output")
```


```
spark.read.option("sep","|").csv("/user/xxxx").select(col("_c1"),col("_c2").as("state")).
where("fname like '%M%'").
groupBy("state").count.write.mode("overwrite").option("compression","gzip").option("fileFormat","parquet").format("hive").saveAsTable("customer_m")
```

## q4

```
spark.sql("select product_category_id, product_name, product_price, rank from product_ranked order by product_category_id, product_price").
write.mode("overwrite").format("text").option("sep","|").save(
```
