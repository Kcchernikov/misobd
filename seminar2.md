# Установить Hadoop (1 нода) записать туда файл, измерить пространство, которое будет использовано, написать инструкцию по установке

Инструкция: 

1.  Устанавливаем java: `apt install default-jdk`
2.	Скачиваем и разархивируем Hadoop в /usr/local/hadoop:
   
        wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz
        mkdir hadoop
        tar -zxf hadoop-*.tar.gz -C ./hadoop
        mv hadoop/ hadoop-3.3.1 /usr/local/hadoop

3.	Создаем для Hadoop отдельную группу и юзера:

        sudo addgroup hadoopgroup
        sudo adduser -ingroup hadoopgroup hduser

4.	Устанавливаем ssh и генерируем ключи (в hduser):
   
        sudo apt-get install ssh
        sudo systemctl enable ssh
        sudo systemctl start ssh
        ssh-keygen -t rsa -P “”
        ssh-copy-id -I /home/hduser/.ssh/id_rsa.pub localhost

5.	Задаем в качестве владельца каталога /usr/local/hadoop созданного пользователя:
   
        chown -R hduser:hadoopgroup /usr/local/hadoop
6.	Настраиваем переменные окружения в /etc/profile.d/hadoop.sh
<img width="485" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/c3ca800b-b9bc-49e1-b40c-749d2d61750a">

7.  В /usr/local/hadoop/etc/hadoop/hadoop-env.sh устанавливаем переменную JAVA_HOME
8.  Конфигурируем Hadoop: core-syte.xml
<img width="303" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/61776857-e078-40de-88df-900adbddc778">

9.  Конфигурируем Hadoop: hdfs-site.xml
<img width="481" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/d6a3ae1c-0cf3-4fa0-b8a9-d81192754766">

10.  Конфигурируем Hadoop: mapred-site.xml
<img width="299" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/cffdde6b-d672-4691-ab18-c2038e6e43b3">

11.  Конфигурируем Hadoop: yarn-site.xml
<img width="392" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/b9c60ed7-8409-4258-b9de-1a81b5663e9b">

12.  Создаем файловую систему: `hdfs namenode -format`
13.  Запускаем: start-dfs.sh start-yarn.sh

         start-dfs.sh
         start-yarn.sh
14.  Готово, теперь на http://77.105.185.153:9870 можно наблюдать:
<img width="485" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/aed480ed-7632-4abb-81d4-6fe33de49194">

Видно, что занято 24KB. Запишем файл hadoop-3.3.1.tar.g размером 578MB. Получим следующую картину:
<img width="485" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/ece29170-c714-4f9d-b9d8-9d8d207d5fb4">

В DFS он занимает 581 MB

# Сконфигурировать кластер из 3-х нод

Конфигугируем на еще 2х нодах Hadoop по аналогии, устанавливаем правильные конфигурации core-syte.xml, чтобы ноды могли объединится, устанавливаем в hdfs-site.xml репликацию 3. Помещаем в эту систему тот же файл. Получаем следующую картину.

<img width="485" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/4dda9738-eabb-453a-a0ce-ccde050a69be">

Теперь он занимает на кластере 1.7 GB (по 581 MB на каждой ноде)

<img width="485" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/2d3ec03d-afd6-4b47-973e-38e264f23470">

Это выглядит следующим образом, если зайти в fs на одной ноде:

<img width="485" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/d7922eee-ec3b-48ea-ace7-4831184ad92c">




