# Ansible-проект установки и конфигурации Redis
___
## Структура роли

``` text
s9277817.redis/
├── defaults|---|---|
│   └── main.yml
├── files
├── handlers
│   └── main.yml # Обработчики запуска и рестарта сервиса Redis
├── meta
│   └── main.yml
├── tasks
│   ├── install
│   │   ├── add_redis_gpg_keyring.yml # Добавление gpg-ключа для скачивания репозитория Redis
│   │   ├── add_redis_repo.yml # Добавление последнего APT-репозитория Redis
│   │   └── redis_installation.yml # Установка Redis
│   └── main.yml # Основные tasks роли
├── templates
│   └── redis.conf.j2 # Конфиг Redis
├── tests
│   ├── inventory
│   └── test.yml
├── vars
│   └── main.yml
└── README.md
```

## Окружения
### Проект имеет поддержку двух окружений с разными параметрами:
### Dev
|Параметр | Значение|
|---|---|
|Порт|6380|
|Количество баз данных (databases)|4|
|Каталог данных (dir)|/mnt/redis|
|Персистентность (appendonly)|no|
### Prod
|Параметр | Значение|
|---|---|
|Порт|6379|
|Количество баз данных (databases)|16|
|Каталог данных (dir)|/var/lib/redis|
|Персистентность (appendonly)|yes|

## Переменные
```
- redis_config_port # Порт Redis
- redis_data_dir # Каталог данных Redis
- redis_config_databases_count # Количество баз данных
- redis_config_appendonly # Персистентность
```

Каждая переменная переопределяма, но имеет дефолтные значения:
|Переменная| Значение|
|---|---|
|redis_config_port|6379|
|redis_data_dir|/var/lib/redis|
|redis_config_databases_count|16|
|redis_config_appendonly|yes|

## Инструкция по запуску плейбука
Команды запускают плейбук в соответсвии с нужным окружением, а также передают пароль Ansible-vault 
```
Запуск в dev окружении:
ansible-playbook playbook.yaml -i inventories/dev --ask-vault-pass

Запуск в prod окружении:
ansible-playbook playbook.yaml -i inventories/prod --ask-vault-pass
```
