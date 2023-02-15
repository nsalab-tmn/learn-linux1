## Цели

Получение базовых навыков проверки сетевой конфигурации в Linux

## Задачи

В этом упражнении вы проверите сетевую конфигурацию одного из ваших серверов.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab net-validate start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети.

```
[student@workstation ~]$ lab net-validate start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации и беспарольного доступа к **servera**.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Найдите имя сетевого интерфейса, связанное с Ethernet-адресом 52:54:00:00:fa:0a. Запишите или запомните это имя и используйте его вместо заполнителя enX в последующих командах.

  <details>
  <summary>Важно</summary>

  Имена сетевых интерфейсов определяются типом шины и порядком обнаружения устройств во время начальной загрузки. Имена сетевых интерфейсов будут различаться в зависимости от платформы и оборудования, используемых в этом курсе.
  </details>

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ip link
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
  3.	Отобразите текущие значения IP-адреса и маски сети для всех интерфейсов.
  [student@servera ~]$ ip addr
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
      inet6 ::1/128 scope host 
        valid_lft forever preferred_lft forever
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
      inet 172.25.250.10/24 brd 172.25.250.255 scope global noprefixroute ens3
        valid_lft forever preferred_lft forever
      inet6 fe80::3059:5462:198:58b2/64 scope link noprefixroute 
        valid_lft forever preferred_lft forever
  ```
  </details>

4.	Отобразите статистику для интерфейса enX.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ip -s link show enX
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
      link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
      RX: bytes  packets  errors  dropped overrun mcast   
      89014225   168251   0       154418  0       0       
      TX: bytes  packets  errors  dropped carrier collsns 
      608808     6090     0       0       0       0
  ```
  </details>

5.	Отобразите информацию о маршрутизации.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ip route
  default via 172.25.250.254 dev enX proto static metric 100
  172.25.250.0/24 dev enX proto kernel scope link src 172.25.250.10 metric 100
  ```
  </details>

6.	Убедитесь, что маршрутизатор доступен.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ping -c3 172.25.250.254
  PING 172.25.250.254 (172.25.250.254) 56(84) bytes of data.
  64 bytes from 172.25.250.254: icmp_seq=1 ttl=64 time=0.196 ms
  64 bytes from 172.25.250.254: icmp_seq=2 ttl=64 time=0.436 ms
  64 bytes from 172.25.250.254: icmp_seq=3 ttl=64 time=0.361 ms

  --- 172.25.250.254 ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss, time 49ms
  rtt min/avg/max/mdev = 0.196/0.331/0.436/0.100 ms
  ```
  </details>

7.	Отобразите все транзитные переходы между локальной системой и classroom.example.com.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ tracepath classroom.example.com
  1?: [LOCALHOST]                      pmtu 1500
  1:  workstation.lab.example.com                           0.270ms 
  1:  workstation.lab.example.com                           0.167ms 
  2:  classroom.example.com                                 0.473ms reached
      Resume: pmtu 1500 hops 2 back 2
  ```
  </details>

8.	Отобразите прослушиваемые сокеты TCP в локальной системе.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ss -lt
  State      Recv-Q Send-Q      Local Address:Port         Peer Address:Port
  LISTEN     0      128               0.0.0.0:sunrpc            0.0.0.0:*   
  LISTEN     0      128               0.0.0.0:ssh               0.0.0.0:*   
  LISTEN     0      128                  [::]:sunrpc               [::]:*   
  LISTEN     0      128                  [::]:ssh                  [::]:*   
  ```
  </details>

9.	Выйдите с **servera**.

```
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```

## Конец

На **workstation** запустите сценарий `lab net-validate finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab net-validate finish
```

Упражнение завершено.