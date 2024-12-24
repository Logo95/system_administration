### **1. Изучите доступные образы в Docker Hub**

#### Команды:
1. Поиск образов: ```docker search ubuntu```  
   Список доступных образов в Docker Hub, включая их название, описание, количество звезд (рейтинг) и поддержку официальных образов.  
   Пример:  
    logo@Kali:~$ docker search ubuntu  
    | Name           | DESCRIPTION                                      | STARS |
    |----------------|--------------------------------------------------|-------|
    | ubuntu         | Ubuntu is a Debian-based linux operating sys...  | 17413 |
    | ubuntu/squid   | Squid is a caching proxy for the web...           | 102   |
    | ubuntu/nginx   | Nginx, a high-performance reverse proxy...        | 123   |
    | ubuntu/cortex  | Cortex provides storage for Prometheus...         | 4     |


2. Загрузка образа:  ```docker pull ubuntu```  
   Скачивание образа с Docker Hub.  
   Пример:  
   logo@Kali:~$ docker pull ubuntu  
   Using default tag: latest  
   latest: Pulling from library/ubuntu  
   de44b2655507a: Pull complete  
   Digest: sha256:...  
   Status: Downloaded newr image for ubuntu:latest  
   docker.io/library/kali:latest  

3. Проверка локальных образов: ```docker images```  
   Список загруженных образов с их идентификаторами, размерами и временем создания. 
   Пример:  
    logo@Kali:~$ docker images  
    | REPOSITORY  | TAG     | IMAGE ID   | CREATED      | SIZE    |
    |-------------|---------|------------|--------------|---------|
    | cp-zooker   | latest  | 26a87e...  | 6 days ago   | 1.08GB  |
    | ubuntu      | latest  | b1d9d...   | 4 weeks ago  | 78.1MB  |
 

---

### **2. Соберите собственный Docker-образ**  
1. Создайте файл `Dockerfile` со следующим содержимым:
   ```Dockerfile
   FROM ubuntu:latest
   RUN apt-get update && apt-get install -y curl
   CMD ["echo", "Custom Docker Image!"]
   ```
   Пример:  
   ```logo@Debian:~$ touch Dockerfile```  
   ```logo@Debian:~$ pluma Dockerfile```  
   
2. Соберите образ:```docker build -t my-custom-image```  
   Процесс сборки с логами, включая выполнение каждого шага `Dockerfile`.  
   Пример:  
   logo@Debian:~$ docker build -t my-custom-image .  
   [+] Building 0.4s (5/5) FINISHJED docker:default  

3. Запустите контейнер из образа: ```docker run my-custom-image```  
   Вывод команды, указанной в `CMD`. `Custom Docker Image!`.  
   Пример:  
   logo@Debian:~$ docker run my-custom-image  
   Custom Docker Image!  

---

### **3. Ознакомьтесь с командами Docker-клиента**

#### Команды:

- **Удаление контейнера:** ```docker rm <container_id>```  
  Удаление указанного контейнера. Если контейнер работает, потребуется добавить флаг `-f`.  

- **Удаление образа:** ```docker rmi <image_id>```  
  Удаление указанного образа. Если есть связанные контейнеры, удаление может быть заблокировано.  

- **Просмотр запущенных контейнеров:**  ```docker ps```  
  Таблицу с информацией о запущенных контейнерах: их ID, имя, статус, порт и использованный образ.  

- **Просмотр всех контейнеров:** ```docker ps -a```  
  Таблицу со всеми контейнерами (включая остановленные).  

- **Создание сети:** ```docker network create my-network```  
  Уникальный идентификатор созданной сети.  

- **Создание тома:** ```docker volume create my-volume```  
  Имя созданного тома.  

---

### **4. Изучите флаги команды `run`**

#### Примеры:

- **Монтирование тома (`-v`)**: ```docker run -v /host/path:/container/path ubuntu ls /container/path```  
  Содержимое каталога `/container/path`, смонтированного из `/host/path`.  

- **Проброс порта (`-p`)**: ```docker run -p 8080:80 nginx```  
  Запущенный Nginx, доступный локально на порту `8080`.  

- **Удаление контейнера после завершения (`--rm`)**: ```docker run --rm ubuntu echo "Temporary container"```  
  Сообщение `Temporary container`. Контейнер автоматически удаляется после завершения.  

- **Присвоение имени контейнеру (`--name`)**: ```docker run --name my-container ubuntu echo "Named container"```  
  Сообщение `Named container`. Контейнер будет доступен по имени `my-container`.  

- **Рестарт контейнера при сбое (`--restart`)**: ```docker run --restart always ubuntu```  
  Контейнер, который автоматически перезапустится при сбое или остановке.  

- **Интерактивный режим (`-i` и `-t`)**: ```docker run -it ubuntu bash```  
  Открытую командную строку внутри контейнера.  

- **Фоновый режим (`-d`)**: ```docker run -d nginx```  
  ID запущенного контейнера. Контейнер работает в фоновом режиме.  
