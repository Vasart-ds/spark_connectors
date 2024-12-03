# Настройка Jupyter Notebook для работы со Spark и Hadoop (на Linux/WSL)

> Перед установкой желательно иметь готовый docker compose Hadoop. Взять его можно [здесь](https://github.com/pyhedgehog/bde2020-docker-hadoop) (2 nodes, hdfs, hive, nginx)
> или [здесь](https://github.com/knight99rus/hadoop_full_pack) (hdfs, hive, hue, superset)

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
3) Запускаем контейнер `docker run -d -it --name sparkbook -p 8888:8888 -p 4040:4040 -v sparkvolume:/home/media jupyter/pyspark-notebook:latest`
4) Проверяем работу контейнера: переходим на `localhost:4040`. Там должен открыться Spark WebUI следующего вида:

Так же проверяем работу Jupyter по адресу `localhost:8888`. **ВАЖНО**: если Jupyter просит вас ввести Token ID, то выполняем следующее:
* Командой `docker logs sparkbook` открываем лог контейнера и смотрим на следующую строчку:
![token.jpg](https://github.com/Vasart-ds/spark_connectors/blob/master/data/token.jpg)

Переходим по активной ссылке через Ctrl+ЛКМ и получаем 

5) 
