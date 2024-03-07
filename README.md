# WA-CLIENT
#### Мэнеджер сообщений мессенджера WhatsApp
![Котик](https://sun9-79.userapi.com/impg/pVbh1d5AKlA3nJlllh_uiLYUTNHSQTWuY5R2zg/v8H8HpTQmgc.jpg?size=795x999&quality=96&sign=30e53c0a79c6e07a582b2caab938d321&type=album)

> При получении сообщения из *whatsapp*, оно отсылается на обозначенный эндпоинт в определенном формате. 
 Так же обьявлен энпоинт самого приложения, на который можно отправлять сообщения, которые парсятся и отправляются в *whatsapp*.

## Оглавление

Необходимые для работы клиента зависимости:
 - node версии 16.16
 - npm версии 7
 - mongodb версии 5.0.9

Для запуска приложения необходимо создать папку, содержащую папки Logs, Media, outbox, outbox.failed, outbox.not_found и файл пользовательских настроек clicker_settings.yaml.

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
