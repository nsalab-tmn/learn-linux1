## Цели

Получение базовых навыков управления разрешениями по умолчанию файловой системы


## Задачи

В этом упражнении вы будете управлять разрешениями для новых файлов, созданных в каталоге, используя настройки пользовательской маски и разрешение **setgid**.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab perms-default start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста servera в сети. Сценарий также создает группу *operators* и пользователя *operator1* на **servera**.

```
[student@workstation ~]$ lab perms-default start
```

1.	С помощью команды `ssh` войдите на servera как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	С помощью команды `su` переключитесь на пользователя *operator1*, указав пароль *redhat*.

    <details>
    <summary>Показать решение</summary>
    ```
    [student@servera ~]$ su - operator1
    Password: redhat
    [operator1@servera ~]$ 
    ```
    </details>

3.	С помощью команды `umask` отобразите значение пользовательской маски по умолчанию для пользователя *operator1*.

    <details>
    <summary>Показать решение</summary>
    ```
    [operator1@servera ~]$ umask
    0002
    ```
    </details>

4.	Создайте каталог с именем **/tmp/shared**. В каталоге **/tmp/shared** создайте файл с именем **defaults**. Просмотрите разрешения по умолчанию.

    <details>
    <summary>Показать решение</summary>

    4.1.	С помощью команды `mkdir` создайте каталог **/tmp/shared**. С помощью команды `ls -ld` отобразите список разрешений для нового каталога.

    ```
    [operator1@servera ~]$ mkdir /tmp/shared
    [operator1@servera ~]$ ls -ld /tmp/shared
    drwxrwxr-x. 2 operator1 operator1 6 Feb  4 14:06 /tmp/shared
    ```

    4.2.	С помощью команды `touch` создайте файл с именем **defaults** в каталоге **/tmp/shared**.

    ```
    [operator1@servera ~]$ touch /tmp/shared/defaults
    ```

    4.3.	С помощью команды `ls -l` отобразите список разрешений для нового файла.

    ```
    [operator1@servera ~]$ ls -l /tmp/shared/defaults
    -rw-rw-r--. 1 operator1 operator1 0 Feb  4 14:09 /tmp/shared/defaults
    ```
    </details>

5.	Измените группу-владельца каталога **/tmp/shared** на *operators*. Проверьте правильность нового владельца и разрешений.

    <details>
    <summary>Показать решение</summary>

    5.1.	С помощью команды `chown` измените группу-владельца каталога **/tmp/shared** на *operators*.

    ```
    [operator1@servera ~]$ chown :operators /tmp/shared
    ```

    5.2.	С помощью команды `ls -ld` отобразите список разрешений для каталога **/tmp/shared**.

    ```
    [operator1@servera ~]$ ls -ld /tmp/shared
    drwxrwxr-x. 2 operator1 operators 22 Feb  4 14:09 /tmp/shared 
    ```

    5.3.	С помощью команды `touch` создайте файл с именем group в каталоге **/tmp/shared**. С помощью команды `ls -l` отобразите список разрешений для файла.

    ```
    [operator1@servera ~]$ touch /tmp/shared/group
    [operator1@servera ~]$ ls -l /tmp/shared/group
    -rw-rw-r--. 1 operator1 operator1 0 Feb  4 17:00 /tmp/shared/group
    ```

    **Обратите внимание:** Группа-владелец файла /tmp/shared/group — не operators, а operator1.
    </details>


6.	Убедитесь, что файлы, созданные в каталоге **/tmp/shared**, принадлежат группе *operators*.

    6.1.	Используйте команду `chmod`, чтобы задать для каталога **/tmp/shared** бит **setgid** для группы-владельца *operators*.

    <details>
    <summary>Показать решение</summary>
    ```
    [operator1@servera ~]$ chmod g+s /tmp/shared
    ```
    </details>

    6.2.	С помощью команды `touch` создайте новый файл с именем **operations_database.txt** в каталоге **/tmp/shared**.

    <details>
    <summary>Показать решение</summary>
    ```
    [operator1@servera ~]$ touch /tmp/shared/operations_database.txt
    ```
    </details>

    6.3.	Используйте команду `ls -l`, чтобы убедиться, что группа *operators* является группой-владельцем нового файла.

    <details>
    <summary>Показать решение</summary>
    ```
    [operator1@servera ~]$ ls -l /tmp/shared/operations_database.txt
    -rw-rw-r--. 1 operator1 operators 0 Feb  4 16:11 /tmp/shared/operations_database.txt 
    ```
    </details>

7.	Создайте в каталоге **/tmp/shared** новый файл с именем **operations_network.txt**. Запишите владельца и разрешения. Измените пользовательскую маску для *operator1*. Создайте новый файл с именем **operations_production.txt**. Запишите владельца и разрешения для файла **operations_production.txt**.

    <details>
    <summary>Показать решение</summary>

    7.1.	С помощью команды `touch` создайте файл с именем **operations_network.txt** в каталоге **/tmp/shared**.

    ```
    [operator1@servera ~]$ touch /tmp/shared/operations_network.txt
    ```

    7.2.	С помощью команды `ls -l` отобразите список разрешений для файла **operations_network.txt**.

    ```
    [operator1@servera ~]$ ls -l /tmp/shared/operations_network.txt
    -rw-rw-r--. 1 operator1 operators 5 Feb  0 15:43 /tmp/shared/operations_network.txt 
    ```

    7.3.	С помощью команды `umask` измените маску для пользователя *operator1* на **027**. Выполните команду `umask`, чтобы подтвердить изменение.

    ```
    [operator1@servera ~]$ umask 027
    [operator1@servera ~]$ umask
    0027
    ```

    7.4.	С помощью команды `touch` создайте новый файл с именем **operations_production.txt** в каталоге **/tmp/shared/**. Используйте команду `ls -l`, чтобы убедиться, что новые файлы были созданы с доступом только на чтение для группы *operators* и без прав доступа для остальных пользователей.

    ```
    [operator1@servera ~]$ touch /tmp/shared/operations_production.txt
    [operator1@servera ~]$ ls -l /tmp/shared/operations_production.txt
    -rw-r-----. 1 operator1 operators 0 Feb  0 15:56 /tmp/shared/operations_production.txt 
    ```
    </details>

8.	Откройте новое окно терминала и войдите на **servera** как пользователь *operator1*.

    ```
    [student@workstation ~]$ ssh operator1@servera
    ...output omitted...
    [operator1@servera ~]$ 
    ```

9.	Отобразите значение пользовательской маски для пользователя *operator1*.

    <details>
    <summary>Показать решение</summary>
    ```
    [operator1@servera ~]$ umask
    0002 
    ```
    </details>

10.	Измените маску по умолчанию для пользователя *operator1*. Новая пользовательская маска запрещает любой доступ для пользователей, принадлежащих к другой группе. Убедитесь, что пользовательская маска была изменена.

    10.1.	С помощью команды `echo` измените маску по умолчанию для пользователя *operator1* на **007**.

    <details>
    <summary>Показать решение</summary>
    ```
    [operator1@servera ~]$ echo "umask 007" >> ~/.bashrc
    [operator1@servera ~]$ cat ~/.bashrc
    # .bashrc

    # Source global definitions
    if [ -f /etc/bashrc ]; then
    . /etc/bashrc
    fi

    # Uncomment the following line if you don't like systemctl's auto-paging feature:
    # export SYSTEMD_PAGER=

    # User specific aliases and functions
    umask 007

    ```
    </details>

    10.2.	Выйдите и войдите снова как пользователь *operator1*. Выполните команду `umask`, чтобы убедиться, что изменение сохранилось.

    <details>
    <summary>Показать решение</summary>
    ```
    [operator1@servera ~]$ exit
    logout
    Connection to servera closed.
    [student@workstation ~]$ ssh operator1@servera
    ...output omitted...
    [operator1@servera ~]$ umask
    0007
    ```
    </details>

11.	На **servera** выйдите из командных оболочек пользователей *operator1* и *student*.

    <details>
    <summary>Предупреждение</summary>
    
    Необходимо выйти из всех командных оболочек, открытых от имени пользователя operator1. Если не сделать этого, финальный сценарий завершится ошибкой.
    </details>

    ```
    [operator1@servera ~]$ exit
    logout
    Connection to servera closed.
    [student@workstation ~]$ 
    ```

## Конец

На **workstation** запустите сценарий `lab perms-default finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab perms-default finish
```

Упражнение завершено.

