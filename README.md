# Как подключить Spark к HDFS, HIVE, S3, YARN, MESOS, KUBERNETES

Разделы:

* [Локальное развертывание](#Локально)
       
* [Подключение к внешним источникам(HDFS, HIVE, S3)](#Подключение-к-внешним-источникам)

* [Подключение к ресурсным менеджерам(YARN, MESOS, K8S)](#Подключение-к-ресурсным-менеджерам)

* [Проблемы и решения](#Проблемы-и-решения)

--------

Spark обладает возможностью подключаться к различным СУБД и DataLake. Для этого он использует точку входа SparkSession со следующими атрибутами:

* **.builder** - конструктор SparkSession;

* **.appName** - название приложения;

* **.master** - определяет, какой кластер или режим работы будет использоваться для выполнения задач;

* **.config** - устанавливает конфигурации подключения. Их может быть как одна, так и несколько;

* **.getOrCreate()** - команда для инциализации.

Для упрощения работы будет использоваться Jupyter Notebook + Pyspark. [Гайд по установке](https://github.com/Vasart-ds/spark_connectors/blob/master/jupyter%2Bpyspark.md)

## Локально
Например, если мы хотим использовать Spark локально (**LocalMode**), где распределением данных будут заняты потоки процессора, то для запуска будет использоваться следующий entrypoint, то есть - точка входа:

```
!pip install pyspark
from pyspark.sql import SparkSession
spark = SparkSession.builder
       .appName("LocalApp") \ 
       .master("local[*]") \ # [*] указывает на использование всех ядер. Если мы хотим задействовать не все ядра, то нужно указать их число - например, [2]
       .getOrCreate()
```

Также мы используем `.master` в случаях, когда хотим подключиться к локальному кластеру Spark (**StandAloneMode**):

```
spark = SparkSession.builder \
      .appName("StandaloneApp") \
      # в качестве hostname можно указать как localhost,
      # если Spark запущен из системы, так и IP контейнера, если кластер располагается в Docker
      .master("spark://hostname:7077") \
      .getOrCreate()
```

Если же мы хотим подключиться к внешним СУБД, DataLake, кластерам или контейнерам, то нам атрибут `.master` указывать **не нужно**: Spark заранее предполагает, что подключаемая среда настроена корректно.

## Подключение к внешним источникам
### HDFS
```
spark = SparkSession.builder \
    .appName("HDFSApp") \
    # аналогично Spark, необходимо указать localhost для подключения к HDFS на компьютере или же IP контейнера
    .config("spark.hadoop.fs.defaultFS", "hdfs://namenode:8020") \ 
    .getOrCreate()
```

### HIVE
```
spark = SparkSession.builder \
    .appName("HiveApp") \
    .config("spark.sql.warehouse.dir", "hdfs://path-to-warehouse") \ # hdfs://path-to-warehouse может выглядеть как hdfs://127.0.0.1:9870/path/to/file
    .enableHiveSupport() \ # обязательный атрибут подключения к HIVE - включение окружения HIVE
    .getOrCreate()
```

### S3
```
spark = SparkSession.builder \
       .appName("S3App") \
       .config("spark.hadoop.fs.s3a.access.key", "your-access-key") \
       .config("spark.hadoop.fs.s3a.secret.key", "your-secret-key") \
       .config("spark.hadoop.fs.s3a.endpoint", "s3.amazonaws.com") \
       .getOrCreate()
```

## Подключение к ресурсным менеджерам
### YARN
```
spark = SparkSession.builder \
       .appName("YARNApp") \
       .master("yarn") \
       .getOrCreate()
```

### MESOS (аналог YARN) 
```
spark = SparkSession.builder \
       .appName("MesosApp") \ 
       .master("mesos://hostname:5050") \
       .getOrCreate()
```

### Kubernetes
```
spark = SparkSession.builder \
       .appName("KubernetesApp") \
       .master("k8s://https://<KUBERNETES_MASTER>") \
       .getOrCreate()
```

------
## Проблемы и решения
### Ошибки окружения 
* Локально

Для решения проблемы подключения из локальной среды вам необходимо установить правильные переменные окружений.

1) Убедитесь, что у вас **локально** установлены: Java, Hadoop, Spark.
2) Далее открываем Jupyter Notebook и перед началом работы прописываем следующие команды:
```
import os

os.environ['SPARK_HOME'] = '/path/to/spark' # Укажите пути до ваших установок (например C:\users\home\spark)
os.environ['JAVA_HOME'] = '/path/to/java'
os.environ['HADOOP_HOME'] = '/path/to/hadoop'
```
* Jupyter в контейнере
1) Для подключения jupyter в контейнере воспользуйтесь следующей [инструкцией](https://github.com/Vasart-ds/spark_connectors/blob/master/jupyter%2Bpyspark.md)
