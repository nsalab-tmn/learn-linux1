## Цели

Получение базовых навыков настройки службы rsyslog

## Задачи

В этом упражнении вы настроите rsyslog для записи определенных сообщений в новый log-файл.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab log-configure start`, чтобы начать упражнение. Этот сценарий обеспечивает правильную настройку среды.

```
[student@workstation ~]$ lab log-configure start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Настройте rsyslog на **servera** для сохранения всех сообщений с приоритетом **debug** или выше для любой службы в новый log-файл **/var/log/messages-debug**, добавив файл конфигурации службы rsyslog **/etc/rsyslog.d/debug.conf**.

  2.1.	Выполните команду `sudo -i`, чтобы переключиться на пользователя root. Укажите *student* в качестве пароля для пользователя *student*, если вам будет предложено его ввести при выполнении команды `sudo -i`.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ sudo -i
  [sudo] password for student: student
  [root@servera ~]# 
  ```
  </details>

  2.2.	Создайте файл **/etc/rsyslog.d/debug.conf** с необходимыми записями, чтобы перенаправлять все сообщения журнала с приоритетом debug в файл **/var/log/messages-debug**. Используйте команду `vim /etc/rsyslog.d/debug.conf`, чтобы создать файл со следующим содержимым.

  ```
  *.debug /var/log/messages-debug
  ```

  Эта строка конфигурации перехватывает сообщения syslog с любым источником и приоритетом **debug** или более высоким. Служба rsyslog запишет соответствующие сообщения в файл **/var/log/messages-debug**. Метасимвол `*` в поле **facility** или **priority** строки конфигурации указывает на любой источник или приоритет.

  2.3.	Перезапустите службу rsyslog.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# systemctl restart rsyslog
  ```
  </details>

3.	Убедитесь, что все сообщения журнала с приоритетом debug отображаются в файле **/var/log/messages-debug**.

  3.1.	Выполните команду `logger` с опцией `-p`, чтобы создать сообщение журнала с источником **user** и приоритетом **debug**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# logger -p user.debug "Debug Message Test"
  ```
  </details>

  3.2.	Выполните команду `tail`, чтобы просмотреть последние 10 сообщений журнала из файла **/var/log/messages-debug**, и убедитесь, что среди них есть сообщение *Debug Message Test*.

  ```
  [root@servera ~]# tail /var/log/messages-debug
  Feb 13 18:22:38 servera systemd[1]: Stopping System Logging Service...
  Feb 13 18:22:38 servera rsyslogd[25176]: [origin software="rsyslogd" swVersion="8.37.0-9.el8" x-pid="25176" x-info="http://www.rsyslog.com"] exiting on signal 15.
  Feb 13 18:22:38 servera systemd[1]: Stopped System Logging Service.
  Feb 13 18:22:38 servera systemd[1]: Starting System Logging Service...
  Feb 13 18:22:38 servera rsyslogd[25410]: environment variable TZ is not set, auto correcting this to TZ=/etc/localtime  [v8.37.0-9.el8 try http://www.rsyslog.com/e/2442 ]
  Feb 13 18:22:38 servera systemd[1]: Started System Logging Service.
  Feb 13 18:22:38 servera rsyslogd[25410]: [origin software="rsyslogd" swVersion="8.37.0-9.el8" x-pid="25410" x-info="http://www.rsyslog.com"] start
  Feb 13 18:27:58 servera student[25416]: Debug Message Test
  ```

  3.3.	Выйдите из оболочек пользователей *root* и *student* на **servera**, чтобы вернуться в оболочку пользователя *student* на **workstation**.

  ```
  [root@servera ~]# exit
  logout
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab log-configure finish`, чтобы закончить упражнение. Этот сценарий обеспечивает очистку среды.

```
[student@workstation ~]$ lab log-configure finish
```

Упражнение завершено.



