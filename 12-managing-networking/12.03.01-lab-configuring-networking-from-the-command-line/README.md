## Цели

Получение базовых навыков настройки сетевой конфигурации в Linux с помощью утилиты nmcli

## Задачи

В этом упражнении вы настроите сетевые параметры с помощью команды nmcli.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab net-configure start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети.

```
[student@workstation ~]$ lab net-configure start
```

<details>
<summary>Примечание</summary>

Если команда sudo запросит пароль пользователя student, введите student.
</details>

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации, поэтому пароль для входа на **servera** не требуется.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Найдите имена сетевых интерфейсов.

  <details>
  <summary>Важно</summary>

  Имена сетевых интерфейсов определяются типом шины и порядком обнаружения устройств во время начальной загрузки. Имена сетевых интерфейсов будут различаться в зависимости от платформы и оборудования, используемых в этом курсе.
  </details>

  В своей системе найдите имя интерфейса (например, ens06 или en1p2), связанное с Ethernet-адресом 52:54:00:00:fa:0a. Используйте это имя интерфейса вместо заполнителя enX, который используется в этом упражнении.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ ip link
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
  ```
  </details>

3.	Просмотрите параметры сети с помощью команды nmcli.


  3.1.	Отобразите все подключения.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ nmcli con show
  NAME                UUID                                  TYPE      DEVICE
  Wired connection 1  03da038a-3257-4722-a478-53055cc90128  ethernet  enX
  ```
  </details>

  3.2.	Отобразите только активное подключение.

  Имя вашего сетевого интерфейса указано столбце **DEVICE**, а имя активного подключения для этого устройства указано в той же строке в столбце **NAME**. В этом упражнении активное подключение ― это **Wired connection 1**.

  Если имя вашего активного подключения отличается, используйте его вместо **Wired connection 1** в оставшейся части упражнения.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ nmcli con show --active
  NAME                UUID                                  TYPE      DEVICE
  Wired connection 1  03da038a-3257-4722-a478-53055cc90128  ethernet  enX
  ```
  </details>

  3.3.	Отобразите все параметры конфигурации для активного подключения.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ nmcli con show "Wired connection 1"
  connection.id:               Wired connection 1
  connection.uuid:             03da038a-3257-4722-a478-53055cc90128
  connection.stable-id:        --
  connection.type:             802-3-ethernet
  connection.interface-name:   --
  connection.autoconnect:      yes
  ...output omitted...
  ipv4.method:                 manual
  ipv4.dns:                    172.25.250.254
  ipv4.dns-search:             lab.example.com,example.com
  ipv4.dns-options:            ""
  ipv4.dns-priority:           0
  ipv4.addresses:              172.25.250.10/24
  ipv4.gateway:                172.25.250.254
  ...output omitted...
  GENERAL.NAME:                Wired connection 1
  GENERAL.UUID:                03da038a-3257-4722-a478-53055cc90128
  GENERAL.DEVICES:             enX
  GENERAL.STATE:               activated
  GENERAL.DEFAULT:             yes
  GENERAL.DEFAULT6:            no
  GENERAL.SPEC-OBJECT:         --
  GENERAL.VPN:                 no
  GENERAL.DBUS-PATH:           /org/freedesktop/NetworkManager/ActiveConnection/1
  GENERAL.CON-PATH:            /org/freedesktop/NetworkManager/Settings/1
  GENERAL.ZONE:                --
  GENERAL.MASTER-PATH:         --
  IP4.ADDRESS[1]:              172.25.250.10/24
  IP4.GATEWAY:                 172.25.250.254
  IP4.ROUTE[1]:                dst = 172.25.250.0/24, nh = 0.0.0.0, mt = 100
  IP4.ROUTE[2]:                dst = 0.0.0.0/0, nh = 172.25.250.254, mt = 100
  IP4.DNS[1]:                  172.25.250.254
  IP6.ADDRESS[1]:              fe80::3059:5462:198:58b2/64
  IP6.GATEWAY:                 --
  IP6.ROUTE[1]:                dst = fe80::/64, nh = ::, mt = 100
  IP6.ROUTE[2]:                dst = ff00::/8, nh = ::, mt = 256, table=255
  ```
  Нажмите q, чтобы выйти из команды.
  </details>

  3.4.	Отобразите состояние устройства.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ nmcli dev status
  DEVICE  TYPE      STATE      CONNECTION
  enX     ethernet  connected  Wired connection 1
  lo      loopback  unmanaged  --
  3.5.	Отобразите параметры для устройства enX.
  [student@servera ~]$ nmcli dev show enX
  GENERAL.DEVICE:              enX
  GENERAL.TYPE:                ethernet
  GENERAL.HWADDR:              52:54:00:00:FA:0A
  GENERAL.MTU:                 1500
  GENERAL.STATE:               100 (connected)
  GENERAL.CONNECTION:          Wired connection 1
  GENERAL.CON-PATH:            /org/freedesktop/NetworkManager/ActiveConnection/1
  WIRED-PROPERTIES.CARRIER:    on
  IP4.ADDRESS[1]:              172.25.250.10/24
  IP4.GATEWAY:                 172.25.250.254
  IP4.ROUTE[1]:                dst = 172.25.250.0/24, nh = 0.0.0.0, mt = 100
  IP4.ROUTE[2]:                dst = 0.0.0.0/0, nh = 172.25.250.254, mt = 100
  IP4.DNS[1]:                  172.25.250.254
  IP6.ADDRESS[1]:              fe80::3059:5462:198:58b2/64
  IP6.GATEWAY:                 --
  IP6.ROUTE[1]:                dst = fe80::/64, nh = ::, mt = 100
  IP6.ROUTE[2]:                dst = ff00::/8, nh = ::, mt = 256, table=255
  ```
  </details>

4.	Создайте статическое подключение с тем же IPv4-адресом, сетевым префиксом и шлюзом по умолчанию. Назовите новое подключение **static-addr**.

  <details>
  <summary>Предупреждение</summary>

  Поскольку доступ к вашей машине предоставляется через основное сетевое подключение, задание неправильных значений во время настройки сети может сделать вашу машину недоступной. Если это произойдет, сбросьте машину и повторите попытку.
  </details>

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ sudo nmcli con add con-name "static-addr" ifname enX \
  type ethernet ipv4.method manual \
  ipv4.address 172.25.250.10/24 ipv4.gateway 172.25.250.254
  Connection 'static-addr' (15aa3901-555d-40cb-94c6-cea6f9151634) successfully added.
  ```
  </details>

5.	Измените новое подключение, добавив параметр DNS - 172.25.250.254.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ sudo nmcli con mod "static-addr" ipv4.dns 172.25.250.254
  ```
  </details>

6.	Отобразите и активируйте новое подключение. Убедитесь в том, что подключение активно

  <details>
  <summary>Показать решение</summary>

  6.1.	Просмотрите все подключения.

  ```
  [student@servera ~]$ nmcli con show
  NAME                UUID                                  TYPE      DEVICE
  Wired connection 1  03da038a-3257-4722-a478-53055cc90128  ethernet  enX
  static-addr         15aa3901-555d-40cb-94c6-cea6f9151634  ethernet  --
  ```

  6.2.	Посмотрите активное подключение.

  ```
  [student@servera ~]$ nmcli con show --active
  NAME                UUID                                  TYPE      DEVICE
  Wired connection 1  03da038a-3257-4722-a478-53055cc90128  ethernet  enX
  ```

  6.3.	Активируйте новое подключение static-addr.

  ```
  [student@servera ~]$ sudo nmcli con up "static-addr"
  Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/2)
  ```

  6.4.	Проверьте новое активное подключение.

  ```
  [student@servera ~]$ nmcli con show --active
  NAME         UUID                                  TYPE      DEVICE
  static-addr  15aa3901-555d-40cb-94c6-cea6f9151634  ethernet  enX
  ```
  </details>

7.	Настройте исходное подключение, чтобы оно не запускалось при начальной загрузке системы, и убедитесь, что после перезагрузки системы используется статическое подключение.

  <details>
  <summary>Показать решение</summary>

  7.1.	Отключите автоматический запуск исходного подключения при начальной загрузке системы.

  ```
  [student@servera ~]$ sudo nmcli con mod "Wired connection 1" \
  connection.autoconnect no
  ```

  7.2.	Перезагрузите систему.

  ```
  [student@servera ~]$ sudo systemctl reboot
  Connection to servera closed by remote host.
  Connection to servera closed.
  [student@workstation ~]$
  ```

  7.3.	Посмотрите активное подключение.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ nmcli con show --active
  NAME         UUID                                  TYPE      DEVICE
  static-addr  15aa3901-555d-40cb-94c6-cea6f9151634  ethernet  enX
  ```
  </details>

8.	Проверьте подключения, используя новые сетевые адреса.


  8.1.	Проверьте IP-адрес.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ ip addr show enX
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
      inet 172.25.250.10/24 brd 172.25.250.255 scope global noprefixroute enX
        valid_lft forever preferred_lft forever
      inet6 fe80::6556:cdd9:ce15:1484/64 scope link noprefixroute
        valid_lft forever preferred_lft forever
  ```
  </details>

  8.2.	Проверьте шлюз по умолчанию.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ ip route
  default via 172.25.250.254 dev enX proto static metric 100
  172.25.250.0/24 dev enX proto kernel scope link src 172.25.250.10 metric 100
  ```
  </details>

  8.3.	Выполните ping адреса DNS.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ ping -c3 172.25.250.254
  PING 172.25.250.254 (172.25.250.254) 56(84) bytes of data.
  64 bytes from 172.25.250.254: icmp_seq=1 ttl=64 time=0.225 ms
  64 bytes from 172.25.250.254: icmp_seq=2 ttl=64 time=0.314 ms
  64 bytes from 172.25.250.254: icmp_seq=3 ttl=64 time=0.472 ms

  --- 172.25.250.254 ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss, time 46ms
  rtt min/avg/max/mdev = 0.225/0.337/0.472/0.102 ms
  ```
  </details>

  8.4.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab net-configure finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab net-configure finish
```

Упражнение завершено.
