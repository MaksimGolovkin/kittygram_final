![Workflow Status](https://github.com/MaksimGolovkin/kittygram_final/actions/workflows/main.yml/badge.svg)

# Проект Kittygram - платформа для обмена фотографиями домашних питомцев.

### Описание проекта:
#### Позволяет публиковать пользователям фотографии своих домашних животных, с кратким описанием.


### Стек применяемых технологий:

- __Python__, 
- __Django__,
- __Django rest framework__, 
- __PostgreSQL__

### _Запуск проекта на локальном сервере:_ 

1. Клонировать репозиторий:

```
git clone https://github.com/yandex-praktikum/kittygram2plus.git
```

2. Создать Docker-образ для контейнеров:
```
cd frontend
docker build -t USERNAME/kittygram_frontend .
cd ../backend
docker build -t USERNAME/kittygram_backend .
cd ../nginx
docker build -t USERNAME/kittygram_gateway .
```
3. Запушить образы на DockerHub

```
docker push USERNAME/kittygram_frontend
docker push USERNAME/kittygram_backend
docker push USERNAME/kittygram_gateway
```
Вместо USERNAME вставить свой логин на DockerHub.

4. Запустите контейнеры, перейдя в корневую директорию, командой:
```
sudo docker compose -f docker-compose.production.yml up -d
```
5. Примените миграции, соберите статику бэкенда и скопируйте для раздачи:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
#### _Для локальной развертки более ничего не требуется, перейдите по адресу "(https://localhost/)"_

---

### _Запуск проекта на удаленном сервере:_
1. Клонировать репозиторий:

```
git clone https://github.com/yandex-praktikum/kittygram2plus.git
```

2. Создать Docker-образ для контейнеров:
```
cd frontend
docker build -t USERNAME/kittygram_frontend .
cd ../backend
docker build -t USERNAME/kittygram_backend .
cd ../nginx
docker build -t USERNAME/kittygram_gateway .
```
Вместо USERNAME вставить свой логин на DockerHub.

3. Запушить образы на DockerHub

```
docker push USERNAME/kittygram_frontend
docker push USERNAME/kittygram_backend
docker push USERNAME/kittygram_gateway
```
Вместо USERNAME вставить свой логин на DockerHub.

4. Подключитесь к удаленному серверу:
```
ssh -i PATH_TO_SSH_KEY/SSH_KEY_NAME YOUR_USERNAME@SERVER_IP_ADDRESS 
```

5. Создайте директорию в которой будет распологаться инструкции для Docker-a:
```
mkdir kittigram
```

6. Установите Docker Compose на сервер:
```
sudo apt update
sudo apt install curl
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo apt install docker-compose
```

7. Скопируйте файлы "docker-compose.production.yml" и ".env" в директорию kittygram/ на сервере:
```
scp -i PATH_TO_SSH_KEY/SSH_KEY_NAME docker-compose.production.yml YOUR_USERNAME@SERVER_IP_ADDRESS:/home/YOUR_USERNAME/kittygram/docker-compose.production.yml
```
    - 'PATH_TO_SSH_KEY' - путь к файлу с вашим SSH-ключом
    - 'SSH_KEY_NAME' - имя файла с вашим SSH-ключом
    - 'YOUR_USERNAME' - ваше имя пользователя на сервере
    - 'SERVER_IP_ADDRESS' - IP-адрес вашего сервера

```
scp -i PATH_TO_SSH_KEY/SSH_KEY_NAME docker-compose.production.yml YOUR_USERNAME@SERVER_IP_ADDRESS:/home/YOUR_USERNAME/kittygram/.env
```
    - 'PATH_TO_SSH_KEY' - путь к файлу с вашим SSH-ключом
    - 'SSH_KEY_NAME' - имя файла с вашим SSH-ключом
    - 'YOUR_USERNAME' - ваше имя пользователя на сервере
    - 'SERVER_IP_ADDRESS' - IP-адрес вашего сервера

В файле ".env" необходимо указать переменные окружения, ожидаются следующие:

    - 'SECRET_KEY' - Секректный ключ от Django, установлен по умолчанию.
    - 'DEBUG_VALUE' - Переключатель режима откладки, по умолчанию 'False'
    - 'ALLOWED_HOSTS' - Список строк, представляющих имена хоста/домена, которые может обслуживать Django, по умолчанию 'localhost'
    - 'SQLITE3_VALUE' - Переключатель базы данных, по умолчанию 'False'.
    - 'POSTGRES_DB' - Имя базы данных PostgreSQL.
    - 'POSTGRES_USER' - Имя пользователя PostgreSQL.
    - 'POSTGRES_PASSWORD' - Пароль пользователя PostgreSQL.
    - 'DB_NAME' - адрес, по которому Django будет соединяться с базой данных. (Имя контейнера)
    - 'DB_PORT' - порт, по которому Django будет обращаться к базе данных. 5432 — это порт по умолчанию для PostgreSQL.


8. Запустите контейнеры, перейдя в корневую директорию, командой:
```
sudo docker compose -f docker-compose.production.yml up -d
```
9. Примените миграции, соберите статику бэкенда и скопируйте для раздачи:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
10. Откройте конфигурационный файл Nginx в редакторе nano:
```
sudo nano /etc/nginx/sites-enabled/default
```

11. Добавьте настройки "location" в секции "server":
```
location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}
```
12. Перезапустите Nginx:
```
sudo service nginx reload
```

#### _Для удаленной развертки более ничего не требуется, перейдите по адресу "(https://akittygramm.sytes.net/)"_

---
_Автор проекта - maksimgolovkin96. :)_