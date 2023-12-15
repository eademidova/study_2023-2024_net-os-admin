---
## Front matter
lang: ru-RU
title: Доклад
subtitle: СУБД PostgreSQL
author:
  - Демидова Е. А.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 11 декабря 2023

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
---

# Вводная часть

## Цель работы

Целью работы является изучение процесса установки и настройки, а также основных команд СУБД PostgreSQL.

## Задачи

- Рассмотротреть СУБД PostgreSQL и её основные преимущества. 
- Рассмотреть процесс установки PostgreSQL и работы в рассматриваемой СУБД.
- Изучить основные файлы и способы конфигурации PostgreSQL.


# Характеристики PostgreSQL

## Что такое PostgreSQL

PostgreSQL — это объектно-реляционная система управления базами данных корпоративного класса с открытым исходным кодом

## Функциии PostgreSQL

- сложные запросы
- внешние ключи
- триггеры
- изменяемые представления
- транзакционная целостность
- многоверсионность

## Преимущества PostgreSQL

- расширяемость
- возможность индексирования географических объектов
- наследование
- масштабируемая система встроенных языков программирования
- устойчивые и надежные функции репликаций и транзакций

# Работа с СУБД PostgreSQL

## Установка PostgreSQL

```
sudo apt install postgresql postgresql-contrib
sudo systemctl status postgresql
```

![Проверка состояния службы PostgreSQL](image/1.png){#fig:001 width=70%}

## Пользователи в PostgreSQL

```
sudo su - postgres
psql
```

![Командная строка PostgreSQL](image/2.png){#fig:002 width=70%}

## Пользователи в PostgreSQL

```
createuser --interactive --pwprompt
```

![Создание нового пользователя](image/3.png){#fig:003 width=70%}

## Пользователи в PostgreSQL

psql-команда - `\du`

![Список пользователей](image/4.png){#fig:004 width=70%}

## Работа с базами данных в PostgreSQL

```
createdb mydb
drop mydb
```

psql-команда - `\l`

![Список баз данных](image/5.png){#fig:005 width=70%}

## Работа с базами данных в PostgreSQL

![Создание базы данных и таблиц в ней](image/6.png){#fig:006 width=70%}

## Работа с базами данных в PostgreSQL

![Получение информации о таблицах](image/7.png){#fig:007 width=70%}

# Конфигурирование PostgreSQL

## Файл postgresql.conf

```
# Это комментарий
log_connections = yes
log_destination = 'syslog'
search_path = '"$user", public'
shared_buffers = 128MB
```

## Внесение изменений конфигураций

```
$ pg_ctl reload
$ kill -HUP

psql# select pg_reload_conf();
```

## Типы данных в конфигурационном файле

- fsync = on (логическое значение)
- listen_addresses = 'localhost' (строка)
- max_connections = 100 (целое число)
- autovacuum_vacuum_scale_factor = 0.2 (дробное число)
- shared_buffers = 128MB (kB, MB, GB, TB)
- authentication_timeout = 1min (ms, s, min, h, d)

## Включение дополнительных файлов

- `include filename`
- `include_if_exists filename`
- `include_dir dirname` (все *.conf файлы)

## Файл postgresql.auto.conf

```
ALTER SYSTEM 
SET conf_parameter TO value; # добавляет или изменяет строки
ALTER SYSTEM 
RESET conf_parameter | ALL;  # удаляет строку или все строки
```

## Работа с конфигурацией PostgreSQL

```
SELECT name, setting, unit FROM pg_settings WHERE context = 'postmaster';
```

![Параметры, изменение которых требует перезапуска сервера](image/8.png){#fig:008 width=50%}

## Работа с конфигурацией PostgreSQL

![Проверка значения нового параметра конфигурации](image/9.png){#fig:009 width=70%}

## Работа с конфигурацией PostgreSQL

![Применение и проверка новой конфигурации](image/10.png){#fig:010 width=70%}

# Заключение

В докладе была изучена СУБД PostgreSQL.

Выполнены задачи:

- Рассмотротрена СУБД PostgreSQL и её основные преимущества. 
- Рассмотрен процесс установки PostgreSQL и работы в рассматриваемой СУБД.
- Изучены основные файлы и способы конфигурации PostgreSQL.

# Список литературы

1. Кузнецов С.Д. Основы баз данных. М.: Интернет-университет информационных технологий; БИНОМ. Лаборатория знаний, 2007. 484 с.
2. PostgreSQL [Электронный ресурс]. The PostgreSQL Global Development Group, 2023. URL: https://www.postgresql.org/.
3. PostgresPRO [Электронный ресурс]. Postgres Pro, 2023. URL: https://postgrespro.ru/docs/postgresql/16/intro-whatis.
4. PostgresPRO Education [Электронный ресурс]. Postgres Professional, 2023. URL: https://edu.postgrespro.ru.