## Цели

Вы сможете настроить в системе Linux с дополнительные учетные записи пользователей.

## Задачи 

В этом упражнении вы создадите нескольких пользователей в своей системе и установите для них пароли.

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab users-manage start`, чтобы начать упражнение. Этот сценарий обеспечивает правильную настройку среды.

```
[student@workstation ~]$ lab users-manage start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	На **servera** с помощью команды `sudo` переключитесь на пользователя *root*, чтобы перейти в среду оболочки пользователя *root*.

    ```
    [student@servera ~]$ sudo su -
    [sudo] password for student: student
    [root@servera ~]# 
    ```

3.	Создайте пользователя *operator1* и убедитесь, что он существует в системе.

    ```
    [root@servera ~]# useradd operator1
    [root@servera ~]# tail /etc/passwd
    ...output omitted...
    operator1:x:1002:1002::/home/operator1:/bin/bash
    ```

4.	Задайте для пользователя *operator1* пароль *P@ssw0rd*.

    ```
    [root@servera ~]# passwd operator1
    Changing password for user operator1.
    New password: P@ssw0rd
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: P@ssw0rd
    passwd: all authentication tokens updated successfully.
    ```

5.	Создайте дополнительных пользователей с именами *operator2* и *operator3*. Задайте для них пароль *P@ssw0rd*.

    5.1.	Добавьте пользователя *operator2*. Задайте для пользователя *operator2* пароль *P@ssw0rd*.

    ```
    [root@servera ~]# useradd operator2
    [root@servera ~]# passwd operator2
    Changing password for user operator2.
    New password: P@ssw0rd
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: P@ssw0rd
    passwd: all authentication tokens updated successfully.
    ```

    5.2.	Добавьте пользователя *operator3*. Задайте для пользователя *operator3* пароль *P@ssw0rd*.

    ```
    [root@servera ~]# useradd operator3
    [root@servera ~]# passwd operator3
    Changing password for user operator3.
    New password: P@ssw0rd
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: P@ssw0rd
    passwd: all authentication tokens updated successfully.
    ```

6.	Обновите учетные записи пользователей *operator1* и *operator2*, добавив комментарии *Operator One* и *Operator Two* соответственно. Убедитесь, что комментарии успешно добавлены.

    6.1.	Выполните команду `usermod -c`, чтобы изменить комментарии для учетной записи пользователя *operator1*.

    ```
    [root@servera ~]# usermod -c "Operator One" operator1
    ```

    6.2.	Выполните команду `usermod -c`, чтобы изменить комментарии для учетной записи пользователя *operator2*.

    ```
    [root@servera ~]# usermod -c "Operator Two" operator2
    ```

    6.3.	Убедитесь, что комментарии для пользователей *operator1* и *operator2* отображаются в пользовательских записях.

    ```
    [root@servera ~]# tail /etc/passwd
    ...output omitted...
    operator1:x:1002:1002:Operator One:/home/operator1:/bin/bash
    operator2:x:1003:1003:Operator Two:/home/operator2:/bin/bash
    operator3:x:1004:1004::/home/operator3:/bin/bash
    ```

7.	Удалите пользователя *operator3* вместе со всеми его личными данными. Убедитесь, что пользователь успешно удален.

    7.1.	Удалите пользователя *operator3* из системы.

    ```
    [root@servera ~]# userdel -r operator3
    ```

    7.2.	Убедитесь, что пользователь *operator3* успешно удален.

    ```
    [root@servera ~]# tail /etc/passwd
    ...output omitted...
    operator1:x:1002:1002:Operator One:/home/operator1:/bin/bash
    operator2:x:1003:1003:Operator Two:/home/operator2:/bin/bash
    ```

    Обратите внимание, что в предыдущем выводе нет информации об учетной записи пользователя *operator3*.


    7.3.	Закройте оболочку пользователя *root*, чтобы вернуться в оболочку пользователя *student*.

    ```
    [root@servera ~]# exit
    logout
    [student@servera ~]$ 
    ```

    7.4.	Выйдите с **servera**.

    ```
    [student@servera ~]$ exit
    logout
    Connection to servera closed.
    [student@workstation ~]$ 
    ```

## Конец

На **workstation** запустите сценарий `lab users-manage finish`, чтобы закончить упражнение. Этот сценарий обеспечивает очистку среды.

```
[student@workstation ~]$ lab users-manage finish
```

Упражнение завершено.

