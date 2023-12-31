---
## Front matter
title: "Доклад"
subtitle: "на тему СУБД PostgreSQL"
author: "Демидова Екатерина Алексеевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

Целью работы является изучение процесса установки и настройки, а также основных команд СУБД PostgreSQL.

В работе поставлены следующие задачи:

- Рассмотротреть СУБД PostgreSQL и её основные преимущества. 
- Рассмотреть процесс установки PostgreSQL и работы в рассматриваемой СУБД.
- Изучить основные файлы и способы конфигурации PostgreSQL.

# Особенности PostgreSQL

СУБД — комплекс программ, позволяющих создать базу данных и манипулировать данными (вставлять, обновлять, удалять и выбирать). Система обеспечивает безопасность, надёжность хранения и целостность данных, а также предоставляет средства для администрирования БД[@subd:2007:bash]. 

PostgreSQL — это объектно-реляционная система управления базами данных корпоративного класса с открытым исходным кодом, которая поддерживает запросы SQL (реляционные) и JSON (нереляционные)[@postgresql:bash]. 

PostgreSQL основана на POSTGRES, Version 4.2 — программе, разработанной на факультете компьютерных наук Калифорнийского университета в Беркли.

Она предлагает множество современных функций:

- сложные запросы
- внешние ключи
- триггеры
- изменяемые представления
- транзакционная целостность
- многоверсионность

Кроме того, пользователи могут всячески расширять возможности PostgreSQL, например, создавая свои:

- типы данных
- функции
- операторы
- агрегатные функции
- методы индексирования
- процедурные языки

В числе преимуществ также можно выделить[@postgrespro:bash]:

- расширяемость (можно определять свои типы данных, создавать пользовательские функции);
- возможность индексирования географических объектов и присутствие расширения PostGIS;
- наследование концепция ООП, считающая, что абстрактный тип данных способен наследовать данные и набор функций уже существующего типа, способствуя переиспользованию компонентов ПО;
- масштабируемая система встроенных языков программирования;
- устойчивые и надежные функции репликаций и транзакций.

У PostgreSQL большой список поддерживаемых данных: численные типы, символьные типы, двоичные типы, «дата/время», булев тип, геометрические примитивы и т.д.

Следующие соглашения используются в описаниях команд: квадратные скобки ([ и ]) обозначают необязательные части. Фигурные скобки ({ и }) и вертикальная черта (|) обозначают выбор одного из предложенных вариантов. Многоточие (...) означает, что предыдущий элемент можно повторить.

Иногда для ясности команды SQL предваряются приглашением =>, а команды оболочки — приглашением $.

Под администратором здесь обычно понимается человек, ответственный за установку и запуск сервера, тогда как пользователь — кто угодно, кто использует или желает использовать любой компонент системы PostgreSQL.

# Установка PostgreSQL и работа с базами данных

Установите пакет Postgres вместе с пакетом -contrib, который добавляет некоторые дополнительные утилиты и функции:

```
sudo apt install postgresql postgresql-contrib
```

Мы можем проверить состояние службы PostgreSQL, чтобы убедиться, что установка прошла гладко. Введите команду ниже в терминал(рис. @fig:001):

```
sudo systemctl status postgresql
```

![Проверка состояния службы PostgreSQL](image/1.png){#fig:001 width=70%}

Как и любую другую серверную службу, доступную для внешнего мира, PostgreSQL рекомендуется запускать под именем отдельного пользователя. Эта учётная запись должна владеть только данными, которыми управляет сервер, и разделять её с другими службами не следует. В частности, рекомендуется не назначать этого пользователя владельцем исполняемых файлов PostgreSQL, чтобы их нельзя было подменить в случае компрометации серверного процесса.

PostgreSQL по умолчанию использует систему аутентификации на основе ролей[@postgrespro:bash]. Будучи суперпользователем, пользователь postgres имеет полные административные права. Введите следующую команду, чтобы войти в систему как пользователь postgres:

```
sudo su - postgres
```

Затем, чтобы получить доступ к командной строке PostgreSQL, введите:

```
psql
```

Теперь вы попадете в командную строку PostgreSQL, где сможете вводить команды SQL(рис. @fig:002).

![Командная строка PostgreSQL](image/2.png){#fig:002 width=70%}

Для создания пользователя в Unix-подобной системе следует искать команду useradd или adduser.

С помощью команды createuser в PostgreSQL вы можете создавать новые роли непосредственно из командной строки. «eademidova» будет названием нашей новой роли. Сначала нажмите Enter после ввода «q», чтобы завершить приглашение PostgreSQL. затем введите команду следующим образом:

```
createuser --interactive --pwprompt
```

Система предложит вам ввести пароль и имя новой роли (мы будем использовать «myuser»). Затем вам будет предложено выбрать, должна ли новая роль быть суперпользователем. Безопаснее ответить «нет», если только вам специально не требуются права суперпользователя для этой роли(рис. @fig:003).

![Создание нового пользователя](image/3.png){#fig:003 width=70%}

Просмотреть список пользователей можно с помощью psql-команды `\du`(рис. @fig:004).

![Список пользователей](image/4.png){#fig:004 width=70%}

С помощью команды «createdb» вы можете создавать новые базы данных точно так же, как вы можете создавать новые роли из командной строки. Начнем с создания новой базы данных под названием «mydb». Используйте следующую команду:

```
createdb mydb
```

Просмотреть список баз данных можно с помощью psql-команды `\l`(рис. @fig:005).

![Список баз данных](image/5.png){#fig:005 width=70%}

Удалить эту базу данных можно с помощью команды:

```
drop mydb
```

Чтобы получить доступ к новой базе данных с новой ролью, сначала выйдите из пользовательской оболочки postgres, набрав «exit» и нажав Enter. Затем введите следующую команду:

```
psql -d mydb -U myuser
```

Вам будет предложено ввести пароль для «myuser». После ввода пароля вы увидите приглашение PostgreSQL для базы данных mydb.

Также создать базу данных можно с помощью команд SQL. Создадим базу данных books_store с помщью команды `CREATE DATABASE books_store;`. Перейти в неё можно с помощью psql команды `\c books_store`(рис. @fig:006). После перехода в базу данных создадим в ней две таблицы с помощью выражения `CREATE TABLE`. Таблица author с двумя столбцами:

- `id` -- уникальный целочисленный идентификатор автора, выполняющий роль первичного ключа таблицы (и не поддерживающий NULL-значения по умолчанию);

- `name` -- текстовое поле, не поддерживающее NULL-значения. В нем будут храниться фамилии и инициалы авторов книг.

Таблица book с четырьмя столбцами:

- `id` -- уникальный целочисленный идентификатор книги, выполняющий роль первичного ключа таблицы (и не поддерживающий NULL-значения по умолчанию);

- `title` -- текстовое поле, не поддерживающее NULL-значения. В нем будут храниться названия книг;

- `author_id` -- целочисленное поле, не поддерживающее NULL-значения. В нем будут храниться уникальные идентификаторы авторов книг. Для явного определения связи между таблицами author и book (и соблюдения целостности их данных) будет добавлен внешний ключ fk_author, основанный на столбце author_id;

- `public_year` -- целочисленное поле, поддерживающее NULL-значения. В нем будут храниться годы публикации книг.


![Создание базы данных и таблиц в ней](image/6.png){#fig:006 width=70%}

Чтобы проверить существование таблицы и получить информацию о ней, можно использовать команды `\d <table_name>` и `\dt`(рис. @fig:007). Первая команда возвращает краткую информацию о столбцах таблицы с именем `<table_name>`: название, тип, collation, допустимость NULL-значений и значение по умолчанию. Также команда выводит индексы и первичные/внешние ключи таблицы. Команда `\dt` возвращает список всех отношений (relations), хранимых в текущей базе данных. По каждому объекту БД выводится следующая информация: имя схемы, название, тип и владелец.

![Получение информации о таблицах](image/7.png){#fig:007 width=70%}

# Определение параметров в файле конфигурации

В PostgreSQL существует большое количество параметров, влияющих на работу СУБД. Параметры позволяют управлять потреблением ресурсов, настраивать работу серверных процессов и многое другое. Например, при помощи параметра max_connections можно ограничить количество одновременных подключений к серверу.

Самый основной способ установки этих параметров — определение их значений в файле postgresql.conf, который обычно находится в каталоге данных[@postgresedu:bash]. При инициализации каталога кластера БД в этот каталог помещается копия стандартного файла. Например, он может выглядеть так: 

```
# Это комментарий
log_connections = yes
log_destination = 'syslog'
search_path = '"$user", public'
shared_buffers = 128MB
```

Это текстовый файл, хранящий параметры в формате «ключ=значение». Если один и тот же параметр указан в конфигурационном файле (файлах) несколько раз, то использоваться будет значение считанное последним. 

Каждый параметр определяется в отдельной строке. Знак равенства в ней между именем и значением является необязательным. Пробельные символы в строке не играют роли (кроме значений, заключённых в апострофы), а пустые строки игнорируются. Знаки решётки (#) обозначают продолжение строки как комментарий. Значения параметров, не являющиеся простыми идентификаторами или числами, должны заключаться в апострофы.

Этот конфигуарционный файл читается один раз при старте сервера. Для вступления в силу внесенных в файл изменений, необходимо, чтобы сервер перечитал файл:

```
$ pg_ctl reload
$ kill -HUP

psql# select pg_reload_conf();
```

Для некоторых параметров требуется перезагрузка сервера.

Рассмотрим типы данных параметров в конфигурационном файле:

- fsync = on (логическое значение)
- listen_addresses = 'localhost' (строка)
- max_connections = 100 (целое число)
- autovacuum_vacuum_scale_factor = 0.2 (дробное число)
- shared_buffers = 128MB (kB, MB, GB, TB)
- authentication_timeout = 1min (ms, s, min, h, d)
- wal_level = minimal (minimal, archive, hot_standby, logical)

Также в конфигурационный файл можно включать дополнительные файлы:

- include filename
- include_if_exists filename
- include_dir dirname (все *.conf файлы)


Самым последним считывается файл postgresql.auto.conf. Этот файл всегда располагается в каталоге данных (PGDATA).
Этот файл не следует изменять вручную. Для его редактирования предназначена команда ALTER SYSTEM. По сути ALTER SYSTEM представляет собой SQL-интерфейс для удаленного управления параметрами конфигурации.

Специальная команда для редактирования:

```
ALTER SYSTEM 
SET conf_parameter TO value; # добавляет или изменяет строки
ALTER SYSTEM 
RESET conf_parameter | ALL;  # удаляет строку или все строки
```

Для применения изменений, сделанных ALTER SYSTEM, сервер должен перечитать конфигурационные файлы, как и в случае с postgresql.conf.


Мы можем посмотреть параметры, которые не требуют перезапуска сервера с помощью команды:

```
SELECT name, setting, unit FROM pg_settings WHERE context = 'postmaster';
```

Получим следующие значения(рис. @fig:008).

![Параметры, изменение которых требует перезапуска сервера](image/8.png){#fig:008 width=70%}

Установим listen_addresses. Этот параметр задаёт адреса TCP/IP, по которым сервер будет принимать подключения клиентских приложений. Это значение принимает форму списка, разделённого запятыми, из имён и/или числовых IP-адресов компьютеров. Особый элемент, *, обозначает все имеющиеся IP-интерфейсы. 

Изменим файл postgresql.conf, задав `listen_addresses = '*'`. Затем проверим, что новое значение есть в файле и ждёт перезапуска(рис. @fig:009).

![Проверка значения нового параметра конфигурации](image/9.png){#fig:009 width=70%}

Для того чтобы применить конфигурацию перезапустим сервер. Затем, войдя в psql проверим применилось ли изменение(рис. @fig:010).

![Применение и проверка новой конфигурации](image/10.png){#fig:010 width=70%}

# Выводы

В работе был изучен процесса установки и настройки, а также основных команд СУБД PostgreSQL.

Были выполнены следующие задачи:

- Рассмотротреть СУБД PostgreSQL и её основные преимущества. 
- Рассмотреть процесс установки PostgreSQL и работы в рассматриваемой СУБД.
- Изучить основные файлы и способы конфигурации PostgreSQL.

# Список литературы{.unnumbered}

::: {#refs}
:::
