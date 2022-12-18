## Цели

Вы сможете создать каталог, который будет доступен всем участникам определенной группы.

## Задачи

В этом упражнении вы будете использовать разрешения файловой системы для создания каталога, в котором все участники определенной группы смогут добавлять и удалять файлы.

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab perms-cli start`. Этот подготовительный сценарий создает группу с именем *consultants* и двух пользователей ― *consultant1* и *consultant2*.

```
[student@workstation ~]$ lab perms-cli start
```

1.	С **workstation** с помощью команды `ssh` войдите на **servera** как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	Переключитесь на пользователя *root*, указав пароль *redhat*.

    ```
    [student@servera ~]$ su -
    Password: redhat
    [root@servera ~]# 
    ```

3.	С помощью команды `mkdir` создайте каталог **/home/consultants**.

    ```
    [root@servera ~]# mkdir /home/consultants
    ```

4.	С помощью команды `chown` измените группу-владельца каталога **consultants** на *consultants*.

    ```
    [root@servera ~]# chown :consultants /home/consultants
    ```

5.	Убедитесь, что разрешения группы *consultants* позволяют ее участникам создавать файлы в каталоге **/home/consultants** и удалять их из него. Разрешения должны запрещать остальным пользователям доступ к файлам.

    5.1.	С помощью команды `ls` убедитесь, что разрешения группы *consultants* позволяют ее участникам создавать файлы в каталоге **/home/consultants** и удалять их из него.

    ```
    [root@servera ~]# ls -ld /home/consultants
    drwxr-xr-x.  2 root    consultants       6 Feb  1 12:08 /home/consultants
    ```

    Обратите внимание, что в настоящее время у группы *consultants* нет разрешения на запись.

    5.2.	С помощью команды `chmod` добавьте разрешение на запись для группы *consultants*.

    ```
    [root@servera ~]# chmod g+w /home/consultants
    [root@servera ~]# ls -ld /home/consultants
    drwxrwxr-x. 2 root consultants 6 Feb  1 13:21 /home/consultants 
    ```

    5.3.	С помощью команды `chmod` запретите остальным пользователям доступ к файлам в каталоге **/home/consultants**.

    ```
    [root@servera ~]# chmod 770 /home/consultants
    [root@servera ~]# ls -ld /home/consultants
    drwxrwx---. 2 root consultants 6 Feb  1 12:08 /home/consultants/
    ```

6.	Выйдите из оболочки *root* и переключитесь на пользователя *consultant1*. Пароль — *redhat*.

    ```
    [root@servera ~]# exit
    logout
    [student@servera ~]$ 
    [student@servera ~]$ su - consultant1
    Password: redhat
    ```

7.	Перейдите в каталог **/home/consultants** и создайте файл с именем **consultant1.txt**.

    7.1.	С помощью команды `cd` перейдите в каталог **/home/consultants**.

    ```
    [consultant1@servera ~]$ cd /home/consultants
    ```

    7.2.	С помощью команды `touch` создайте пустой файл с именем **consultant1.txt**.

    ```
    [consultant1@servera consultants]$ touch consultant1.txt
    ```

8.	С помощью команды `ls -l` отобразите владельца и группу-владельца, заданных по умолчанию для нового файла, а также разрешения файла.

```
[consultant1@servera consultants]$ ls -l consultant1.txt
-rw-rw-r--. 1 consultant1 consultant1 0 Feb  1 12:53 consultant1.txt
```

9.	Убедитесь, что все участники группы *consultants* могут редактировать файл **consultant1.txt**. Задайте для файла **consultant1.txt** группу-владельца *consultants*.

    9.1.	С помощью команды `chown` измените группу-владельца файла **consultant1.txt** на *consultants*.

    ```
    [consultant1@servera consultants]$ chown :consultants consultant1.txt
    ```

    9.2.	Используйте команду `ls` с опцией `-l`, чтобы отобразить нового владельца файла **consultant1.txt**.

    ```
    [consultant1@servera consultants]$ ls -l consultant1.txt
    -rw-rw-r--. 1 consultant1 consultants 0 Feb  1 12:53 consultant1.txt
    ```

10.	Выйдите из оболочки и переключитесь на пользователя *consultant2*. Пароль — *redhat*.

```
[consultant1@servera consultants]$ exit
logout
[student@servera ~]$ su - consultant2
Password: redhat
[consultant2@servera ~]$ 
```

11.	Перейдите в каталог **/home/consultants**. Убедитесь, что пользователь *consultant2* может добавлять содержимое в файл **consultant1.txt**. Выйдите из оболочки.

    11.1.	С помощью команды `cd` перейдите в каталог **/home/consultants**. С помощью команды `echo` добавьте текст в файл **consultant1.txt**.

    ```
    [consultant2@servera ~]$ cd /home/consultants/
    [consultant2@servera consultants]$ echo "text" >> consultant1.txt
    [consultant2@servera consultants]$ 
    ```

    11.2.	Выполните команду `cat`, чтобы убедиться, что текст был добавлен в файл **consultant1.txt**.

    ```
    [consultant2@servera consultants]$ cat consultant1.txt
    text
    [consultant2@servera consultants]$ 
    ```

    11.3.	Выйдите из оболочки.

    ```
    [consultant2@servera consultants]$ exit
    logout
    [student@servera ~]$ 
    ```

12.	Выйдите с servera.

```
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$ 
```

## Конец

На workstation запустите сценарий `lab perms-cli finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab perms-cli finish
```

Упражнение завершено.
