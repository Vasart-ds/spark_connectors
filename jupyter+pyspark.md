# Настройка Jupyter Notebook для работы со Spark и Hadoop (на Linux/WSL)

1) Проверяем установленный Docker `docker --version` и `docker compose version`

* Если не установлены, то выпоолняем по очереди следу.щие команды:
```
sudo -s
apt install gnome-terminal
apt-get update
apt-get install ./docker-desktop-amd64.deb
systemctl --user start docker-desktop
```
2) Скачиваем образ командой `docker pull jupyter/pyspark-notebook`

> Перед установкой желательно иметь готовый docker compose Hadoop. Взять его можно [здесь](https://github.com/pyhedgehog/bde2020-docker-hadoop) (2 nodes, hdfs, hive, nginx)
> или [здесь](https://github.com/knight99rus/hadoop_full_pack) (hdfs, hive, hue, superset)

4) Запускаем контейнер `docker run -d -it --name sparkbook -p 8888:8888 -p 4040:4040 -v sparkvolume:/home/media jupyter/pyspark-notebook:latest`
5) Проверяем работу Jupyter по адресу `localhost:8888`. **ВАЖНО**: если Jupyter просит вас ввести Token ID, то выполняем следующее:
* Командой `docker logs sparkbook` открываем лог контейнера и смотрим на следующую строчку:

![token.jpg](https://github.com/Vasart-ds/spark_connectors/blob/master/data/token.jpg)

Переходим по активной ссылке через Ctrl+ЛКМ и получаем

![lab.jpg](https://github.com/Vasart-ds/spark_connectors/blob/master/data/lab.jpg)

Также проверяем работу `localhost:4040`. Там должен открыться Spark WebUI следующего вида:

![sparkGUI.jpg](https://github.com/Vasart-ds/spark_connectors/blob/master/data/sparkGUI.jpg)

Иногда бывает так, что WebUI не запускается. Переживать не надо, сам Spark запускается с контейнером и работает, проверить его можно через команды:

```
docker exec -it sparkbook bash
pyspark --version
```

Позднее напишу как пофиксить.

5) После проверки работы нашего контейнера объединяем его в сеть с `namenode` контейнеров Hadoop

![network.jpg](https://github.com/Vasart-ds/spark_connectors/blob/master/data/network.jpg)

6) Проверяем коннектор с HDFS:
![hdfs_connect.jpg](https://github.com/Vasart-ds/spark_connectors/blob/master/data/hdfs_connect.jpg)

Также можно посмотреть на локальную работу Spark внутри контейнера:

![selfspark.jgp](https://github.com/Vasart-ds/spark_connectors/blob/master/data/selfspark.jpg)

8) В конце работы обязательно останавливаем Spark командой `spark.stop()`
