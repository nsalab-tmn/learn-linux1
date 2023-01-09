## Цели

Вы сможете управлять службами, которые контролирует **systemd**, с помощью команды `systemctl`.


## Задачи

В этом упражнении вы будете использовать команду `systemctl` для остановки, запуска, перезапуска, перезагрузки, включения и отключения службы, управляемой **systemd**.

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab services-control start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста servera в сети. Сценарий также гарантирует, что на servera запущены службы **sshd** и **chronyd**.

```
[student@workstation ~]$ lab services-control start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации, поэтому пароль не требуется.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Выполните команды `systemctl restart` и `systemctl reload` для службы **sshd**. Обратите внимание на разные результаты выполнения этих команд.

  2.1.	Отобразите состояние службы **sshd**. Обратите внимание на идентификатор процесса демона **sshd**.

  ```
  [student@servera ~]$ systemctl status sshd
  ● sshd.service - OpenSSH server daemon
    Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2019-02-06 23:50:42 EST; 9min ago
      Docs: man:sshd(8)
            man:sshd_config(5)
  Main PID: 759 (sshd)
      Tasks: 1 (limit: 11407)
    Memory: 5.9M
  ...output omitted...
  ```

  Нажмите q, чтобы выйти из команды.

  2.2.	Перезапустите службу **sshd** и посмотрите ее состояние. Идентификатор процесса демона должен измениться.

  ```
  [student@servera ~]$ sudo systemctl restart sshd
  [sudo] password for student: student
  [student@servera ~]$ systemctl status sshd
  ● sshd.service - OpenSSH server daemon
    Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2019-02-06 23:50:42 EST; 9min ago
      Docs: man:sshd(8)
            man:sshd_config(5)
  Main PID: 1132 (sshd)
      Tasks: 1 (limit: 11407)
    Memory: 5.9M
  ...output omitted...
  ```

  Обратите внимание, что в приведенном выше выводе идентификатор процесса изменился с 759 на 1132 (в вашей системе значения, скорее всего, будут другими). Нажмите q, чтобы выйти из команды.

  2.3.	Перезагрузите службу **sshd** и просмотрите ее состояние. Идентификатор процесса демона не должен измениться, и подключения не должны прерваться.

  ```
  [student@servera ~]$ sudo systemctl reload sshd
  [student@servera ~]$ systemctl status sshd
  ● sshd.service - OpenSSH server daemon
    Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2019-02-06 23:50:42 EST; 9min ago
      Docs: man:sshd(8)
            man:sshd_config(5)
  Main PID: 1132 (sshd)
      Tasks: 1 (limit: 11407)
    Memory: 5.9M
  ...output omitted...
  ```

  Нажмите q, чтобы выйти из команды.

3.	Убедитесь, что служба **chronyd** запущена.

  ```
  [student@servera ~]$ systemctl status chronyd
  ● chronyd.service - NTP client/server
    Loaded: loaded (/usr/lib/systemd/system/chronyd.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2019-02-06 23:50:38 EST; 1h 25min ago
    ...output omitted...
  ```

  Нажмите q, чтобы выйти из команды.

4.	Остановите службу **chronyd** и просмотрите ее состояние.

  ```
  [student@servera ~]$ sudo systemctl stop chronyd
  [student@servera ~]$ systemctl status chronyd
  ● chronyd.service - NTP client/server
    Loaded: loaded (/usr/lib/systemd/system/chronyd.service; enabled; vendor preset: enabled)
    Active: inactive (dead) since Thu 2019-02-07 01:20:34 EST; 44s ago
    ...output omitted...
  ... servera.lab.example.com chronyd[710]: System clock wrong by 1.349113 seconds, adjustment started
  ... servera.lab.example.com systemd[1]: Stopping NTP client/server...
  ... servera.lab.example.com systemd[1]: Stopped NTP client/server.
  ```

  Нажмите q, чтобы выйти из команды.

5.	Определите, включена ли служба **chronyd** для запуска во время начальной загрузки системы.

  ```
  [student@server ~]$ systemctl is-enabled chronyd
  enabled
  ```

6.	Перезагрузите **servera** и просмотрите состояние службы **chronyd**.

  ```
  [student@servera ~]$ sudo systemctl reboot
  Connection to servera closed by remote host.
  Connection to servera closed.
  [student@workstation ~]$ 
  ```


  Войдите на **servera** как пользователь *student* и просмотрите состояние службы **chronyd**.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ systemctl status chronyd
  ● chronyd.service - NTP client/server
    Loaded: loaded (/usr/lib/systemd/system/chronyd.service; enabled; vendor preset: enabled)
    Active: active (running) since Thu 2019-02-07 01:48:26 EST; 5min ago
    ...output omitted...
  ```

  Нажмите q, чтобы выйти из команды.

7.	Отключите службу **chronyd**, чтобы она не запускалась во время начальной загрузки системы, а затем просмотрите ее состояние.

  ```
  [student@servera ~]$ sudo systemctl disable chronyd
  [sudo] password for student: student
  Removed /etc/systemd/system/multi-user.target.wants/chronyd.service.
  [student@servera ~]$ systemctl status chronyd
  ● chronyd.service - NTP client/server
    Loaded: loaded (/usr/lib/systemd/system/chronyd.service; disabled; vendor preset: enabled)
    Active: active (running) since Thu 2019-02-07 01:48:26 EST; 5min ago
    ...output omitted...
  ```

  Нажмите q, чтобы выйти из команды.

8.	Перезагрузите **servera** и просмотрите состояние службы **chronyd**.

  ```
  [student@servera ~]$ sudo systemctl reboot
  Connection to servera closed by remote host.
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

  Войдите на **servera** как пользователь *student* и просмотрите состояние службы **chronyd**.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ systemctl status chronyd
  ● chronyd.service - NTP client/server
    Loaded: loaded (/usr/lib/systemd/system/chronyd.service; disabled; vendor preset: enabled)
    Active: inactive (dead)
    Docs: man:chronyd(8)
          man:chrony.conf(5)
  ```

9.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab services-control finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab services-control finish
```

Упражнение завершено.

