# Welcome to Docker Majordomo!

Доброго дня. Это предварительная версия majordomo под docker с правильным стеком.

# Stack
 - Supervisord - как центральный процесс запуска всех daemon
 - Nginx - как web-сервер
 - php-fpm - как php-сервер
 - cycle.php через supervisord для стабильности

## Что появится

В ближайших планах разработка под **alpine** образа. Так же чистка некоторых хвостов  и доработка **init**-скриптов и **make** файла. Так же вывод всего в стабильный образ в **regestry** и разработка **модулей** под docker-связку.

## Поддерживаемые в данный момент OS

Mainstream: Любая Linux os с поддержкой Docker и Docker-compose.
Windows: как дополнительная os, через ubuntu WSL
MacOs и Raspberry в ближайшем будущем будут протестированы.

## Установка под Linux

[инструкция с картинками](https://kb.mjdm.ru/%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-majordomo-%D0%B2-docker/)

1. Склонировать данный репозиторий.
 2. Скопировать config.env.dist в config.env и настраиваем под себя
    `cp config.env.dist config.env`
3. Подтянуть git
    `make clone_code"
4. Устанавливаем docker и docker-compose для себя. Как вариант(плохой) и перезапускаем сервер:
    `sudo apt get update && sudo apt-get install docker docker-compose && sudo usermod -aG docker $USER && reboot`
5. Запускаем сборку и подгружаем базу данных. У вас спросят удалять ли базу данных. Соглашаемся.
    `make install && make init-db`
6. Настраиваем в ./app config.php. Учтите, host mysql теперь: 127.0.0.1(а не localhost) и в Define('BASE_URL указываем ваш ip-адрес системы.
    `cp -f ./app/config.php.sample ./app/config.php && nano ./app/config.php`
7. Перезапускам всё, что бы заново иницилизировать cycle.
8. Открываем 127.0.0.1 или localhost или ip где запущен докер. Радуемся.

## Known bugs
Некоторые части могут быть не отлажены корректно. Поэтому в данный момент права выдаются не корректно. Что бы исправить:
`make exec-app`
`chown -R www-data:www-data /var/www`
`chmod -R 777 /var/www`

####problem whe init DB
- `cp app/db_terminal.sql db-data/db.sql`
- `docker-compose exec mysql bash`
- `mysqladmin -p drop majordomo`
            Enter password:
            Do you really want to drop the 'majordomo' database [y/N] y
            Database "majordomo" dropped
- `mysqladmin -p create majordomo`
        Enter password:
- `mysql -u root -p majordomo < var/lib/mysql/db.sql`
Enter password:

# Отзывы
Всем отзывам буду рад в Issue.
[link](https://github.com/A-SOM/docker-majordomo)

#TODO
- English version.
- Более внятная инструкция для Линукс систем.
- Исправить проблемы с правами