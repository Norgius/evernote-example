# Оставляем заметки
Данный проект написан для работы с `API` веб-сервисом [Evernote](https://sandbox.evernote.com/), для просмотра заметок, а также для их копирования.

## Что необходимо для запуска
В данном проекте вы будуте работать с `Python2`. Для его установки поочередно выполните команды:
```
sudo apt install python2-minimal
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
python2.7 get-pip.py
python2.7 -m pip install virtualenv
```
Файл `get-pip.py` можно удалить. Он был неоходим для установки `pip` на `Python2`.

Теперь, когда Python2 установлен можно создать виртуальное окружение и установить необходимые зависимости:
```
virtualenv env --python=python2.7
. ./env/bin/activate
pip install -r requirements.txt
```

Вот теперь необходимо получить `токен`. Следуя данной инструкции, вы его сможете получить [Инструкция](https://dev.evernote.com/doc/start/python.php)

Токен будет вида: 
```
S=s1:U=96cbc:E=18bd41tfca0:C=1847c6ed1a0:P=1cd:A=en-devtoken:V=2:H=ed71e53426de07e70b5d70a93fb9d952
```
Если запутались в инструкции, то его можно получить [по адресу](https://sandbox.evernote.com/api/DeveloperToken.action). Но предварительно, вы обязательно должны зарегистрироваться.

Сам ключ будем хранить в файле `.env`, который необходимо создать в корневой директории проекта:
```
EVERNOTE_PERSONAL_TOKEN=
```
Отлично, далее вы должны создать пару блокнотов уже будучи зарегистрированными на сайте [sandbox.evernote](https://sandbox.evernote.com/).

Для получения идентификаторов самих блокнотов и их заметок будем запускать `python-скрипты`.

В файле `.env` в процессе запуска узнаем и допишем недостающие `GUID`:
```
JOURNAL_TEMPLATE_NOTE_GUID=
JOURNAL_NOTEBOOK_GUID=
INBOX_NOTEBOOK_GUID=
```
## Файл config.py
Файл берет значения токена и `GUID` блонкотов и заметок, которые вы указываете в файле `.env`.

## Файл list_notebooks.py
Данный файл подключается к API Evernote с помощью полученного ранее `токена` и отображает идентификаторы `GUID` и названия блокнотов.

## Файл dump_inbox.py
Данный файл выводит выводит в терминал текст заметки и её `GUID`.

Если в файле `.env` значение `INBOX_NOTEBOOK_GUID` отсутствует, то в консоль будут выводиться все заметки вашей учетной записи, однако, если указать `GUID` определенного дневника, то и заметки будут только с него.

Файл при запуске выводит 10 заметок (если они есть), вы можете поменять это значение, указав в аргументах нужное вам число, например 3:
```
python dump_inbox.py 3
```
Значения `GUID` заметок, которые выводятся в консоль, понадобятся в работе другого файла этого проекта.

## Файл add_note2journal.py
Данный файл позволяет копировать выбранную вами заметку в блокнот (не в тот, в котором находится сама заметка).

В файле `.env` указываем `GUID` значение выбранной заметки для ключа `JOURNAL_TEMPLATE_NOTE_GUID`. Значение блокнота, в который будем копировать заметку запишем для ключа `JOURNAL_NOTEBOOK_GUID`.

Указав в конце заголовка выбранной вами заметки `{}, {}` программа дозаполнит заголовок заметки датой и днем недели. Например:
```
Покупки в торговом центре. {}, {}
```
На выходе вы получите копию заметки, которую скопировали, вот с таким заголовком:
```
Покупки в торговом центре. 2022-11-17, четверг
```
Она уже будет находиться в другом блонкоте. Если же вы не укажете данные символы `{}, {}`, всё будет работать, но без измененного заголовка заметки.

По умолчанию, если не указать дату самому, то программа автоматически выберет сегодняшнее число.

Для того, чтобы узнать в каком формате указать дату, выполните запрос, указанный ниже:
```
python add_note2journal.py -h
```
Вызов должет быть такого типа:
```
python add_note2journal.py 2010-10-20
```