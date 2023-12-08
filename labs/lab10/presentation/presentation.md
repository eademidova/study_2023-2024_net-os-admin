---
## Front matter
lang: ru-RU
title: Лабораторная работа №10
subtitle: Расширенные настройки SMTP-сервера
author:
  - Демидова Е. А.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 4 декабря 2023 

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

Приобретение практических навыков по конфигурированию SMTP-сервера в части настройки аутентификации.

## Задание

1. Настройте Dovecot для работы с LMTP.
2. Настройте аутентификацию посредством SASL на SMTP-сервере.
3. Настройте работу SMTP-сервера поверх TLS.
4. Скорректируйте скрипт для Vagrant, фиксирующий действия расширенной настройки SMTP-сервера во внутреннем окружении виртуальной машины server.

# Выполнение лабораторной работы

## Настройка LMTP в Dovecote

![Изменение списка протоколов для работы с Dovecot](image/1.png){#fig:001 width=70%}

## Настройка LMTP в Dovecote

![Настройка сервиса  lmtp для связи с Postfix](image/2.png){#fig:002 width=70%}

## Настройка LMTP в Dovecote

Переопределим в Postfix с помощью postconf передачу сообщений не на прямую, а через заданный unix-сокет с помощью команды:

```
postconf -e 'mailbox_transport = lmtp:unix:private/dovecot-lmtp'
```

## Настройка LMTP в Dovecote

![Задание формата имени пользователя](image/3.png){#fig:003 width=70%}

## Настройка LMTP в Dovecote

![Просмотр мониторинга почтовой службы](image/4.png){#fig:004 width=70%}

## Настройка LMTP в Dovecote

![Просмотр почтового ящика пользователя](image/5.png){#fig:005 width=70%}

## Настройка SMTP-аутентификации

![Определение службы аутентификации пользователей](image/6.png){#fig:006 width=70%}

## Настройка SMTP-аутентификации

![Конфигурации Postfix](image/7.png){#fig:007 width=70%}

## Настройка SMTP-аутентификации

![Временный запуск SMTP-сервера](image/8.png){#fig:008 width=70%}

## Настройка SMTP-аутентификации

![Получение строки для аутентификация и проверка посредством telnet](image/9.png){#fig:009 width=70%}

## Настройка SMTP over TLS

![Конфигарции Postfix для настройки TLS](image/10.png){#fig:010 width=70%}

## Настройка SMTP over TLS

![Изменение конфигураций для запуска SMTP-сервера на 587-порту](image/11.png){#fig:011 width=70%}

## Настройка SMTP over TLS

![Настройка межсетевого экрана для работы службы smtp-submission](image/12.png){#fig:012 width=70%}

## Настройка SMTP over TLS

![Подключение через openssl к SMTP-серверу](image/13.png){#fig:013 width=60%}

## Настройка SMTP over TLS

![Проверка подключеня и аутентфикации по telnet](image/14.png){#fig:014 width=70%}

## Настройка SMTP over TLS

![Изменение настроек учетной записи Evolution](image/15.png){#fig:015 width=50%}

## Настройка SMTP over TLS

![Проверка корректности отправки почтовых сообщений с помощью Evolution](image/16.png){#fig:016 width=60%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

![Создание окружения для внесения изменений в настройки окружающей среды](image/17.png){#fig:017 width=60%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

![Изменение файла /vagrant/provision/server/mail.sh](image/18.png){#fig:018 width=45%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

![Изменение файла /vagrant/provision/client/mail.sh](image/19.png){#fig:019 width=70%}

# Заключение

## Выводы

В результате выполнения данной работы были приобретены практические навыки по конфигурированию SMTP-сервера в части настройки аутентификации.