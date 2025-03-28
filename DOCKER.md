[[CONTAINERIZATION]] 

**После полного выполнения процесса внутри контейнера, контейнер автоматически остановится.**

## Commands

- `docker ps -a` - показывает список запущенных и остановленных контейнеров. Без `-a` будем видеть только запущенные контейнеры.
- `docker images` - показывается список локальных образов.
- `docker run <images>` - запуск контейнера. К примеру `docker run hello-world`. Докер сначала ищет образ локально, если не находит, то он ищет на DockerHub -> скачивает и сохраняет его локально и запускает контейнер на основании этого образа. Каждый раз запускается **новый** контейнер.

Можно к названию образа добавить тег и тогда будет скачана определенная версия этого образа, пример -> `docker run hello-world:latest` (скачивает (если не скачан) и запускает 
контейнер) / `docker pull hello-world:latest` - скачивает образ.

- `docker rm <CONTAINER ID/CONTAINER NAME>` - удаление контейнера
	Пример - `docker rm b01832b9dc00`
- `-i` - интерактивный, `-t` - терминал.
- `docker container prune` - удаляет все остановленные контейнеры.
- `--rm` - автоматические удаляет остановленный контейнер.
	Пример: `docker run -it --rm busybox`. Удалит его после завершения процесса внутри контейнера. В `docker ps -a` его не будет.
- `docker run -d nginx` - запуск контейнера в фоновом режиме.
- `docker container inspect <CONTAINER ID/CONTAINER NAME>` - вывод всех деталей о контейнере.
- `docker stop <CONTAINER ID/CONTAINER NAME>` - остановка контейнера.
- `docker kill <CONTAINER ID/CONTAINER NAME` - процесс внутри контейнера останавливается моментально.
- `docker exec <CONTAINER ID/CONTAINER NAME> <NAME PROCESS>`. 
	Пример: `docker exec -it 311c5a02feeb bash`
- `hostname -i` - посмотреть ip адрес
- `docker run --name <CUSTOM NAME> <images>` - назначить кастомное имя контейнеру. 
	Пример:  `docker run -d --name my_ngnix nginx`.
- `docker logs <CONTAINER ID/CONTAINER NAME>` - посмотреть логи контейнера.
- `docker tag <CONTAINER ID> <NEW NAME:NEW VERION>` - переименовать образ (и задать версию). Пример: `docker tag 7d0e39bc3b1f ubuntu_docker:v1` 
- `docker image inspect <CONTAINER ID>` - посмотреть информацию об образе
	Пример `docker image inspect ubuntu_docker:v1`
- `docker volume ls` - посмотреть volumes
	

## nginx

**nginx работает на 80 порту.**

`cd /usr/share/nginx/html` - пусть внутри контейнера, где находится файл `index.html`.

По-стандарту нельзя подключиться к 80 порту со своего компьютера. Для того, чтобы это сделать, нужно сделать **mapping портов**.

### ports mapping

Для того, чтобы открыть доступ к какому-то сервису внутри контейнера, который работает на определенном порту, необходимо использовать опцию `-p` (publish)
С помощью этой опции можно открыть определенный порт на своем компьютере и пробросить его на другой порт внутри контейнера. Эту проброску делает докер.
`docker run -p 8080:80 nginx`. (`8080 - внешний порт, 80 - внутренний порт, внутри контейнера`). 

```shell
docker run -p <HOST_PORT>:<CONTAINER_PORT> <IMAGE>
```

После этого я смогу открыть веб-браузер, ввести `localhost 8080` и я попаду внутрь контейнера и docker **перебросит это соединение с порта 8080 компьютера на порт 80 внутри контейнера**.

### mapping томов

Mapping томов позволяет привязывать папки со своего компьютера к папкам в контейнере.

`docker run -v ${PWD}:/usr/share/nginx/html nginx`
	`-v` - подключение томов
	`${PWD}` - путь к локальной папке. `${PWD}` - переменная. Можно также заменить на абсолютный путь к папке.
	`/usr/share/nginx/html` - путь к папке внутри контейнера (тут находится файл `index.html`)

## Запуск кастомной html страницы через 80 порт nginx

```shell
docker run -v ${PWD}:/usr/share/nginx/html -p 8080:80 -d --rm nginx
```
`${PWD}` = `/home/temaantonov11/docker_/nginx` (*в моем случае*)

#### Перенос строк в shell командах

```shell
docker run \
	--name my_nginx \
	-v ${PWD}:/usr/share/nginx/html \
	-p 8080:80 \
	-d \
	--rm \
	nginx
```

## Docker volumes

Docker volumes - механизм в docker, обеспечивающий **persisting data (сохраняющиеся данные)**.

По факту, **volume** - *примонтированная директория с хоста в контейнер*. Тем самым обеспечивается сохранение данных. (А также их подкачка, к примеру как с развертыванием кастомной html страницы в nginx - за счет того, что у нас получается по сути одна директория). 

Это нужно потому, что в **контейнерах** используется **временная файловая система** (то есть после остановки или удаления контейнера она полностью удаляется).


`docker volume ls` - просмотр volumes
`docker volume create <VOLUME NAME>` - создание volume
`docker volume rm <VOLUM_NAME` - удаление volume
### Способы
##### Host volumes (mapping томов)

```shell
docker run -v /opt/mysql_data:/var/lib/mysql mysql
```

`opt/mysql_data` будет смонтирована в `/var/lib/mysql`.

##### Anonymous volumes

```shell
docker run -v /var/lib/mysql mysql
```

![[Pasted image 20250203004502.png]]

##### Named volumes

```shell
docker run -v mysql_data:/var/lib/mysql mysql
```

![[Pasted image 20250203004543.png]]

### Dockerfile and volumes

В `Dockerfile` нельзя напрямую создавать или управлять томами, так как `Dockerfile` предназначен для сборки образов, а тома - runtime-концепция, которая используется при запуске контейнеров. Однако в `Dockerfile` можно указать **точки монтирования томов** с помощью инструкции `VOLUME`. Это позволит определить, какие директории в контейнере будут использоваться для хранения данных, которые должны сохраняться или быть доступны на хосте.
Инструкция `VOLUME` автоматически **создает анонимный том**, при других сценариях использования томов она несет больше **информативный характер** (для того, чтобы понимать, в какую директорию контейнера нужно монтировать).

```dockerfile
# показваем, что build будет томом (сюда будет монтироваться директория из хоста)
VOLUME /app/build
```

1. При сборке образа Docker создает точку монтирования для указанной директории
2. При запуске контейнера Docker автоматически создает анонимный том (если том не указан явно) и монтирует его в указанную директорию.
#### Использование именованного тома

```shell
docker run -v build_artefacts:/app/build my_building_docker
```

`build_artefacts` - именованный том, который будет смонтирован в `/app/build` внутри контейнера.
Если том `build_artefacts` не существует, то он будет автоматически создан.
#### Использование анонимного тома

```shell
docker run my_bilding_docker
```

Анонимный том будет смонитрован в `/app/build`.

## Images

Образ есть смысл создавать тогда, когда **нет подходящего** на *DockerHub*. 
При создании собственного образа необходимо указывать **базовый образ**, который берется за основу. Все слои нового образа добавляются к слоям базового образа.

### Этапы создания образа

1) Создание `Dockerfile` (он необходим для создания образа, **1 dokerfile = 1 image**)
2) Обычно `Dockerfile` помещают в корень папки приложения.
3) `Dokerfile` содержит инструкции по созданию образа. Каждая инструкция будет создавать новый слой.
4) При создании образа можно указать имя и тег для образа. Если не указать имя то docker сгенерирует id для образа автоматически. А если не указать тег, то docker автоматически добавит тег `latest`.
5) На основании готового образа можно создавать контейнеры.

#### Пример `Dockerfile`

```dockerfile
FROM python:alpine -> базовый образ, :alpine - это версия (тег).
WORKDIR /app -> создается рабочая категория внутри образа
COPY . . -> копируем все файлы из локальной текущей папки в WORKDIR внутри контейнера
CMD ["python", "main.py"] -> указывает на то, какая команда будет выполнения при создании контейнера на основании образа.
```

`WORKDIR` - создает и **устанавливает текущую рабочую директорию** для всех последующих команд.
**Рекомендуется всегда создавать папку внутри образа для приложения.** В любом контейнере есть список файлов и папок в **корневой директории, туда не рекомендуется** помещать файлы и папки приложения, потому что можно случайно перезаписать системные папки.

`COPY . .` -> копируем все файлы из локальной текущей папки в `WORKDIR` внутри контейнера. После выполнения этой команды docker перейдет в папку `WORKDIR` (`/app` для нашего примера) и папка `WORKDIR` станет текущей папкой для всех последующих команд.

`CMD ["python", "main.py"]` -> указываем команду, которая должна будет выполнена при создании контейнера. В примере будет создан процесс `python` с переданным ему аргументом `main.py`. Файл `main.py` должен быть в рабочей директории `WORKDIR` (`/app`). Эта команда может быть перезаписана в терминале при создании контейнера.

`ENTRYPOINT [" "]` -> указываем команду, которая будет выполнена в контейнере. **Но она неизменяемая**. 

`RUN <command>` -> выполнить команду. Пример: `RUN pip install pymongo`.

#### Создание образа

Сохраняем `Dockerfile` и собираем образ:

**LEGACY METHOD:**
```shell
docker build .
```

**MODERN METHOD:**
```shell
docker buildx build .
```

`.` - текущая директория. Докер будет искать dokerfile в текущей директории и если он будет найден, то запуститься процесс создания нового образа.

##### Добавление имени к образу

```shell
docker buildx build . -t my-calendar:4.1.3
```

`-t` - добавление имена и тега для образа. Тег указывается после `:`. Если не добавлять тег, то докер автоматически добавит тег `latest`. 

#### Удаление образа

```shell
docker rmi <IMAGES ID/IMAGES NAME>
```


## Docker Compose

*Императивный подход* - ввод команды в терминал для запуска какого-то действия.
*Декларативный подход* - автоматизация процесса, в файле описываем ожидаемый результат. Абстрагируемя от команд (`docker run`, `docker build`) и создаем функции более высокого уровня, и с помощью этих инструкций мы описываем конечную цель.

### Формат yaml
##### Список

```yaml
fruits:
	- banana
	- apple
	- orange
```
##### Словарь

```yaml
pen:
	color: yellow
	model:
		type: pen
		material: plastic
	price: 2
```

В одном `yaml` файле можно комбинировать словари и списки.
`yaml` файлы выступают в качестве конфигурационных файлов.
### Преимущества docker compose

- *Декларативный* подход к созданию контейнеров.
- Все необходимые контейнеры **запускаются одной командой**.
- Автоматическое создание необходимых образов на основании `Dockerfile` каждого приложения.
- Автоматическое создание **изолированной сети** (и подключает все созданные контейнеры к этой изолированной сети) для взаимодействия контейнеров. Отдельная сеть для некоторых контейнеров позволяет изолировать их от других контейнеров, которые работают на этом же docker host. 
- Благодаря **DNS** возможно взаимодействие между контейнерами, используя **имена сервисов**. 
### Пример `docker-compose.yml`

```yml
version: '3'

services:
	app:
		build: ./app
	mongo:
		image: mongo
```

Создается два контейнера под приложение (название сервиса: `app`) и под базу данных (название сервиса: `mongo`).

`build` - подразумевает создание кастомного образа на основе `Dockerfile`, который находится в папке `/app`. (`build: <dir, where there is Dockerfile>`)


### Запуск контейнеров 

**Запуск**: `docker compose up` (использование `Docker Compose V2`).
**Запуск в фоновом режиме:** `docker compose up -d`.
**Остановка и удаление:** `docker compose down`.
**Запуск и ребилд:** `docker compose up -d --build`. Это необходимо делать каждый раз, когда изменяем какое-либо приложение. 
