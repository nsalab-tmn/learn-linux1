## Цели

Получение базовых навыков поиска событий в системном журнале

## Задачи

В этом упражнении вы будете искать в системном журнале записи о событиях, соответствующие определенным критериям.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab log-query start`, чтобы начать упражнение. Этот сценарий обеспечивает правильную настройку среды.

```
[student@workstation ~]$ lab log-query start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Используйте сопоставление **_PID=1** с командой `journalctl`, чтобы отобразить все события журнала, источником которых является процесс **systemd**, работающий на **servera** с идентификатором процесса 1. Нажмите **q**, чтобы выйти из команды `journalctl`.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ journalctl _PID=1
  ...output omitted...
  Feb 13 13:21:08 localhost systemd[1]: Found device /dev/disk/by-uuid/cdf61ded-534c-4bd6-b458-cab18b1a72ea.
  Feb 13 13:21:08 localhost systemd[1]: Started dracut initqueue hook.
  Feb 13 13:21:08 localhost systemd[1]: Found device /dev/disk/by-uuid/44330f15-2f9d-4745-ae2e-20844f22762d.
  Feb 13 13:21:08 localhost systemd[1]: Reached target Initrd Root Device.
  lines 1-5/5 (END) q
  [student@servera ~]$ 
  ```

  В вашей системе вывод команды journalctl может быть другим.
  </details>

3.	Используйте сопоставление **_UID=81** с командой `journalctl`, чтобы отобразить все события журнала, источником которых является системная служба, запущенная на **servera** с идентификатором пользователя 81. Нажмите **q**, чтобы выйти из команды `journalctl`.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ journalctl _UID=81
  ...output omitted...
  Feb 22 01:29:09 servera.lab.example.com dbus-daemon[672]: [system] Activating via systemd: service name='org.freedesktop.nm_dispatcher'>
  Feb 22 01:29:09 servera.lab.example.com dbus-daemon[672]: [system] Successfully activated service 'org.freedesktop.nm_dispatcher'
  lines 1-5/5 (END) q
  [student@servera ~]$ 
  ```
  </details>

4.	Используйте опцию `-p warning` с командой `journalctl`, чтобы отобразить события журнала с приоритетом **warning** и выше на **servera**. Нажмите **q**, чтобы выйти из команды `journalctl`.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ journalctl -p warning
  ...output omitted...
  Feb 13 13:21:07 localhost kernel: Detected CPU family 6 model 13 stepping 3
  Feb 13 13:21:07 localhost kernel: Warning: Intel Processor - this hardware has not undergone testing by Red Hat and might not >
  Feb 13 13:21:07 localhost kernel: acpi PNP0A03:00: fail to add MMCONFIG information, can't access extended PCI configuration s>
  Feb 13 13:21:07 localhost rpc.statd[288]: Running as root.  chown /var/lib/nfs/statd to choose different user
  Feb 13 13:21:07 localhost rpc.idmapd[293]: Setting log level to 0
  ...output omitted...
  Feb 13 13:21:13 servera.lab.example.com rsyslogd[1172]: environment variable TZ is not set, auto correcting this to TZ=/etc/lo>
  Feb 13 14:51:42 servera.lab.example.com systemd[1]: cgroup compatibility translation between legacy and unified hierarchy sett>
  Feb 13 17:15:37 servera.lab.example.com rsyslogd[25176]: environment variable TZ is not set, auto correcting this to TZ=/etc/l>
  Feb 13 18:22:38 servera.lab.example.com rsyslogd[25410]: environment variable TZ is not set, auto correcting this to TZ=/etc/l>
  Feb 13 18:47:55 servera.lab.example.com rsyslogd[25731]: environment variable TZ is not set, auto correcting this to TZ=/etc/l>
  lines 1-17/17 (END) q
  [student@servera ~]$ 
  ```
  </details>

5.	Отобразите все события журнала, записанные на **servera** в течение последних 10 минут.

  <details>
  <summary>Показать решение</summary>


  Используйте опцию `--since` с командой `journalctl`, чтобы отобразить все события журнала, записанные на **servera** в течение последних 10 минут. Нажмите **q**, чтобы выйти из команды `journalctl`.

  ```
  [student@servera ~]$ journalctl --since "-10min"
  ...output omitted...
  Feb 13 22:31:01 servera.lab.example.com CROND[25890]: (root) CMD (run-parts /etc/cron.hourly)
  Feb 13 22:31:01 servera.lab.example.com run-parts[25893]: (/etc/cron.hourly) starting 0anacron
  Feb 13 22:31:01 servera.lab.example.com run-parts[25899]: (/etc/cron.hourly) finished 0anacron
  Feb 13 22:31:41 servera.lab.example.com sshd[25901]: Bad protocol version identification 'brain' from 172.25.250.254 port 37450
  Feb 13 22:31:42 servera.lab.example.com sshd[25902]: Accepted publickey for root from 172.25.250.254 port 37452 ssh2: RSA SHA2>
  Feb 13 22:31:42 servera.lab.example.com systemd[1]: Started /run/user/0 mount wrapper.
  Feb 13 22:31:42 servera.lab.example.com systemd[1]: Created slice User Slice of UID 0.
  Feb 13 22:31:42 servera.lab.example.com systemd[1]: Starting User Manager for UID 0...
  Feb 13 22:31:42 servera.lab.example.com systemd[1]: Started Session 118 of user root.
  Feb 13 22:31:42 servera.lab.example.com systemd-logind[712]: New session 118 of user root.
  Feb 13 22:31:42 servera.lab.example.com systemd[25906]: pam_unix(systemd-user:session): session opened for user root by (uid=0)
  ...output omitted...
  lines 1-32/84 39% q
  [student@servera ~]$
  ```
  </details>

6.	Используйте опцию `--since` и сопоставление `_SYSTEMD_UNIT="sshd.service"` с командой `journalctl`, чтобы отобразить все события журнала, источником которых является служба **sshd** и которые были записаны сегодня после **09:00:00** на **servera**. Нажмите **q**, чтобы выйти из команды `journalctl`.

  <details>
  <summary>Примечание</summary>

  Ваш часовой пояс может отличаться от часового пояса учебной аудитории. Проверьте время на машине **servera** и при необходимости измените значение опции `--since` соответствующим образом.
  </details>

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ journalctl --since 9:00:00 _SYSTEMD_UNIT="sshd.service"
  ...output omitted...
  Feb 13 13:21:12 servera.lab.example.com sshd[727]: Server listening on 0.0.0.0 port 22.
  Feb 13 13:21:12 servera.lab.example.com sshd[727]: Server listening on :: port 22.
  Feb 13 13:22:07 servera.lab.example.com sshd[1238]: Accepted publickey for student from 172.25.250.250 port 50590 ssh2: RSA SH>
  Feb 13 13:22:07 servera.lab.example.com sshd[1238]: pam_unix(sshd:session): session opened for user student by (uid=0)
  Feb 13 13:22:08 servera.lab.example.com sshd[1238]: pam_unix(sshd:session): session closed for user student
  Feb 13 13:25:47 servera.lab.example.com sshd[1289]: Accepted publickey for root from 172.25.250.254 port 37194 ssh2: RSA SHA25>
  Feb 13 13:25:47 servera.lab.example.com sshd[1289]: pam_unix(sshd:session): session opened for user root by (uid=0)
  Feb 13 13:25:47 servera.lab.example.com sshd[1289]: pam_unix(sshd:session): session closed for user root
  Feb 13 13:25:48 servera.lab.example.com sshd[1316]: Accepted publickey for root from 172.25.250.254 port 37196 ssh2: RSA SHA25>
  Feb 13 13:25:48 servera.lab.example.com sshd[1316]: pam_unix(sshd:session): session opened for user root by (uid=0)
  Feb 13 13:25:48 servera.lab.example.com sshd[1316]: pam_unix(sshd:session): session closed for user root
  Feb 13 13:26:07 servera.lab.example.com sshd[1355]: Accepted publickey for student from 172.25.250.254 port 37198 ssh2: RSA SH>
  Feb 13 13:26:07 servera.lab.example.com sshd[1355]: pam_unix(sshd:session): session opened for user student by (uid=0)
  Feb 13 13:52:28 servera.lab.example.com sshd[1473]: Accepted publickey for root from 172.25.250.254 port 37218 ssh2: RSA SHA25>
  Feb 13 13:52:28 servera.lab.example.com sshd[1473]: pam_unix(sshd:session): session opened for user root by (uid=0)
  ...output omitted...
  lines 1-32 q
  [student@servera ~]$ 
  ```
  </details>

7.	Выйдите с **servera**.

```
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$ 
```

## Конец

На **workstation** запустите сценарий `lab log-query finish`, чтобы закончить упражнение. Этот сценарий обеспечивает очистку среды.

```
[student@workstation ~]$ lab log-query finish
```

Упражнение завершено.


