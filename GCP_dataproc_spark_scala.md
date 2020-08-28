# GCP_dataproc_spark_scala

1.クラスター作成
- マシンタイプ n1-standard-8
- クラスタ上のデフォルトコンポーネント及び選択した・・・ウェブインターフェイスへのアクセス→有効
- イメージ→spark 2.4一番上
- クラスター作成

2.VM
- クラスター名クリック→VM→SSH

```
// hdfsのファイル確認
hdfs dfs -ls /user

// GCSFUSEインストール => yes/no聞かれたら Y
export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s`
echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
sudo apt-get install gcsfuse

// VMローカルにGoogle Storageマウントポイント用フォルダ作成
mkdir ~/spark-dataset

// google storageにデータアップロード用フォルダ作成　endw-spark-dataframe

// マウント： google storageとVMローカルフォルダをマウント
gcsfuse endw-spark-dataframe ~/spark-dataset

// VMローカルのフォルダにgoogle storageのフォルダ内容が表示されることを確認
cd ./spark-dataset
ls

// hdfsに作業用フォルダ作成
hdfs dfs -mkdir /user/testdata

// VMローカルフォルダ(google storageとマウント)の配下データ全部をhdfs作業フォルダにコピー(put)
hdfs dfs -put ~/spark-dataset/* /user/testdata

```

- 作業したいときはローカルでデータ作成→google storageにアップロード→マウントしたvmローカルからhdfsの作業フォルダにコピー
- hdfs上でデータを作成した場合、アウトプットフォルダに作成
- クラスタ削除するとデータが消えるため、storageから
