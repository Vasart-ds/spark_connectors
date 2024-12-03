# Подключения Spark

Spark обладает возможностью подключаться к различным СУБД и DataLake. Для этого он использует точку входа SparkSession со следующими атрибутами:
**.builder**
**.appName** - название приложения;
**.master** - определяет, какой кластер или режим работы будет использоваться для выполнения задач;
**.config** - устанавливает конфигурации подключения. Их может быть как одна, так и несколько;
**.getOrCreate()** - команда для инциализации.

Например, если мы хотим использовать Spark локально (**LocalMode**), где распределением данных будут заняты потоки процессора, то для запуска будет использоваться следующий entrypoint, то есть - точка входа:

```!pip install pyspark
from pyspark.sql import SparkSession
spark = SparkSession.builder
       .appName("LocalApp") \ 
       .master("local[*]") \ # [*] указывает на использование всех ядер. Если мы хотим задействовать не все ядра, то нужно указать их число - например, [2]
       .getOrCreate()```

Также мы используем `.master` в случаях, когда хотим подключиться к локальному кластеру Spark (**StandAloneMode**):

`spark = SparkSession.builder \
      .appName("StandaloneApp") \
      .master("spark://hostname:7077") \ # в качестве hostname можно указать как localhost, если Spark запущен из системы, либо IP контейнера, если кластер располагается в Docker
      .getOrCreate()`

Если же мы хотим подключиться к внешним СУБД, DataLake, кластерам или контейнерам, то нам атрибут `.master` указывать **не нужно**: Spark заранее предполагает, что подключаемая среда настроена корректно.

## Примеры подключения к внешним источникам
### 
