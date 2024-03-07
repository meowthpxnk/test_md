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

3. После успешного запуска приложения, необходимо отсканировать полученный QR код со своего аккаунта **WhatsApp**. *Settings -> Linked Devices -> Link Device*

## Настройки
Приложение обладает гибкими настройками, которые можно задать сформировав `.yml` файл по заданному [шаблону](./clicker_settings.yaml "Перейти к примеру настроек").

### Переменные окружения
* ❌ - обязательный параметр
* ♻️ - необязательный параметр
<table>
    <tr>
        <th>Название</th>
        <th>Описание</th>
        <th>По умолчанию</th>
    </tr>
    <tr>
        <td colspan=3 align="left"><b><i>chat2desk</i></b></td>
    </tr>
    <tr>
        <td>gateway</td>
        <td>Root эндпоинт, с которым будет общаться приложение</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td>attachmentSizeLimit</td>
        <td>Максимальный размер вложений, в Мб</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>browser</td>
        <td>Название браузера</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>checkMessageStatusDelay</td>
        <td>Время ожидания ожидания статуса сообщения</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>createDateOffset</td>
        <td>Неизвестно</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>dontMarkMessageRead</td>
        <td>Отключает пометку сообщений прочитанными</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>duplicateMessages</td>
        <td>Повторяет отправку сообщений</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>duplicateMessageDelay</td>
        <td>Время ожидания статуса сообщения для дублирования сообщений</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>groupDeletable</td>
        <td>Включает выход из групп при удалении чатов</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>firstMessageDelay</td>
        <td>Задержка между отправкой пустого сообщения и первого при инициации диалога</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>messageDaysLimit</td>
        <td>Неизвестно</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>nonDeletableChats</td>
        <td>Кол-во чатов, сверх которого лишние будут удаляться, если включено удаление</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>offsetDeletableChats</td>
        <td>Неизвестно</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>os</td>
        <td>Название ОС</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>os_release</td>
        <td>Версия ОС</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>skipMessageDuplication</td>
        <td>Отключает повторную отправку сообщени</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>skipSendInitialMessage</td>
        <td>Отключает отпарвку пустого сообщения при инициации диалога</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>user_agent</td>
        <td>Юзер-агент</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>user_path</td>
        <td>Путь к пользовательской папке</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td>whatsapp_web_version</td>
        <td>Версия web whatsapp</td>
        <td align="center"">♻️</td>
    </tr>
    <tr>
        <td colspan=3 align="left"><b><i>clicker_server</i></b></td>
    </tr>
    <tr>
        <td>host</td>
        <td>Хост внутреннего сервера</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td>port</td>
        <td>Порт внутреннего сервера</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td colspan=3 align="left"><b><i>common</i></b></td>
    </tr>
    <tr>
        <td>device_number</td>
        <td>Номер телефона используемого для подключения</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td>one_file_auth_state</td>
        <td>Метод хранения данных авторизации</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td colspan=3 align="left"><b><i>control</i></b></td>
    </tr>
    <tr>
        <td>host</td>
        <td>URL коннектора</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td colspan=3 align="left"><b><i>database</i></b></td>
    </tr>
    <tr>
        <td>host</td>
        <td>Хост базы данных</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td>port</td>
        <td>Порт базы данных</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td colspan=3 align="left"><b><i>whatsapp</i></b></td>
    </tr>
    <tr>
        <td>host</td>
        <td>Хост api развернутого приложения</td>
        <td align="center"">❌</td>
    </tr>
    <tr>
        <td>port</td>
        <td>Порт api развернутого приложения</td>
        <td align="center"">❌</td>
    </tr>
</table>

## API

Для удаленного управления работой приложения выделен API интерфейс.

### Методы интерфейса:
* **GET** ***(/media)*** - запрос на получения медиа, исходя из ID сообщения
* **GET** ***(/avatar)*** - запрос на получение аватара пользователя, исходя из его ID
* **POST** ***(/outbox)*** - запрос на отправку сообщения в мессенджер.

## База данных / MongoDB
Для реализации хранения данных в проекте выбрана нереляционная база данных, т.к. нет большой необходимости хранить постоянные данные. В основном база данных служит хранилищем очередей сообщений/загрузок, и для сохранения их если проект вдруг упадет.

Название базы данных: **WA_MD_*`{номер телефона аккаунта}`*_1**

### Коллекции:
**meta** - неизвестно
* **contacts** - контакты
* **readMessages** - прочитанные сообщения
* **sentMessages** - отправленные сообщения
* **inMessages** - входящие сообщения
* **outMessages** - исходящие сообщения
* **failedMessages** - проблемные сообщения?
* **fromWaQueue** - данные авторизации
* **creds** - данные авторизации
* **keys** - данные авторизации
* **preKeys** - данные авторизации
* **sessions** - данные авторизации
* **appStateSyncKeys** - данные авторизации
* **appStateVersions** - данные авторизации
* **senderKeys** - данные авторизации

Запуск whatsapp-клиента выполняется следующей командой:
```node <path_to_clicker.js> <user_path>```

## Архитектура
### Clicker
Директория предназначенная для исполняемых файлов приложения
Подробнее:
* **clicker.ts** - основной исполняемый файл. Инициализация всех основных сущностей и запуск приложения
* **constants.ts** - используемые константные значения
* **custom_auth_utils.ts** - утилиты для самописной авторизации через базу данных
* **helpers.ts** - вспомогательные функции, для связи с контрольным центром.
* **mongodb.ts** - работа с базой данных
* **outbox_reader.ts** - HTTP сервер, для приема сообщений на отправку в мессенджер. + встроенная очередь сообщений.
* **queue.ts** - Абстрактная модель очереди
* **reader.ts** - сборщик сообщений из мессенджера и отправка на Root сервер
* **sender.ts** - сборщик сообщений с Root сервера и отправка в мессенджер
* **settings.ts**
* **utilities.ts**

### clicker_settings.yaml
Файл примера настроек кликера

## Работа приложения

## Памятка разработчику

### Локальный запуск

1. После загрузки проекта используем команду 
```sh
yarn
```
> Если нет yarn
```sh
npm install -g yarn
```

2. Устанавливаем MongoDB на наш компьютер и запускаем.
> Для нормальной отладки, и просмотра содержимого базы данных, рекомендуется использовать **MongoDBCompass**.

3. Понадобится любой развернутый сервер который будет отвечать на запросы
* POST http://server *(Gateway)*
* POST http://server/qr/add *(Control)*
* POST http://server/device/status *(Control)*
И отвечать на них телом
```json
{
    "status": "success"
}
```
**Host** и **port** этого сервера стоит внести в ***`Control`*** и в ***`Gateway`*** в настройках

4. Из директории которой будем работать создаем папку под названием используемого **номера телефона**, копируем туда пример настроек девайса и заполняем.

5. Из рабочей директории вызываем скрипт
```sh
npx ts-node ./Clicker/clicker.ts {PHONE_NUMBER}
```
> ***./Clicker/clicker.ts*** - путь до исполняемого файла, от может оказаться другим

> Если **typescripn** is not recognised

```sh
npm install -g typescript
```

> Если нужно избежать **ошибок типизации** используем аргумент **-T**
```sh
npx ts-node -T clicker.ts {PHONE_NUKMBER}
```

6. После успешной авторизации приложение будет готово к работе.

### Запуск через сборку проекта
1. Из нашей ключевой библиотеки выполняем команду
```sh
tsc
```
> Если **typescrip** is not recognised

```sh
npm install -g typescript
```

2. Проходимся по всем пунктам из раздела [Локальный запуск](#локальный-запуск), до старта проекта.

3. Запускаем проект
```
node lib/clicker.js
```

4. После успешной авторизации приложение будет готово к работе.


## Термины
* Коннектор - Ключевой центр который собирает информацию о работах подобных инстансах приложения.
* Вложения / Медиа - Медиа файлы приложенные к сообщению с одной или другой стороны диалога.