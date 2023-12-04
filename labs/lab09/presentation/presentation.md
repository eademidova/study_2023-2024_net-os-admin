---
## Front matter
lang: ru-RU
title: Лабораторная работа №9
subtitle: Настройка POP3/IMAP сервера
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

Приобретение практических навыков по установке и простейшему конфигурированию POP3/IMAP-сервера.

## Задание

1. Установите на виртуальной машине server Dovecot и Telnet для дальнейшей проверки корректности работы почтового сервера.
2. Настройте Dovecot.
3. Установите на виртуальной машине client программу для чтения почты Evolution и настройте её для манипуляций с почтой вашего пользователя. Проверьте корректность работы почтового сервера как с виртуальной машины server, так и с виртуальной машины client.
4. Измените скрипт для Vagrant, фиксирующий действия по установке и настройке Postfix и Dovecote во внутреннем окружении виртуальной машины server, создайте скрипт для Vagrant, фиксирующий действия по установке Evolution во внутреннем окружении виртуальной машины client. Соответствующим образом внесите изменения в Vagrantfile.

# Выполнение лабораторной работы

## Установка Dovecot

![Установка Dovecot](image/1.png){#fig:001 width=70%}

## Настройка dovecot

![Редактирование файла /etc/dovecot/dovecot.conf](image/2.png){#fig:002 width=70%}

## Настройка dovecot

![Редактирование файла /etc/dovecot/conf.d/10-auth.conf](image/3.png){#fig:003 width=70%}

## Настройка dovecot

![Просмотр файла /etc/dovecot/conf.d/auth-system.conf.ext](image/4.png){#fig:004 width=70%}

## Настройка dovecot

![Просмотр файла /etc/dovecot/conf.d/auth-system.conf.ext](image/5.png){#fig:005 width=70%}

## Настройка dovecot

![Редактирование файла /etc/dovecot/conf.d/10-mail.conf](image/6.png){#fig:006 width=70%}

## Настройка dovecot

![Конфигурация Postfix, межсетевого экрана для работы с POP3 и IMAP и запуск Dovecot](image/7.png){#fig:007 width=60%}



## Проверка работы Dovecot

На дополнительном терминале виртуальной машины server запустим мониторинг
работы почтовой службы с помощью команды:

```
tail -f /var/log/maillog
```

## Проверка работы Dovecot

![Просмотр почты и mailbox](image/8.png){#fig:008 width=70%}

## Проверка работы Dovecot

![Установка почтового клиента evolution на виртуальную машину client](image/9.png){#fig:009 width=70%}

## Проверка работы Dovecot

![Настройка учетной записи почтового клиента](image/10.png){#fig:010 width=60%}

## Проверка работы Dovecot

![Настройка IMAP-сервера для входящих сообщений](image/11.png){#fig:011 width=60%}

## Проверка работы Dovecot

![Настройка SMTP-сервера для исходящих сообщений](image/12.png){#fig:012 width=60%}

## Проверка работы Dovecot

![Проверка получения писем на почтовом клиенте](image/13.png){#fig:013 width=60%}

## Проверка работы Dovecot

![Просмотр мониторинга почтовой службы на сервере](image/14.png){#fig:014 width=70%}

## Проверка работы Dovecot

![Просмотр информации о почтовой службе с помощью doveadm и mail](image/15.png){#fig:015 width=70%}

## Проверка работы Dovecot

![Проверка почтовой службы с помощью протокола Telnet](image/16.png){#fig:016 width=55%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

![Создание окружения для внесения изменений в настройки окружающей среды](image/17.png){#fig:017 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

![СИзменение файла /vagrant/provision/server/mail.sh](image/18.png){#fig:018 width=50%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

![Изменение файла /vagrant/provision/client/mail.sh](image/19.png){#fig:019 width=70%}

# Заключение

## Выводы

В результате выполнения данной работы были приобретены практические навыки по установке и простейшему конфигурированию POP3/IMAP-сервера.