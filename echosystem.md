# Hadoop技術外観

https://www.udemy.com/course/the-ultimate-hands-on-hadoop-tame-your-big-data

- Hadoopエコシステム + 外部データベース + クエリエンジン

## ファイルシステム・管理システム
- HDFS・・・・ファイルシステム
- Apache Ambari・・・管理システム、システムブラウザ

### HDFSの上：繋ぎ役
- YARN・・・yet another resource negotiator
- MESOS
- HBASE

### YARNの上：処理の仕組み・処理ツール
- MapReduce・・・プロセスの仕組
- TEZ・・・DAG。基本使うと速くなる
- Spark・・・scala (python, java)。

### MapReduceの上：SQLで処理。裏側でMapReduce等を使う
- Pigs
- Hive

### HDFSの横：外との連携
- HBASE・・外部データベースとの接続
- Apache Storm・・・streaming

## ジョブ管理・クラスタ管理
- Oozie・・・スケジューリング・ジョブ管理
- Zookeeper・・・クラスターコーディネート

## HBASE、Stormの横(HDFSにデータ取り込み)
- sqoop
- flume
- kafka

## 外部DB
- mongoDB
- cassandra
- MySQL

## クエリエンジン
- Apache Drill
- HIVE
- Apache Phoenix
- Presto
- Apache Zeppelin・・・jupyter notebookみたいなもの


