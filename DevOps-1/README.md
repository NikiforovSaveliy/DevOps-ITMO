<h1 align="center"> Лабораторная работа №1 </h1>


<h2 align="center">Постановка задачи</h2>

Перед нами стоит задача передать файл с комьютера А на компьютер Б при помощи терминала на компьютере Ц.

---

<h2 align="center">Выполнение работы</h2>

1. На комьютере A создадим текстовый файл.

```bash
 echo "Hello world!" >> file.txt
 ```

2. Установка ssh сервера на машину.

```bash
    sudo apt update && apt install openssh-server -y
```

![Установка сервера](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/install_server.jpg)

3. Запуск ssh сервера.

```bash
service ssh start
```
Также проверим запустился ли сервис.

```bash
service ssh status
```

![Проверка процесса](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/ssh_status.jpg)

4. Получение ip адрессов машин в локальной сети.

```bash
apt install net-tools && ifconfig
```
Получили IP адресс машины в локальной сети.

![Адресс в локальной сети](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/ip_allocation.jpg)

5.  Подключение к машине по ssh

```bash
ssh {username}@{address}
```
![Вход по ssh]()
Вводим пароль для входа в систему

![Вид на терминал на удаленном комьютере](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/logged_terminal.jpg)

Получили доступ к терминалу.

6. Повтор шагов 2-4 для машины Б

7. Передача файла.

Для передачи файла `test.txt` с комьютера А на Б при помощи терминала на машине Ц воспользовались утилитой `scp`.
```bash
scp {host_1}@{address_1}:{path_to file} {host_2}@{address_2}:{path_to_file}
```

Имеем данный вывод в терминал:

![terminal_exec](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/scp_success.jpg)

8. Проверка наличия файлов.

![Success](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/check_file.jpg)

Файл `test.txt` был успешно передан меджду двумя удаленными хостами
<details>

<summary> Вариант со звездочкой </summary>

1. Сгенерируем RSA ключ.
    ```bash
    ssh-keygen -t rsa -b 2048 -N "" -f ".ssh/"
    ```

2. Скопируем публичные ключи на хосты.
    ```bash
    ssh-copy-id
    ```
    ![ssh_copy_id](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/ssh_copy_id.jpg)

3. Попробуем подключиться
    ```bash
    ssh {user}@{address}
    ```
    В итоге пароль для входа в систему не потребовался.
    ![enter_ssh_copy_id](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/ssh_enter_copy_id.jpg)

4. Повтор действий из пункта 7
    ![p_7](https://github.com/NikiforovSaveliy/DevOps-ITMO/blob/main/DevOps-1/p7_again.jpg)
</details>

----

<h2 align="center"> Вывод </h2>

В результате выполнения данной лабораторной работы удалось совершить подключение по протоколу SSH для получения доступа над удаленной машиной и передачи файлов.


