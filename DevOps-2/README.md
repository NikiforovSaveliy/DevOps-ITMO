<h1 align="center">Лабораторная работа №2</h1>

<h2 align="center">Постановка задачи</h2>
В данной лабораторной работе нам необходимо написать два Dockerfile – плохой и хороший. Плохой должен запускаться и работать корректно, но в нём должно быть не менее 3 “bad practices”. В хорошем Dockerfile они должны быть исправлены. Также нужно описать все плохие практики из кода Dockerfile и то, как они были исправлены в хорошем Dockerfile. Помимо этого необходимо привести в пример две плохие практики по использованию хорошего контейнера.

<h3 align="center">Плохие практики</h3>

   1. Использование универсального родитеслького образа.
      
      В нашем случае в плохом `Dockerfile` будет использоваться родительский образ `ubuntu`.

   2. Использование не стогой версии родительского образа.
      
      Использование родительского образа с тэгом `latest` может привести к непредвиденным багам и проблемам при дальнешей эксплуатации контейнера.

   3. Запуск контейнера от имени `root` пользователя
      
      Запуск контейнеров от имени `root` может привести к проблемам с безопасностью, проблем разграничений прав доступа.

   4. Копирование всех файлов проекта.

      При использовании python проекта могут существовать файлы, которое не целесообразно копировать в docker-образ.
 Например, файлы окружения (`.env`), файлы системы git (`.git`, `.submodules`) и т.д.

<h2 align="center">Выполнение работы</h2>

<h3 align="center">Подготовка к написанию Dockerfile</h3>

Сначала создадим папку с файлом `requirements.txt`, в котором запишем требуемые библиотеки Python.

![Файл 'requirements.txt'](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/requirements.jpg)

Далее добавим в эту директорию файл `server.py` с простым веб-приложением.

![Файл 'server.py'](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/server_py.jpg)

<h3 align="center">Плохой Dockerfile</h3>

Напишем инструкции для плохого Dockerfile. Загружает базовый образ Ubuntu, устанавливаем Python3 и pip, копируем файл `requirements.txt` из директории проекта в рабочую директорию `/app` внутри образа, устанавливаем необходимые зависимости Python, копируем все файлы из директории проекта внутрь образа и задаем команду для запуска контейнера.

![Заполенение bad Dockerfile](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/изображение_2023-11-01_181416451.png)

Команда для сборки плохого dockerfile:
```bash
docker build -f BadDockerfile . -t bad_docker
```

Получаем:

![Вывод при билде плохого dockerfile](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/build_docker.jpg)

Комманда для запуска плохого контейнера

```bash
docker run --name bad -p 80:80 -d bad_docker
```
Переходим на хосте по 0.0.0.0:80 и видим:

![Результат запуска плохого контейнера](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/hello_world_web.jpg)

<h3 align="center">Хороший Dockerfile</h3>

Для хорошего dockerfile уместно использовать образ Python, вместо образа ubuntu. Заменяем строчку `FROM ubuntu` на `FROM python:3.11-slim-bookworm` и теперь нам не нужно подключать `pip`, потому что образ Python в себе включает эту библиотеку. Далее с помощью команды `pip install` устанавливаем зависимости из `requirements.txt` внутри контейнера. Инструкция `COPY . .` копирует все файлы и папки из вашего локального контекста сборки в контейнер. Последнюю строчку не меняем. В итоге получаем dockerfile с вот этим содержимым:

![Вывод при билде хорошего dockerfile](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/good_dockerfile.jpg)

Собираем образ вот этой командой:

```bash
docker build -f GoodDockerfile . -t good_docker
```

![Вывод при билде хорошего dockerfile](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/build_good_docker.jpg)

Запускаем контейнер этой командой:

```bash
docker run --name good -p 80:80 -d good_docker
```

И видим следующий результат:

![Результат запуска хорошего контейнера](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/hello_world_web.jpg)

<details>
<summary> Вариант со звездочкой</summary>
   
   
   1. Установка minikube (для MacOS)

   ```bash
      brew install minikube
   ```

   2. Запуск кластера 

   ```bash
   minikube start && minikube dashboard
   ```

   3. Пуш образа в репозиторий Dockerhub
   
   ```bash
   docker push waswel/dev-ops-itmo:
   ```

</details>
---

<h2 align="center">Вывод</h2>
