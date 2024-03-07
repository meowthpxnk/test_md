# WA-CLIENT
#### Мэнеджер сообщений мессенджера WhatsApp
![Котик](https://psv4.userapi.com/c909518/u192567609/docs/d24/523bf2b9d272/Untitled.png?extra=yJq8sVhJ9FH5yYxG6G6OC1GIfP0KJEVMDhgvB65qWHkhL6wpeq4W_KcRLGLQwyKNOzSSMOfTu-KZG1l_N1TIR5YySDWu45LdjgvNnWi08ioQJmQyTkOhRgu8pr4Lozecjhpe5FIMXWcVhlpWRqsIcfvN)

> При получении сообщения из *мессенджера*, оно отсылается на обозначенный *эндпоинт* в определенном формате. 
 Так же обьявлен *эндпоинт* самого приложения, на который можно отправлять сообщения, которые парсятся и отправляются в *мессенджер*.

## Оглавление
- [Ключевые зависимости](#ключевые-зависимости)
- [Запуск](#запуск)
- [Настройки](#настройки)
  <!-- - [Реализация](#реализация) -->
  <!-- - [Переменные окружения](#переменные-окружения) -->
- [API](#api)
- [База данных / MongoDB](#база-данных--mongodb)
  <!-- - [Коллекции](#коллекции) -->
<!-- - [Локализация](#локализация) -->
- [Архитектура](#архитектура)
  <!-- - [app](#app) -->
  <!-- - [FakeGateway](#fakegateway) -->
  <!-- - [languages](#languages) -->
  <!-- - [settings](#settings) -->
  <!-- - [Дополнительно](#дополнительно) -->
- [Работа приложения](#работа-приложения)
  <!-- - [Обработка сообщений](#обработка-сообщений) -->
  <!-- - [Запуск приложения](#запуск-приложения) -->
- [Памятка разработчику](#памятка-разработчику)
  <!-- - [Локальный запуск](#локальный-запуск) -->
  <!-- - [Локальный запуск используя Docker](#локальный-запуск-используя-docker) -->
  <!-- - [Работа с логами](#работа-с-логами) -->
- [Термины](#термины)

## Ключевые зависимости
[![NodeJS](https://img.shields.io/badge/NODEJS-16.16-6DA55F?style=for-the-badge&logo=node.js&logoColor=6DA55F)](https://nodejs.org/en/blog/release/v16.16.0 "Node JS") [![npm](https://img.shields.io/badge/Npm-7-red?style=for-the-badge&logo=npm&logoColor=red) ](https://www.npmjs.com/package/npm/v/7.0.0 "NPM")

[![MongoDB](https://img.shields.io/badge/MONGODB->=5.0.9-589636?style=for-the-badge&logo=mongodb&logoColor=589636)](https://www.mongodb.com/docs/manual/release-notes/5.0/ "MongoDB")

[![Baileys](https://img.shields.io/badge/@whiskeysockets/baileys-6.6.0-25d366?style=for-the-badge&logo=whatsapp&logoColor=25d366)](https://github.com/WhiskeySockets/Baileys "Страница библиотеки Baileys")

## Запуск
1. Для запуска приложения, потребуется в директориии(*из которой ведется запуск*) создать папку с названием номера телефона, который подключаем в приложение.

2. В папке создаем файл `clicker_settings.yaml`, основанный на примере [файла настроек](./clicker_settings.yaml "Перейти к примеру настроек")

1. Для запуска приложения, потребуются `API ID` и `API hash` приложения от **Telegram**. Получить их можно здесь: https://my.telegram.org/auth.

1. Для запуска приложения, потребуются `API ID` и `API hash` приложения от **Telegram**. Получить их можно здесь: https://my.telegram.org/auth.

Пользовательские настройки:
Обязательные:
chat2desk:
  gateway: урл gw-gateway для отправки данных
clicker_server:
  host: хост внутреннего сервера
  port: порт внутреннего сервера
common:
  device_number: номер телефона подключенного whatsapp
  one_file_auth_state: определяет метод хранения данных авторизации
control:
  host: урл коннектора
database:
  host: хост MongoDB
  port: порт MongoDB
whatsapp:
  host: хост приложения
  port: порт приложения

Опциональные:
common:
  attachmentSizeLimit: максимальный размер вложений, в Мб
  browser: название браузера
  checkMessageStatusDelay: number, time of message status waiting
  createDateOffset: неизвестно
  dontMarkMessageRead: отключает пометку сообщений прочитанными
  duplicateMessages: повторяет отправку сообщений?
  duplicateMessageDelay: время ожидания статуса сообщения для дублирования сообщений
  groupDeletable: включает выход из групп при удалении чатов
  firstMessageDelay: задержка между отправкой пустого сообщения и первого при инициации диалога
  messageDaysLimit: неизвестно
  nonDeletableChats: кол-во чатов, сверх которого лишние будут удаляться, если включено удаление
  offsetDeletableChats: неизвестно
  os: название ОС
  os_release: версия ОС
  skipMessageDuplication: отключает повторную отправку сообщений
  skipSendInitialMessage: отключает отпарвку пустого сообщения при инициации диалога
  user_agent: юзер-агент
  user_path: путь к пользовательской папке
  whatsapp_web_version: версия web whatsapp

База данных / MongoDB
Для реализации хранения данных в проекте выбрана нереляционная база данных, т.к. нет большой необходимости хранить постоянные данные. В основном база данных служит хранилищем очередей сообщений/загрузок, и для сохранения их если проект вдруг упадет.

Название базы данных: WA_MD_<phone>_1

Коллекции:
meta - неизвестно
contacts - контакты
readMessages - прочитанные сообщения
sentMessages - отправленные сообщения
inMessages - входящие сообщения
outMessages - исходящие сообщения
failedMessages - проблемные сообщения?
fromWaQueue - данные авторизации
creds - данные авторизации
keys - данные авторизации
preKeys - данные авторизации
sessions - данные авторизации
appStateSyncKeys - данные авторизации
appStateVersions - данные авторизации
senderKeys - данные авторизации

Запуск whatsapp-клиента выполняется следующей командой:
```node <path_to_clicker.js> <user_path>```



Структура проекта:

1. package.json, 
   - зависимости - из корня проекта выполнить ```npm install```, 
   что создаст папку node_modules со всеми библиотеками, используемыми в проекте
2. tsconfig.json
   - для компиляции .ts файлов из Clicker в исполняемый js
4. lib
   - исполняемые приложением файлы
   - компилируется командой ```tsc``` из корня проекта (требуется папка Clicker)
5. Clicker
   - clicker.ts 
     - инициализация websocket и всех компонентов проекта
     - определение обработчиков нужных событий (сообщения, авторизация в whatsapp, и т.д.)
   - reader.ts - обработка и отправка сообщений из whatsapp
     - подготавливает сообщение для отправки в chat2desk
     - отправляет сообщения в chat2desk
   - sender.ts - отправка сообщений в whatsapp
   - outbox_reader.ts 
     - получает сообщения из chat2desk
     - сохраняет в UserData/outboxes
     - передает сообщение в sender.ts
   - queue.ts 
     - ReadQueue - реализация простой очереди для сообщений из whatsapp
   - helpers.ts
     - sendFromQueue
     - beforeReaderSend - выполняется перед каждой отправкой сообщения
       - загружает непрочитанные сообщения из локальной бд
     - c2dHelper - отправка в chat2desk статусов и qr
   - utilities.ts, settings.ts, constants.ts

### Workflow ###

1. делаем изменения в Clicker/
2. собираем исполняемый js командой ```tsc``` из корня проекта
3. запускаем командой ```node lib/clicker.js```
