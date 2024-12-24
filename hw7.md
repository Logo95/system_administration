### Шаг 1: Подготовка ClickHouse на хосте

1. Установка ClickHouse на хосте:
     ```
     sudo apt-get update
     sudo apt-get install -y apt-transport-https ca-certificates
     sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv E0C56BD4
     echo "deb https://repo.clickhouse.com/deb/stable/ main/" | sudo tee /etc/apt/sources.list.d/clickhouse.list
     sudo apt-get update
     sudo apt-get install -y clickhouse-server clickhouse-client
     ```

2. Запускаем ClickHouse-сервер:
   ```
   sudo service clickhouse-server start
   ```
---

### Шаг 2: Настройка Docker-контейнеров для ClickHouse

1. Запускаем контейнер:
   ```
   docker run -d --name clickhouse_local -p 8124:8123 -v /hm7/:/var/lib/clickhouse yandex/clickhouse-server
   ```

---

### Шаг 3: Настройка конфигурации ClickHouse для репликации

#### Подготовливаем ZooKeeper (для репликации)

ClickHouse использует ZooKeeper для управления репликацией.

1. Запускаем ZooKeeper в Docker (локально или удалённо):
   ```
   docker run -d --name zookeeper -p 2181:2181 confluentinc/cp-zookeeper:latest
   ```

#### 3.2 Настройка конфигурации ClickHouse для ZooKeeper

1. Редактируем файл конфигурации ClickHouse (`/etc/clickhouse-server/config.xml`):
   ```
   <zookeeper>
       <node>
           <host>localhost</host>
           <port>2181</port>
       </node>
   </zookeeper>
   ```

2. Перезапускаем сервер ClickHouse:
   ```
   sudo service clickhouse-server restart
   ```

---

### Шаг 4: Создание реплицируемой таблицы

Создаём таблицы на всех инстансах ClickHouse.

1. На мастере (хосте):
   ```
   CREATE TABLE test_table (
       id UInt64,
       value String
   ) ENGINE = ReplicatedMergeTree('/clickhouse/tables/test_table', 'replica_1')
   ORDER BY id;
   ```

2. На локальном контейнере:
   ```
   CREATE TABLE test_table (
       id UInt64,
       value String
   ) ENGINE = ReplicatedMergeTree('/clickhouse/tables/test_table', 'replica_2')
   ORDER BY id;
   ```

#### Шаг 4.1: Создание общей таблицы
Создаём `Distributed` таблицу для объединения данных:
```
CREATE TABLE distributed_table AS test_table
ENGINE = Distributed('cluster_name', 'default', 'test_table', rand());
```

---

### Шаг 5: Проверка репликации

1. Вставьте данные на мастере:
   ```sql
   INSERT INTO test_table VALUES (1, 'value1'), (2, 'value2');
   ```
