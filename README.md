# Подключения Spark

Spark обладает возможностью подключаться к различным СУБД и DataLake. Для этого он использует точку входа SparkSession со следующими атрибутами:

**.builder**
**.appName** - название приложения;
**.master** - определяет, какой кластер или режим работы будет использоваться для выполнения задач;
**.config** - устанавливает конфигурации подключения. Их может быть как одна, так и несколько;
**.getOrCreate()** - команда для инциализации.

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

## Примеры подключения к внешним источникам
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
