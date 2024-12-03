# Настройка Jupyter Notebook для работы со Spark и Hadoop (на Linux/WSL)

> Перед установкой желательно иметь готовый docker compose Hadoop. Взять его можно [здесь](https://github.com/pyhedgehog/bde2020-docker-hadoop) (2 nodes, hdfs, hive, nginx)
> или [здесь](https://github.com/knight99rus/hadoop_full_pack) (hdfs, hive, hue, superset)

1) Проверяем установленный Docker `docker --version` и `docker compose version`

* Если не установлены, то выпоолняем по очереди следюущие команды:
```
sudo -s
apt install gnome-terminal
apt-get update
apt-get install ./docker-desktop-amd64.deb
systemctl --user start docker-desktop
```
2) 
