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


<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/requirements.jpg"/>
</p>


Далее добавим в эту директорию файл `server.py` с простым веб-приложением.

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/server_py.jpg"/>
</p>

<h3 align="center">Плохой Dockerfile</h3>

Напишем инструкции для плохого Dockerfile. Загружает базовый образ Ubuntu, устанавливаем Python3 и pip, копируем файл `requirements.txt` из директории проекта в рабочую директорию `/app` внутри образа, устанавливаем необходимые зависимости Python, копируем все файлы из директории проекта внутрь образа и задаем команду для запуска контейнера.


<img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/изображение_2023-11-01_181416451.png"/>


Команда для сборки плохого dockerfile:
```bash
docker build -f BadDockerfile . -t bad_docker
```

Получаем:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/build_docker.jpg"/>
</p>

Комманда для запуска плохого контейнера

```bash
docker run --name bad -p 80:80 -d bad_docker
```
Переходим на хосте по 0.0.0.0:80 и видим:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/hello_world_web.jpg"/>
</p>


<h3 align="center">Хороший Dockerfile</h3>

Для хорошего dockerfile уместно использовать образ Python, вместо образа ubuntu. Заменяем строчку `FROM ubuntu` на `FROM python:3.11-slim-bookworm` и теперь нам не нужно подключать `pip`, потому что образ Python в себе включает эту библиотеку. Далее с помощью команды `pip install` устанавливаем зависимости из `requirements.txt` внутри контейнера. Инструкция `COPY . .` копирует все файлы и папки из вашего локального контекста сборки в контейнер. Последнюю строчку не меняем. В итоге получаем dockerfile с вот этим содержимым:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/good_dockerfile.jpg"/>
</p>

Собираем образ вот этой командой:

```bash
docker build -f GoodDockerfile . -t good_docker
```

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/build_good_docker.jpg"/>
</p>

Запускаем контейнер этой командой:

```bash
docker run --name good -p 80:80 -d good_docker
```

И видим следующий результат:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/hello_world_web.jpg"/>
</p>

<h3 align="center">Плохие практики по использованию этого контейнера</h3>

   1. Использовать докер, как виртуальную машину. Нужно четко понимать, где нужно использовать докер, а где виртуальную машину, иначе выполнение требуемых задач будет небезопасным и неэффективным.
      
   2. Создавание образа с помощью команды `docker commit`. Использование этой команды может привести к невозможности вернуться к предыдущему состоянию контейнера и к отсутствию автоматизации, что трудоемко и неэффективно.

<h2 align="center">Вывод</h2>

В ходе данной лабараторной работы были написаны 2 dockerfile, которые успешно заработали.

<details>
<summary> Вариант со звездочкой </summary>

   <h2 align="center">Постановка задачи</h2>

   Запустить Kubernetes кластер (подойдёт minikube или kind). Запустить контейнеры внутри этого кластера, при этом всё должно быть описано кодом. В минимальном варианте должен быть deployment и service. Приложение, работающее внутри контейнера внутри кластера должно открываться локально в браузере.

<h2 align="center">Выполнение работы</h2>

<h3 align="center">Установка инструментов</h3>

   Для начала, установим minikube - инструмент, который позволяет нам запустить и управлять небольшим, локальным кластерам Kubernetes. Для этого воспользуемся данной командой:
   
   ```bash
      curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube
   ```

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/setup_minikube.jpg"/>
</p>

Далее, выполняем эти команды, чтобы исполняемы файл Minikube был доступен из любой директории:

    ```sudo mkdir -p /usr/local/bin/
       sudo install minikube /usr/local/bin/
    ```
Запустим Minikube вот этой командой `minikube start --vm-driver=<docker>`. Здесь мы использовали драйвер docker для виртуализации. После выполнения этой команды сможем работать с Kubernetes, не создавая полноценный удаленный кластер Kubernetes. Проверим, что все работает командой `minikube status`:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/minikube_status.jpg"/>
</p>

Теперт установим kubectl. Для этого последовательно выполним следующие команды:

   ```curl -LO https://dl.k8s.io/release/`curl -LS https://dl.k8s.io/release/stable.txt`/bin/linux/amd64/kubectl
      chmod +x ./kubectl
      sudo mv ./kubectl /usr/local/bin/kubectl
   ```

Убеждаемся, что все правильно установили, вводя команду `kubectl version --client`:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/kubectl_version.jpg"/>
</p>


<h3 align="center">Создание и настройка Deployment и Service</h3>

Создадим файл `Deployment.yaml` и наполним его следующим:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/deployment_inside.jpg"/>
</p>

Разберем этот файл подробнее. Мы создаем объект развертывания, поэтому поле `apiVersion` установлен на `apps/v1` - версия `API Kubernetes`, и `kind` установлен на `Deployment`. В разделе `metadata` содежится имя развертывания - `flask-deployment` и добавлена метка `app`. 
В разделе `spec` описываются настройки для развертывания, а именно: 
   1. `selector`, который указывает, что будет контролироваться под с меткой `app: flask`.
   2. `template`, где определено описание пода
   3. `containers` - здесь определен контейнер с именем `flask-server`, который будет создан из образа `waswel/good_docker`.

Чтобы получить доступ к поду, необходимо написать еще один файл - `Service.yaml`:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/system_inside.jpg"/>
</p>


Этот файл конфигурации описывает сервис с именем `flask-service`, который будет доступен снаружи кластера через порт 80, и все входящие запросы на этот порт будут перенаправляться на поды, помеченные как `app: flask` на их порт 80, где, работает flask-приложение.

Теперь создадим `Makefile.txt`, с помощью которого автоматизируем и упростим процесс работы с minikube. Его содержимое представлено ниже:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/makefile_inside.jpg"/>
</p>


В данном Makefile есть несколько целей, каждая из которых выполняет определенное действие:

1. `run`: Запускает сервис, который развернут в кластере Minikube. Он использует команду `minikube service flask-service` для открытия сервиса в браузере.
2. `apply-deploy`: Применяет конфигурацию развертывания из файла `Deployment.yaml` в кластере с помощью команды `kubectl apply`.
3. `apply-service`: Применяет конфигурацию службы из файла `Service.yaml` в кластере с помощью команды `kubectl apply`.
4. `start`: Запускает локальный кластер Minikube с помощью команды `minikube start`.
5. `dashboard`: Запускает веб-интерфейс Kubernetes Dashboard для управления и мониторинга кластера Minikube с помощью команды `minikube dashboard`.

<h3 align="center">Результат</h3>

Использовав команду `make apply-deploy`, мы применим Deployment. Далее пишем `make apply-service` и применяем Service. Теперь подключаемся к сервису flask-service используя следующий синтаксис: `make run`:

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/run_port_forwarding.png"/>
</p>

<p align="center">
  <img src="https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-2/Pictures/result.jpg"/>
</p>


Все работает.

</details>



