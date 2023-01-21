## Цель

Получение базовых навыков создания жестких и символьных ссылок между файлами.

## Задачи

В этом упражнении вы создадите жесткие ссылки и символьные ссылки, а затем сравните результаты.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab files-make start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста servera в сети и создает на servera файлы и рабочие каталоги.

```shell
[student@workstation ~]$ lab files-make start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации, поэтому пароль не требуется.

    ```shell
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	Создайте жесткую ссылку **/home/student/backups/source.backup** для существующего файла **/home/student/files/source.file**.

    2.1.	Просмотрите количество ссылок для файла /home/student/files/source.file.

    ```shell
    [student@servera ~]$ ls -l files/source.file
    total 4
    -rw-r--r--. 1 student student 11 Mar  5 21:19 source.file
    ```

    2.2.	Создайте жесткую ссылку с именем **/home/student/backups/source.backup**. Свяжите ее с файлом **/home/student/files/source.file**.

    ```shell
    [student@servera ~]$ ln /home/student/files/source.file \
    /home/student/backups/source.backup
    ```

    2.3.	Проверьте количество ссылок для исходного файла **/home/student/files/source**.file и нового связанного файла **/home/student/backups/source.backup**. Количество ссылок должно быть равно 2 для обоих файлов.
    
    ```shell
    [student@servera ~]$ ls -l /home/student/files/
    -rw-r--r--. 2 student student 11 Mar  5 21:19 source.file
    [student@servera ~]$ ls -l /home/student/backups/
    -rw-r--r--. 2 student student 11 Mar  5 21:19 source.backup
    ```

3.	Создайте символьную ссылку **/home/student/tempdir**, указывающую на каталог **/tmp** на хосте **servera**.

    3.1.	Создайте символьную ссылку **/home/student/tempdir** и свяжите ее с **/tmp**.

    ```shell
    [student@servera ~]$ ln -s /tmp /home/student/tempdir
    ```

    3.2.	С помощью команды `ls -l` проверьте созданную символьную ссылку.

    ```shell
    [student@servera ~]$ ls -l /home/student/tempdir
    lrwxrwxrwx. 1 student student  4 Mar  5 22:04 /home/student/tempdir -> /tmp
    ```

4.	Выйдите с **servera**.

```shell
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```

## Конец

На **workstation** запустите сценарий `lab files-make finish`, чтобы закончить упражнение. Этот сценарий удаляет все файлы и каталоги, созданные на servera во время упражнения.

```shell
[student@workstation ~]$ lab files-make finish
```

Упражнение завершено.
