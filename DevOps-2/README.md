<h1 align="center">Лабораторная работа №2</h1>

<h2 align="center">Постановка задачи</h2>
В данной лабораторной работе нам необходимо написать два Dockefile. Один содержит плохие практики написания. Второй содержит разрешения плохих практик. Также необходимо описать плохие практики использования написанного контейнера.

---

<h2 align="center">Выполнение работы</h2>

<h3 align="center">Плохой докерфайл</h3>

В плохом докерфайле были выбранных следующие плохие практики:

1. Запуск контейнера от имени рут пользвоателя 

    Запуск контейнерных приложений от имени `root` представляет собой риск для безопасности.

2. Использование неправильного родительского образа (использование образа Ubuntu вместо python)

    Использование обощенного родительского образа делает Dockerfile запутаным и трудным для понимания

    ```Dockerfile
        #before
        FROM ubuntu
        #after
        FROM python:3.11-bookworm-latest
    ```

3. Использование большого образа, вместо малого.

    Из-за использования большого родителького образа возникает проблема эффективности работы
 
    Решение:
    ```Dockerfile 
        #before
        FROM python:3.11-bookworm
        #after
        FROM python:3.11-slim-bookworm
    ```

<h3 align="center">Создание образа и запуск контейнера</h3>

Комманда для сборки хорошего и плохого образа
```bash
docker duild -f BadDockerfile . -t bad_docker
```
```bash
docker duild -f GoodDockerfile . -t good_docker
```

Комманда для запуска контейнеров

```bash
docker run -p 80:80 {tag_of_container}
```

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
