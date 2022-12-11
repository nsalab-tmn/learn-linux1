## Цели

Вы сможете:

* с помощью команды `sudo` переключиться на пользователя *root* и получить доступ к интерактивной оболочке от имени *root* без пароля привилегированного пользователя;
* объяснить, как команды `su` и `su -` могут повлиять на среду оболочки, запуская или не запуская сценарии входа в систему;
* с помощью команды `sudo` выполнять другие команды от имени *root*.

## Задачи 

В этом упражнении вы будете переключаться на учетную запись *root* и выполнять команды от имени пользователя *root*.

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab users-sudo start`, чтобы начать упражнение. Этот сценарий создает необходимые учетные записи пользователей и файлы для настройки среды.

```
[student@workstation ~]$ lab users-sudo start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	Исследуйте среду оболочки пользователя *student*. Просмотрите сведения о текущем пользователе и текущей группе и отобразите текущий рабочий каталог. Также просмотрите переменные среды, которые определяют домашний каталог пользователя и расположение исполняемых файлов пользователя.

    2.1.	Выполните команду `id` для просмотра информации о текущем пользователе и текущей группе.

    ```
    [student@servera ~]$ id
    uid=1000(student) gid=1000(student) groups=1000(student),10(wheel) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
    ```

    2.2.	Выполните команду `pwd`, чтобы отобразить текущий рабочий каталог.

    ```
    [student@servera ~]$ pwd
    /home/student
    ```

    2.3.	Отобразите значения переменных HOME и PATH, чтобы определить домашний каталог и путь к исполняемым файлам пользователя.

    ```
    [student@servera ~]$ echo $HOME
    /home/student
    [student@servera ~]$ echo $PATH
    /home/student/.local/bin:/home/student/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
    ```

3.	Переключитесь на пользователя *root* в нерегистрационной оболочке и исследуйте новую среду оболочки.

    3.1.	Выполните команду `sudo su` в приглашении оболочки, чтобы переключиться на пользователя *root*.

    ```
    [student@servera ~]$ sudo su
    [sudo] password for student: student
    [root@servera student]# 
    ```

    3.2.	Выполните команду `id` для просмотра информации о текущем пользователе и текущей группе.

    ```
    [root@servera student]# id
    uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
    ```

    3.3.	Выполните команду `pwd`, чтобы отобразить текущий рабочий каталог.

    ```
    [root@servera student]# pwd
    /home/student
    ```

    3.4.	Отобразите значения переменных HOME и PATH, чтобы определить домашний каталог и путь к исполняемым файлам пользователя.

    ```
    [root@servera student]# echo $HOME
    /root
    [root@servera student]# echo $PATH
    /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin
    ```

    Если у вас уже есть опыт работы с Linux и командой `su`, вы могли ожидать, что при использовании команды `su` без дефиса (-) для переключения на *root* сохранится текущее значение переменной PATH ― student. Этого не произошло. Как вы увидите на следующем шаге, для *root* это тоже не обычная переменная PATH.

    Что же произошло? Разница в том, что вы не выполнили команду `su` напрямую. Вместо этого вы выполнили команду `su` как *root* с помощью `sudo`, поскольку не знали пароль привилегированного пользователя. Команда `sudo` переопределяет переменную PATH из начальной среды в целях безопасности. Любая команда, которая выполняется после первоначального переопределения, может обновить переменную PATH, как вы увидите на следующих шагах.

    3.5.	Закройте оболочку пользователя *root*, чтобы вернуться в оболочку пользователя *student*.

    ```
    [root@servera student]# exit
    exit
    [student@servera ~]$ 
    ```

4.	Переключитесь на пользователя *root* в регистрационной оболочке и исследуйте новую среду оболочки.

    4.1.	Выполните команду `sudo su -` в приглашении оболочки, чтобы переключиться на пользователя *root*.

    ```
    [student@servera ~]$ sudo su -
    [root@servera ~]# 
    ```

    Обратите внимание на отличие этого приглашения оболочки от `sudo su` на предыдущем шаге.

    `sudo` может запросить, а может не запрашивать у вас пароль *student* в зависимости от заданного периода ожидания `sudo`. По умолчанию период ожидания составляет 5 минут. Если вы прошли аутентификацию в `sudo` в течение последних пяти минут, `sudo` не будет запрашивать пароль. Если с момента аутентификации в `sudo` прошло более пяти минут, вам потребуется ввести пароль *student* для аутентификации в `sudo`.

    4.2.	Выполните команду `id` для просмотра информации о текущем пользователе и текущей группе.

    ```
    [root@servera ~]# id
    uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
    ```

    4.3.	Выполните команду `pwd`, чтобы отобразить текущий рабочий каталог.

    ```
    [root@servera ~]# pwd
    /root
    ```

    4.4.	Отобразите значения переменных HOME и PATH, чтобы определить домашний каталог и путь к исполняемым файлам пользователя.

    ```
    [root@servera ~]# echo $HOME
    /root
    [root@servera ~]# echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
    ```

    Как и на предыдущем шаге, после того как команда `sudo` сбросила для переменной PATH значение, заданное в настройках среды оболочки пользователя *student*, команда `su -` запустила сценарии входа в оболочку для *root* и присвоила переменной PATH другое значение. Команда `su` без дефиса (-) этого не делала.

    4.5.	Закройте оболочку пользователя *root*, чтобы вернуться в оболочку пользователя *student*.

    ```
    [root@servera ~]# exit
    logout
    [student@servera ~]$ 
    ```

5.	Убедитесь, что пользователю *operator1* разрешено выполнение любой команды от имени любого пользователя с помощью `sudo`.

    ```
    [student@servera ~]$ sudo cat /etc/sudoers.d/operator1
    operator1 ALL=(ALL) ALL
    ```

6.	Переключитесь на пользователя *operator1* и просмотрите содержимое файла **/var/log/messages**. Скопируйте файл **/etc/motd** в **/etc/motdOLD** и удалите его (**/etc/motdOLD**). Эти операции требуют административных прав, поэтому используйте `sudo` для выполнения этих команд от имени привилегированного пользователя. Не переключайтесь на пользователя *root* с помощью команды `sudo su` или `sudo su -`. Используйте *redhat* в качестве пароля пользователя *operator1*.

    6.1.	Переключитесь на пользователя *operator1*.

    ```
    [student@servera ~]$ su - operator1
    Password: redhat
    [operator1@servera ~]$ 
    ```

    6.2.	Попробуйте просмотреть последние пять строк файла **/var/log/messages**, не используя `sudo`. Эта попытка должна завершиться неудачей.

    ```
    [operator1@servera ~]$ tail -5 /var/log/messages
    tail: cannot open '/var/log/messages' for reading: Permission denied
    ```

    6.3.	Попробуйте просмотреть последние пять строк файла **/var/log/messages**, используя `sudo`. Эта попытка должна быть успешной.

    ```
    [operator1@servera ~]$ sudo tail -5 /var/log/messages
    [sudo] password for operator1: redhat
    Jan 23 15:53:36 servera su[2304]: FAILED SU (to operator1) student on pts/1
    Jan 23 15:53:51 servera su[2307]: FAILED SU (to operator1) student on pts/1
    Jan 23 15:53:58 servera su[2310]: FAILED SU (to operator1) student on pts/1
    Jan 23 15:54:12 servera su[2322]: (to operator1) student on pts/1
    Jan 23 15:54:25 servera su[2353]: (to operator1) student on pts/1
    ```

    <details>
    <summary>Примечание</summary>

    Вывод в вашей системе может отличаться от показанного выше.
    </details>

    6.4.	Попробуйте создать копию файла **/etc/motd** как **/etc/motdOLD**, не используя `sudo`. Эта попытка должна завершиться неудачей.

    ```
    [operator1@servera ~]$ cp /etc/motd /etc/motdOLD
    cp: cannot create regular file '/etc/motdOLD': Permission denied
    ```

    6.5.	Попробуйте создать копию файла **/etc/motd** как **/etc/motdOLD**, используя `sudo`. Эта попытка должна быть успешной.

    ```
    [operator1@servera ~]$ sudo cp /etc/motd /etc/motdOLD
    [operator1@servera ~]$ 
    ```

    6.6.	Попробуйте удалить файл **/etc/motdOLD**, не используя `sudo`. Эта попытка должна завершиться неудачей.

    ```
    [operator1@servera ~]$ rm /etc/motdOLD
    rm: remove write-protected regular empty file '/etc/motdOLD'? y
    rm: cannot remove '/etc/motdOLD': Permission denied
    [operator1@servera ~]$ 
    ```

    6.7.	Попробуйте удалить файл **/etc/motdOLD**, используя `sudo`. Эта попытка должна быть успешной.

    ```
    [operator1@servera ~]$ sudo rm /etc/motdOLD
    [operator1@servera ~]$ 
    ```

    6.8.	Закройте оболочку пользователя *operator1*, чтобы вернуться в оболочку пользователя *student*.

    ```
    [operator1@servera ~]$ exit
    logout
    [student@servera ~]$ 
    ```

    6.9.	Выйдите с **servera**.

    ```
    [student@servera ~]$ exit
    logout
    Connection to servera closed.
    [student@workstation ~]$ 
    ```

## Конец

На **workstation** запустите сценарий `lab users-sudo finish`, чтобы закончить упражнение. Этот сценарий удаляет учетные записи пользователей и файлы, созданные в начале упражнения, для очистки среды.

```
[student@workstation ~]$ lab users-sudo finish
```

Упражнение завершено.

