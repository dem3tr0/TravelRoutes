# TravelRoutes
Мы - команда Equal Squad из Хабаровского края выполнили отборочное задание на международный научный фестиваль школьников **Технострелка** по направлению *web-разработка*. Мы представляем наше решение:
 - [Backend](https://github.com/dem3tr0/TravelRoutes-Backend)
 - [Frontend](https://github.com/shamash22351/TravelRoutes-Frontend)
 - [Обзор функционала (видеопрезентация)](https://youtu.be/4qzZWND0Emg?si=ScfxwsNw48OLRFJp)
# Инструкция по сборке
Инструкция приведена для разверстки web-приложения **TravelRoutes** локально, т. е. на вашем компьютере. Никакие сервисы не находятся удаленно, кроме сторонних API, использованных в разработке приложения.
## 1. Установка всех необходимых приложений
Список необходимых компонентов:
 - Python 3.13
 - PostgreSQL 17.3
 - pgAdmin4 (платформа для администрирования и настройки СУБД PostgreSQL)
 - Nginx 1.27.4
 - NodeJS 22.14.0 LTS

> ВАЖНО!!! Установка Nginx и репозиториев веб-приложения выполняется в выделенную папку на вашем устройстве   
### 1.1 Установка Python
Скачайте и установите [Python](https://www.python.org/downloads/) версии не менее 3.13. Проверить корректность установки можно командой в cmd (powershell): ``` python --version pip --version ``` 
#### 1.2 Установка PostgreSQL и PgAdmin
Скачайте и установите [PostrgeSQL](https://www.postgresql.org/download/) версии 16 и выше. Начните установку. При выборе дополнительных компонентов обязательно укажите `pgAdmin 4`.

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/PgAdmin.png)

Здесь установите пароль для доступа к вашим базам данных. Обязательно его сохраните, он вам позже понадобится.

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/Password.png)

Порт оставьте по умолчанию *5432*.

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/PostgrePort.png)

Убедитесь, что по следующему пути в интерфейсе PgAdmin (Server -> PostgreSQL -> Databases ) у вас имеется хотя бы 1 база данных:

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/CheckPgAdmin.png)
### 1.3 Установка Ngnix
Скачайте и установите [Ngnix](https://nginx.org/ru/download.html) версии не ниже 1.27.4.
### 1.4 Установка Node JS 
Скачайте и установите [Node JS](https://nodejs.org/en/download) версии не ниже 22.14.0 
### 1.5 Установка backend и frontend частей приложения 
В самом конце скачайте frontend и backend части нашего приложения с GitHub как zip архивы. Ссылки на репозитории указаны в самом начале файла `README.md`.
## 2. Настройка PostgreSQL
Сейчас мы должны настроить базу данных и Django проект для корректной работы с ней.
### 2.1 Содание базы данных
Снова откроем `pgAdmin`. Получим доступ к нашему серверу PostgreSQL, введя пароль. Далее создадим новую базу данных.

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/CreatingDB1.png)

Вводим любое название, допустим `travelrts`.

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/CreatingDB2.png)

Остальные настройки оставляем по умолчанию, они на работу нашего проекта никак не влияют.

### 2.2 Настройка Django для доступа к базе данных
Перейдем в директорию `app`, находящуюся по такому пути `путь_к_папке_проекта\TravelRoutes-Backend\app`. Там находим файл `settings.py`. Откроем его в каком-то редакторе кода.

На 70 строке находим параметры подключения к базе данных:
>P.S. Сейчас в редакторе большая часть кода может подсвечиваться красным, что говорит нам об ошибках, но это нормально. Мы это исправим в последующих пунктах.
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'travelrts', 
        'USER': 'postgres',
        'PASSWORD': 'admin',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```
 - `'ENGINE'` - драйвер, определяющий с какой базой данных мы будем работать. В этой строчке ничего не меняем.

 - `'NAME'` - название базы данных, которую будет использовать Django. Ставим то, что указывали при создании в пункте 2.1.

 - `'USER'` - пользователь или владелец этой базы данных. Оставляем `postgres`, если ничего не меняли при создании базы данных.

 - `'PASSWORD'` - очевидно, пароль доступа к нашей базе данных. Указываем ваш пароль

 - `'HOST'` - хост, на котором размещена база данных. Так как мы устанавливаем все локально, оставляем `localhost`.

 - `'PORT'` - порт, по которому доступна база данных. Если мы ничего не меняли при установке базы данных, то оставляем 5432, иначе указываем тот порт, что указали при установке.  


## 3. Установка компонентов для backend и frontend
Приступим к настройке самих проектов backend'а и frontend'а.
### 3.1 Компоненты React приложения
Для корректной работы frontend'а мы должны установить все компоненты с помощью пакетного менеджера npm. Перейдем в корневую папку фронтенд приложения:
```powershell
cd TravelRoutes-Frontend
```
Устанавливаем компоненты (команды выполняем последовательно):
```powershell
npm install
```
```powershell
npm install bootstrap
```
```powershell
npm install react-router-dom
```
### 3.2 Компоненты Django приложения
Опять же для корректной работы backend'a мы должны установить все компоненты, только уже с помощью встроенного пакетного менеджера pip. Перейдем в корневую папку бэкенд приложения:

> P.S Так как мы в прошлом пункте находились в корневой папке фронтенд приложения, мы должны вернуться в корневую папку всего проекта, выполнив дважды эту команду: 
>```
>cd ..
>```

Перейдем в корневую папку бэкенда:
```powershell
cd TravelRoutes-Backend
```
Установим и активируем виртуальную среду python:
```powershell
python -m venv .venv
```
Немного подождав, в директории где установлен проект появится папка виртуального окружения python с названием `.venv`. Ее активация отличается взависимости от исполняющего терминала (`cmd`, `powershell` и т. д.). Для powershell:
```powershell
.venv\Scripts\activate
```
Если все прошло успешно мы увидим измененную строку ввода:
```powershell
(.venv) PS E:\TravelRoutes\TravelRoutes-Backend>
```
Теперь установим все библиотеки. Мы подготовили файл `requirements.txt` со всеми библиотеками и их версиями, находящийся в корневой папке бэкенда. Выполним установку:
```powershell
pip install -r requirements.txt
```
Это может занять какое-то время.
## 4. Запуск frontend'а и backend'a
Приступим к запуску frontend'a и backend'a
### 4.1 Применение миграций
Перед запуском бэкенда нам нужно применить миграции к базе данных, то есть, чтобы создались необходимые таблицы для корректной работы.
Перейдем в корневую папку бэкенда:
```powershell
cd TravelRoutes-Backend
```
Последовательно выполним команды для миграции:
```powershell
python manage.py makemigrations
```
```powershell
python manage.py migrate
```
### 4.2 Запуск backend
Перейдем в корневую папку бэкенда:
```powershell
cd TravelRoutes-Backend
```
Запустим бэкэнд:
```
python manage.py runserver
```
> ВАЖНО!!! Не закрывайте терминал с введёнными командами, без него приложение работать не будет!
### 4.3 Запуск frontend
Запустим **НОВЫЙ** терминал. Перейдем в корневую папку фронтэнда:
```powershell
cd TravelRoutes-Frontend
```
Запустим фронтенд:
```
npm start
```
> ВАЖНО!!! Не закрывайте терминал с введёнными командами, без него приложение работать не будет!
## 5. Настройка Nginx
Как уже было сказано в начале инструкции, все веб-приложение запускается локально, поэтому для доступа к его обеим частям (backend, frontend) по одному домену нам нужно настроить обратное проксирование. Для этого мы воспользуемся Nginx.

Перейдем в папку, где мы установили Nginx:

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/Nginx1.png)

Откроем папку conf

![](https://github.com/dem3tr0/TravelRoutes/raw/main/Images/NginxConf.png)

Откроем файл `nginx.conf` как текстовый файл в любом текстовом редакторе, например в Visual Studio Code (хотя можно и в обчном блокноте). Вместо того, что написано в этом файле вам надо вставить конфигурацию ниже: 
```nginx
worker_processes 1;
events  {
	worker_connections  1024;
}
http  {
	include mime.types;
	default_type application/octet-stream;
	sendfile on;
	keepalive_timeout  65;
	
	upstream backend {
		server localhost:8000;
	}
	
	upstream frontend {
		server localhost:3000;
	}
	
	server  {
		listen  80;
		server_name localhost;
		location /api/ {
			proxy_pass  http://backend;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header X-Forwarded-Proto $scheme;
		}

		location / {
			proxy_pass  http://frontend;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header X-Forwarded-Proto $scheme;
		}
		
		error_page  500  502  503  504 /50x.html;

		location = /50x.html {
		root html;
		}
	}
}
```
Далее зайдем обратно в терминал и перейдем в корневую папку nginx:
```powershell
cd ПолныйПутьКПапкеПроекта\nginx
```

И заупстим файл `nginx.exe` командой:
```powershell
nginx.exe
```

Теперь мы можем проверить работу, зайдя в браузер и перейдя по адресу: `http://localhost`
