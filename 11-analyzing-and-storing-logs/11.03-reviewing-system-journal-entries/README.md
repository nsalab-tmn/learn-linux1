## Цели

После завершения этого раздела вы сможете находить и интерпретировать записи в системном журнале для устранения проблем или просмотра состояния системы.

## Поиск событий

Служба **systemd-journald** сохраняет протоколируемые данные в структурированном и индексированном двоичном файле, который называется журналом. Эти данные включают дополнительную информацию о протоколируемых событиях. Например, для событий syslog могут записываться сведения об источнике и приоритете исходного сообщения.

<details>
<summary>Важно</summary>

В Linux системный журнал по умолчанию хранится в каталоге **/run/log**. Содержимое каталога **/run/log** очищается при перезагрузке системы. Вы можете изменить эти настройки (способ рассматривается позже в этой главе).
</details>

Для просмотра сообщений журнала используйте команду `journalctl`. С помощью этой команды можно отобразить все сообщения в журнале или найти определенные событий на основе широкого диапазона опций и критериев. Если выполнить команду от имени пользователя *root*, будет предоставлен полный доступ к журналу. Обычные пользователи также могут выполнять эту команду, но некоторые сообщения будут недоступны.

```
[root@host ~]# journalctl
...output omitted...
Feb 21 17:46:25 host.lab.example.com systemd[24263]: Stopped target Sockets.
Feb 21 17:46:25 host.lab.example.com systemd[24263]: Closed D-Bus User Message Bus Socket.
Feb 21 17:46:25 host.lab.example.com systemd[24263]: Closed Multimedia System.
Feb 21 17:46:25 host.lab.example.com systemd[24263]: Reached target Shutdown.
Feb 21 17:46:25 host.lab.example.com systemd[24263]: Starting Exit the Session...
Feb 21 17:46:25 host.lab.example.com systemd[24268]: pam_unix(systemd-user:session): session c>
Feb 21 17:46:25 host.lab.example.com systemd[1]: Stopped User Manager for UID 1001.
Feb 21 17:46:25 host.lab.example.com systemd[1]: user-runtime-dir@1001.service: Unit not neede>
Feb 21 17:46:25 host.lab.example.com systemd[1]: Stopping /run/user/1001 mount wrapper...
Feb 21 17:46:25 host.lab.example.com systemd[1]: Removed slice User Slice of UID 1001.
Feb 21 17:46:25 host.lab.example.com systemd[1]: Stopped /run/user/1001 mount wrapper.
Feb 21 17:46:36 host.lab.example.com sshd[24434]: Accepted publickey for root from 172.25.250.>
Feb 21 18:01:01 host.lab.example.com CROND[24468]: (root) CMD (run-parts /etc/cron.hourly)
lines 1464-1487/1487 (END) q
```

Команда `journalctl` подсвечивает важные сообщения журнала: сообщения с приоритетом **notice** или **warning** выделены жирным шрифтом, а сообщения с приоритетом **error** или выше выделены красным цветом.

Чтобы успешно использовать журнал для устранения проблем и аудита, рекомендуется ограничивать результаты поиска в журнале нужными данными.

По умолчанию команда `journalctl -n` отображает последние 10 записей журнала. Изменить это значение можно с помощью аргумента, который указывает, сколько записей журнала нужно отобразить. Чтобы отобразить последние пять записей журнала, выполните следующую команду `journalctl`:

```
[root@host ~]# journalctl -n 5
-- Logs begin at Wed 2019-02-20 16:01:17 +07, end at Thu 2019-02-21 18:01:01 +07. --
...output omitted...
Feb 21 17:46:37 host.lab.example.com systemd-logind[708]: New session 20 of user root.
Feb 21 17:46:37 host.lab.example.com sshd[24434]: pam_unix(sshd:session): session opened for u>
Feb 21 18:01:01 host.lab.example.com CROND[24468]: (root) CMD (run-parts /etc/cron.hourly)
Feb 21 18:01:01 host.lab.example.com run-parts[24471]: (/etc/cron.hourly) starting 0anacron
Feb 21 18:01:01 host.lab.example.com run-parts[24477]: (/etc/cron.hourly) finished 0anacron
lines 1-6/6 (END) q
```

Как и команда `tail -f`, команда `journalctl -f` выводит последние 10 строк системного журнала и продолжает выводить новые записи журнала по мере их появления. Чтобы выйти из процесса `journalctl -f`, используйте сочетание клавиш **Ctrl+С**.

```
[root@host ~]# journalctl -f
-- Logs begin at Wed 2019-02-20 16:01:17 +07. --
...output omitted...
Feb 21 18:01:01 host.lab.example.com run-parts[24477]: (/etc/cron.hourly) finished 0anacron
Feb 21 18:22:42 host.lab.example.com sshd[24437]: Received disconnect from 172.25.250.250 port 48710:11: disconnected by user
Feb 21 18:22:42 host.lab.example.com sshd[24437]: Disconnected from user root 172.25.250.250 port 48710
Feb 21 18:22:42 host.lab.example.com sshd[24434]: pam_unix(sshd:session): session closed for user 
Feb 21 18:22:43 host.lab.example.com sshd[24499]: Accepted publickey for root from 172.25.250.250 port 48714 ssh2: RSA SHA256:1UGybTe52L2jzEJa1HLVKn9QUCKrTv3ZzxnMJol1Fro
Feb 21 18:22:44 host.lab.example.com systemd-logind[708]: New session 21 of user root.
Feb 21 18:22:44 host.lab.example.com systemd[1]: Started Session 21 of user root.
Feb 21 18:22:44 host.lab.example.com sshd[24499]: pam_unix(sshd:session): session opened for user root by (uid=0)
^C
[root@host ~]# 
```

При решении проблем удобно отфильтровать вывод журнала по приоритету записей. Команда `journalctl -p` принимает имя или номер приоритета и отображает сообщения журнала для указанного и более высоких уровней приоритета. Команде `journalctl` известны следующие уровни приоритета: **debug**, **info**, **notice**, **warning**, **err**, **crit**, **alert** и **emerg**.
Выполните следующую команду `journalctl`, чтобы отобразить записи журнала с приоритетом **err** или выше:

```
[root@host ~]# journalctl -p err
-- Logs begin at Wed 2019-02-20 16:01:17 +07, end at Thu 2019-02-21 18:01:01 +07. --
..output omitted...
Feb 20 16:01:17 host.lab.example.com kernel: Detected CPU family 6 model 13 stepping 3
Feb 20 16:01:17 host.lab.example.com kernel: Warning: Intel Processor - this hardware has not undergone testing by Red Hat and might not be certif>
Feb 20 16:01:20 host.lab.example.com smartd[669]: DEVICESCAN failed: glob(3) aborted matching pattern /dev/discs/disc*
Feb 20 16:01:20 host.lab.example.com smartd[669]: In the system's table of devices NO devices found to scan
lines 1-5/5 (END) q
```

При поиске конкретных событий можно ограничивать вывод определенными временными рамками. Для команды `journalctl` предусмотрены две опции, ограничивающие вывод указанным временным диапазоном: `--since` и `--until`. Обе опции принимают аргумент времени в формате **"ГГГГ-ММ-ДД чч:мм:сс"** (двойные кавычки необходимы для сохранения пробела в опции). Если дата не указана, команда использует сегодняшнюю дату, а если не указано время, команда использует целые сутки начиная с 00:00:00. Обе опции, помимо полей даты и времени, могут принимать аргументы **yesterday**, **today** и **tomorrow**.

Выполните следующую команду `journalctl`, чтобы отобразить все сегодняшние записи журнала.

```
[root@host ~]# journalctl --since today
-- Logs begin at Wed 2019-02-20 16:01:17 +07, end at Thu 2019-02-21 18:31:14 +07. --
...output omitted...
Feb 21 18:22:44 host.lab.example.com systemd-logind[708]: New session 21 of user root.
Feb 21 18:22:44 host.lab.example.com systemd[1]: Started Session 21 of user root.
Feb 21 18:22:44 host.lab.example.com sshd[24499]: pam_unix(sshd:session): session opened for user root by (uid=0)
Feb 21 18:31:14 host.lab.example.com dnf[24533]: Red Hat Enterprise Linux 8.0 AppStream (dvd)    637 kB/s | 2.8 kB     00:00
Feb 21 18:31:14 host.lab.example.com dnf[24533]: Red Hat Enterprise Linux 8.0 BaseOS (dvd)       795 kB/s | 2.7 kB     00:00
Feb 21 18:31:14 host.lab.example.com systemd[1]: Started dnf makecache.
lines 533-569/569 (END) q
Выполните следующую команду journalctl, чтобы отобразить все записи журнала в диапазоне с 2019-02-10 20:30:00 по 2019-02-13 12:00:00.
[root@host ~]# journalctl --since "2019-02-10 20:30:00" \
--until "2019-02-13 12:00:00"
...output omitted...
```

Вы также можете указать все записи за определенный промежуток времени относительно настоящего момента. Например, чтобы указать все записи за последний час, выполните следующую команду:

```
[root@host ~]# journalctl --since "-1 hour"
...output omitted...
```

<details>
<summary>Примечание</summary>

Вы можете использовать и другие, более сложные временные характеристики с опциями `--since` и `--until`. Некоторые примеры см. на man-странице `systemd.time(7)`.
</details>

Помимо видимого содержимого журнала, к его записям добавлены поля, которые отображаются, только если включена детализация вывода. Все отображаемые дополнительные поля могут использоваться для фильтрации вывода из запроса к журналу. Это удобно для сокращения вывода сложных запросов при поиске определенных событий в журнале.

```
[root@host ~]# journalctl -o verbose
-- Logs begin at Wed 2019-02-20 16:01:17 +07, end at Thu 2019-02-21 18:31:14 +07. --
...output omitted...
Thu 2019-02-21 18:31:14.509128 +07...
    PRIORITY=6
    _BOOT_ID=4409bbf54680496d94e090de9e4a9e23
    _MACHINE_ID=73ab164e278e48be9bf80e80714a8cd5
    SYSLOG_FACILITY=3
    SYSLOG_IDENTIFIER=systemd
    _UID=0
    _GID=0
    CODE_FILE=../src/core/job.c
    CODE_LINE=826
    CODE_FUNC=job_log_status_message
    JOB_TYPE=start
    JOB_RESULT=done
    MESSAGE_ID=39f53479d3a045ac8e11786248231fbf
    _TRANSPORT=journal
    _PID=1
    _COMM=systemd
    _EXE=/usr/lib/systemd/systemd
    _CMDLINE=/usr/lib/systemd/systemd --switched-root --system --deserialize 18
    _CAP_EFFECTIVE=3fffffffff
    _SELINUX_CONTEXT=system_u:system_r:init_t:s0
    _SYSTEMD_CGROUP=/init.scope
    _SYSTEMD_UNIT=init.scope
    _SYSTEMD_SLICE=-.slice
    UNIT=dnf-makecache.service
    MESSAGE=Started dnf makecache.
    _HOSTNAME=host.lab.example.com
    INVOCATION_ID=d6f90184663f4309835a3e8ab647cb0e
    _SOURCE_REALTIME_TIMESTAMP=1550748674509128
lines 32239-32275/32275 (END) q
```

В следующем списке приведены общие поля системного журнала, которые можно использовать для поиска строк, относящихся к конкретному процессу или событию:

* _COMM ― имя команды
* _EXE ― путь к исполняемому объекту для процесса
* _PID ― PID процесса
* _UID ― UID пользователя, выполняющего процесс
* _SYSTEMD_UNIT ― юнит systemd, запустивший процесс


Можно объединить несколько полей системного журнала для формирования подробного поискового запроса с помощью команды `journalctl`. Например, следующая команда `journalctl` отображает все записи журнала, связанные с юнитом **sshd.service** systemd, от процесса с PID 1182.

```
[root@host ~]# journalctl _SYSTEMD_UNIT=sshd.service _PID=1182
Apr 03 19:34:27 host.lab.example.com sshd[1182]: Accepted password for root from ::1 port 52778 ssh2
Apr 03 19:34:28 host.lab.example.com sshd[1182]: pam_unix(sshd:session): session opened for user root by (uid=0)
...output omitted...
```

<details>
<summary>Примечание</summary>

Список часто используемых полей журнала см. на man-странице `systemd.journal-fields(7)`.
</details>