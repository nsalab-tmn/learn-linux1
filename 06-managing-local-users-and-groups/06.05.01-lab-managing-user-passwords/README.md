## Цели

Вы сможете:

* настроить принудительное изменение пароля пользователем при первом входе в систему;
* настроить принудительное изменение пароля каждые 90 дней;
* задать для учетной записи окончание срока действия через 180 дней после текущего дня.

## Задачи 

В этом упражнении вы настроите политики паролей для нескольких пользователей.

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab users-pw-manage start`, чтобы начать упражнение. Этот сценарий создает необходимые учетные записи пользователей и файлы для настройки среды.

```
[student@workstation ~]$ lab users-pw-manage start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	На **servera** попробуйте заблокировать и разблокировать учетные записи пользователей как *student*.

    2.1.	Как *student* заблокируйте учетную запись *operator1*, используя права администратора.

    ```
    [student@servera ~]$ sudo usermod -L operator1
    [sudo] password for student: student
    ```

    2.2.	Попробуйте войти как пользователь *operator1*. Эта попытка должна завершиться неудачей.

    ```
    [student@servera ~]$ su - operator1
    Password: redhat
    su: Authentication failure
    ```

    2.3.	Разблокируйте учетную запись *operator1*.

    ```
    [student@servera ~]$ sudo usermod -U operator1
    ```

    2.4.	Снова попробуйте войти как пользователь *operator1*. Эта попытка должна быть успешной.

    ```
    [student@servera ~]$ su - operator1
    Password: redhat
    ...output omitted...
    [operator1@servera ~]$ 
    ```

    2.5.	Закройте оболочку пользователя *operator1*, чтобы вернуться в оболочку пользователя *student*.

    ```
    [operator1@servera ~]$ exit
    logout
    ```

3.	Измените политику паролей для пользователя *operator1*, чтобы требовать смены пароля каждые 90 дней. Убедитесь, что срок действия пароля успешно установлен.

    3.1.	Установите для пароля пользователя *operator1* максимальный срок действия 90 дней.

    ```
    [student@servera ~]$ sudo chage -M 90 operator1
    ```

    3.2.	Убедитесь, что срок действия пароля пользователя *operator1* заканчивается через 90 дней после его изменения.

    ```
    [student@servera ~]$ sudo chage -l operator1
    Last password change      : Jan 25, 2019
    Password expires          : Apr 25, 2019
    Password inactive         : never
    Account expires           : never
    Minimum number of days between password change    : 0
    Maximum number of days between password change    : 90
    Number of days of warning before password expires : 7
    ```

4.	Включите для учетной записи *operator1* принудительную смену пароля при первом входе.

    ```
    [student@servera ~]$ sudo chage -d 0 operator1
    ```

5.	Выполните вход как пользователь *operator1* и измените пароль на *forsooth123*. Задав пароль, вернитесь в оболочку пользователя *student*.

    5.1.	Выполните вход как пользователь *operator1* и измените пароль на *forsooth123*, когда появится соответствующее приглашение.

    ```
    [student@servera ~]$ su - operator1
    Password: redhat
    You are required to change your password immediately (administrator enforced)
    Current password: redhat
    New password: forsooth123
    Retype new password: forsooth123
    ...output omitted...
    [operator1@servera ~]$ 
    ```

    5.2.	Закройте оболочку пользователя *operator1*, чтобы вернуться в оболочку пользователя *student*.

    ```
    [operator1@servera ~]$ exit
    logout
    ```

6.	Задайте для учетной записи *operator1* окончание срока действия через 180 дней после текущего дня. Подсказка. Команда `date -d "+180 days"` позволяет получить дату и время через 180 дней после текущей даты и времени.

    6.1.	Определите дату на 180 дней в будущем. Используйте формат `%F` с командой date, чтобы получить точное значение.

    ```
    [student@servera ~]$ date -d "+180 days" +%F
    2019-07-24
    ```

    Вы можете получить другое значение в зависимости от текущей даты и времени в вашей системе.

    6.2.	Установите окончание срока действия учетной записи на дату, указанную на предыдущем шаге.

    ```
    [student@servera ~]$ sudo chage -E 2019-07-24 operator1
    ```

    6.3.	Убедитесь, что дата окончания срока действия учетной записи успешно установлена.

    ```
    [student@servera ~]$ sudo chage -l operator1
    Last password change      : Jan 25, 2019
    Password expires          : Apr 25, 2019
    Password inactive         : never
    Account expires           : Jul 24, 2019
    Minimum number of days between password change    : 0
    Maximum number of days between password change    : 90
    Number of days of warning before password expires : 7
    ```

7.	Установите окончание срока действия паролей через 180 дней после текущей даты для всех пользователей. Используйте права администратора, чтобы изменить файл конфигурации.

    7.1.	Задайте для PASS_MAX_DAYS значение 180 в файле **/etc/login.defs**. Используйте права администратора при открытии файла в текстовом редакторе. Для выполнения этого шага можно использовать команду `sudo vim /etc/login.defs`.

    ```
    ...output omitted...
    # Password aging controls:
    #
    #       PASS_MAX_DAYS   Maximum number of days a password may be
    #       used.
    #       PASS_MIN_DAYS   Minimum number of days allowed between
    #       password changes.
    #       PASS_MIN_LEN    Minimum acceptable password length.
    #       PASS_WARN_AGE   Number of days warning given before a
    #       password expires.
    #
    PASS_MAX_DAYS   180
    PASS_MIN_DAYS   0
    PASS_MIN_LEN    5
    PASS_WARN_AGE   7
    ...output omitted...
    ```

    <details>
    <summary>Важно</summary>

    Настройки по умолчанию, касающиеся срока действия паролей и учетных записей, будут действовать для новых пользователей, но не для существующих.
    </details>

    7.2.	Выйдите с servera.

    ```
    [student@servera ~]$ exit
    logout
    Connection to servera closed.
    [student@workstation ~]$ 
    ```

## Конец

На **workstation** запустите сценарий `lab users-pw-manage finish`, чтобы закончить упражнение. Этот сценарий удаляет учетные записи пользователей и файлы, созданные в начале упражнения, для очистки среды.

```
[student@workstation ~]$ lab users-pw-manage finish
```

Упражнение завершено.