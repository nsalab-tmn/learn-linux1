## Цели

Получение базовых навыков определения установленных юнитов служб и идентификации активных и включенных служб в системе.

## Задачи

В этом упражнении вы отобразите список установленных юнитов служб и идентифицируете службы, которые в данный момент включены и активны на сервере.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab services-identify start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста servera в сети.

```
[student@workstation ~]$ lab services-identify start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации, поэтому пароль для входа на **servera** не требуется.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Отобразите список всех юнитов служб, установленных на **servera**.

  ```
  [student@servera ~]$ systemctl list-units --type=service
  UNIT                 LOAD   ACTIVE SUB     DESCRIPTION
  atd.service          loaded active running Job spooling tools
  auditd.service       loaded active running Security Auditing Service
  chronyd.service      loaded active running NTP client/server
  crond.service        loaded active running Command Scheduler
  dbus.service         loaded active running D-Bus System Message Bus
  ...output omitted...
  ```

  Нажмите q, чтобы выйти из команды.


3.	Отобразите список всех юнитов сокетов (активных и неактивных) на **servera**.

  ```
  [student@servera ~]$ systemctl list-units --type=socket --all
  UNIT                 LOAD   ACTIVE   SUB       DESCRIPTION
  dbus.socket          loaded active   running   D-Bus System Message Bus Socket 
  dm-event.socket      loaded active   listening Device-mapper event daemon FIFOs
  lvm2-lvmpolld.socket loaded active   listening LVM2 poll daemon socket
  ...output omitted...
  systemd-udevd-control.socket    loaded active   running   udev Control Socket
  systemd-udevd-kernel.socket     loaded active   running   udev Kernel Socket

  LOAD   = Reflects whether the unit definition was properly loaded.
  ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
  SUB    = The low-level unit activation state, values depend on unit type.

  12 loaded units listed.
  To show all installed unit files use 'systemctl list-unit-files'.
  ```

4.	Проверьте состояние службы **chronyd**. Эта служба используется для сетевой синхронизации времени (NTP).

  4.1.	Отобразите состояние службы chronyd. Обратите внимание на идентификатор процесса любого активного демона.

  ```
  [student@servera ~]$ systemctl status chronyd
  ● chronyd.service - NTP client/server
    Loaded: loaded (/usr/lib/systemd/system/chronyd.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2019-02-06 12:46:57 IST; 4h 7min ago
      Docs: man:chronyd(8)
            man:chrony.conf(5)
    Process: 684 ExecStartPost=/usr/libexec/chrony-helper update-daemon (code=exited, status=0/SUCCESS)
    Process: 673 ExecStart=/usr/sbin/chronyd $OPTIONS (code=exited, status=0/SUCCESS)
    Main PID: 680 (chronyd)
      Tasks: 1 (limit: 11406)
    Memory: 1.5M
    CGroup: /system.slice/chronyd.service
            └─680 /usr/sbin/chronyd

  ... servera.lab.example.com systemd[1]: Starting NTP client/server...
  ...output omitted...
  ... servera.lab.example.com systemd[1]: Started NTP client/server.
  ... servera.lab.example.com chronyd[680]: Source 172.25.254.254 offline
  ... servera.lab.example.com chronyd[680]: Source 172.25.254.254 online
  ... servera.lab.example.com chronyd[680]: Selected source 172.25.254.254
  ```

  Нажмите q, чтобы выйти из команды.

  4.2.	Убедитесь, что указанный демон работает. В приведенном выше выводе идентификатор процесса, связанного с **chronyd**, ― 680. В вашей системе этот идентификатор может быть другим.

  ```
  [student@servera ~]$ ps -p 680
    PID TTY          TIME CMD
    680 ?        00:00:00 chronyd
  ```

5.	Проверьте состояние службы **sshd**. Эта служба используется для установки безопасного шифрованного подключения между системами.

  5.1.	Определите, включена ли служба **sshd** для запуска во время начальной загрузки системы.

  ```
  [student@servera ~]$ systemctl is-enabled sshd
  enabled
  ```

  5.2.	Определите, является ли служба **sshd** активной, не отображая всю информацию о ее состоянии.

  ```
  [student@servera ~]$ systemctl is-active sshd
  active
  ```

  5.3.	Отобразите состояние службы **sshd**.

  ```
  [student@servera ~]$ systemctl status sshd
  ● sshd.service - OpenSSH server daemon
    Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2019-02-06 12:46:58 IST; 4h 21min ago
      Docs: man:sshd(8)
            man:sshd_config(5)
  Main PID: 720 (sshd)
      Tasks: 1 (limit: 11406)
    Memory: 5.8M
    CGroup: /system.slice/sshd.service
    └─720 /usr/sbin/sshd -D -oCiphers=aes256-gcm@openssh.com,
    chacha20-poly1305@openssh.com,aes256-ctr,
    aes256-cbc,aes128-gcm@openssh.com,aes128-ctr,
    aes128-cbc -oMACs=hmac-sha2-256-etm@openssh.com,hmac-sha>

  ... servera.lab.example.com systemd[1]: Starting OpenSSH server daemon...
  ... servera.lab.example.com sshd[720]: Server listening on 0.0.0.0 port 22.
  ... servera.lab.example.com systemd[1]: Started OpenSSH server daemon.
  ... servera.lab.example.com sshd[720]: Server listening on :: port 22.
  ...output omitted...
  ... servera.lab.example.com sshd[1380]: pam_unix(sshd:session): session opened for user student by (uid=0)
  ```

  Нажмите q, чтобы выйти из команды.

6.	Отобразите список состояний всех юнитов служб (включенных и отключенных).

  ```
  [student@servera ~]$ systemctl list-unit-files --type=service
  UNIT FILE                    STATE          
  arp-ethers.service           disabled       
  atd.service                  enabled        
  auditd.service               enabled        
  auth-rpcgss-module.service   static         
  autovt@.service              enabled        
  blk-availability.service     disabled       
  chrony-dnssrv@.service       static         
  chrony-wait.service          disabled       
  chronyd.service              enabled 
  ...output omitted...
  ```

  Нажмите q, чтобы выйти из команды.

7.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab services-identify finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab services-identify finish
```

Упражнение завершено.
