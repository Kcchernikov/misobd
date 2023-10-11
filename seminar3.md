# Часть 1 (1 файл размера 1G)

Создаём файл из букв 'a’ размером 1G: `perl -e 'print "a" x 1024 x 1024 x 1024' > 1gfile`

Копируем его с помощью scp

Смотрим на ресурсы nn и nd до перемещения:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/8a7fb37f-bc95-459a-9d7a-f7ce0b239268">

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/2b133c11-58c2-49b9-9745-93dc6917c17b">

Также посмотрим на потребление cpu и ram на одной ноде

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/6a35d1b4-1eac-4c8d-9f14-0db6949bc6da">

Кладём файл в hdfs (`hadoop fs -put 1gfile /`)

Во время операции на короткое время можно увидеть следующее потребление:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/4ac5bd7a-45db-4cd9-a9f5-278a231eb2e1">

Смотрим на ресурсы после операции:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/358ef6f1-a5f7-44f0-a32a-47152f0eb59a">

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/072dca97-3d9d-4baf-988b-468a22807050">

Поскольку репликация стоит 3, то потребление disk на каждой dn увеличился примерно на 1G

# 1000 файлов суммарного размера ~1G

Создаём 1000 файлов суммарным размером ~1G при помощи простого python script:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/c9d5de05-6946-44fd-9635-724fc8fdb561">

Файлы лежат в директории 1000files, копируем ее при помощи scp

Смотрим на ресурсы nn и nd до перемещения:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/7cd6bc18-2eea-4171-a56a-1fe95dbe8432">

<img width="421" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/87bab92f-8f29-4ab4-b6c9-2cf9796e7e13">

Также посмотрим на потребление cpu и ram на одной ноде

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/9ca0e5a1-aa51-454e-b610-03499d535773">

Кладём файл в hdfs (`hadoop fs -put 1000files /`)

Во время операции на короткое время можно увидеть следующее потребление:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/acf2c803-c03e-4bea-9dce-aecafb5c7820">

Можно отметить, что в случае с 1000 файлами операция занимает больше времени.

Смотрим на ресурсы после операции:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/6c33f486-7f68-4816-9916-b39826911025">

<img width="406" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/a1076344-550f-499e-a2d6-e4eb167de0b8">

Поскольку репликация стоит 3, то потребление disk на каждой dn увеличился примерно на 1G

Отличие от одного файла можно увидеть в числе блоков, которые потребовались

# Как изменился журнал изменений

В журнал изменений добавились записи о загрузке файлов. Выполнив `sudo ls -al /usr/local/hadoop/hadoopdata/hdfs/namenode/current/` можно получить следующее:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/260d1d88-33ee-4e79-8235-160d3fc6ccef">

Видно, что для каждого загруженного файла есть запись:

<img width="468" alt="image" src="https://github.com/Kcchernikov/team7/assets/80039707/5e4db425-b2a8-4f1d-b7c2-6215047b23d7">






