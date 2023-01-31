## Цели

Получение базовых навыков настройки сетевой конфигурации путём редактирования файлов конфигурации

## Задачи

В этом упражнении вы вручную измените файлы конфигурации сети и убедитесь, что новые настройки вступили в силу.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab net-edit start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хостов **servera** и **serverb** в сети.

```
[student@workstation ~]$ lab net-edit start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации, поэтому пароль для входа на **servera** не требуется.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Найдите имена сетевых интерфейсов.

  Найдите имя сетевого интерфейса, связанное с Ethernet-адресом 52:54:00:00:fa:0a. Запишите или запомните это имя и используйте его вместо заполнителя enX в последующих командах. Имя активного подключения ― Wired connection 1 (соответственно, управление им осуществляется с помощью файла **/etc/sysconfig/network-scripts/ifcfg-Wired_connection_1**).

  Если вы выполнили предыдущие упражнения данной главы и в вашей системе подключение называлось именно так, то это имя останется и в текущем упражнении.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ ip link
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
  [student@servera ~]$ nmcli con show --active
  NAME                UUID                                  TYPE      DEVICE
  Wired connection 1  03da038a-3257-4722-a478-53055cc90128  ethernet  enX
  [student@servera ~]$ ls /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1
  /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1
  ```
  </details>

3.	На машине **servera** переключитесь на пользователя *root* и отредактируйте файл **/etc/sysconfig/network-scripts/ifcfg-Wired_connection_1**, добавив дополнительный адрес `10.0.1.1/24`.

  <details>
  <summary>Показать решение</summary>

  3.1.	Выполните команду `sudo -i`, чтобы переключиться на пользователя *root*. Пароль для пользователя *student* — *student*.

  ```
  [student@servera ~]$ sudo -i
  [sudo] password for student: student
  [root@servera ~]# 
  ```

  3.2.	Добавьте в файл запись, чтобы указать IPv4-адрес.

  ```
  [root@servera ~]# echo \
  "IPADDR1=10.0.1.1" >> /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1
  ```

  3.3.	Добавьте запись в файл, указав префикс сети.

  ```
  [root@servera ~]# echo "PREFIX1=24" >> /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1
  ```
  </details>

4.	Активируйте новый адрес.

  <details>
  <summary>Показать решение</summary>

  4.1.	Перезагрузите измененную конфигурацию.

  ```
  [root@servera ~]# nmcli con reload
  ```

  4.2.	Перезапустите подключение с новыми параметрами.

  ```
  [root@servera ~]# nmcli con up "Wired connection 1"
  Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/3)
  ```
  </details>

5.	Проверьте новый IP-адрес.

  ```
  [root@servera ~]# ip addr show enX
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
      inet 172.25.250.10/24 brd 172.25.250.255 scope global noprefixroute enX
        valid_lft forever preferred_lft forever
      inet 10.0.1.1/24 brd 10.0.1.255 scope global noprefixroute enX
        valid_lft forever preferred_lft forever
      inet6 fe80::4bf3:e1d9:3076:f8d7/64 scope link noprefixroute
        valid_lft forever preferred_lft forever
  ```

6.	Выйдите с **servera**, чтобы вернуться на **workstation** как пользователь *student*.

  ```
  [root@servera ~]# exit
  logout
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

7.	На **serverb** отредактируйте файл **/etc/sysconfig/network-scripts/ifcfg-Wired_connection_1**, добавив дополнительный адрес `10.0.1.2/24`, а затем загрузите новую конфигурацию.

  <details>
  <summary>Показать решение</summary>

  7.1.	С **workstation** с помощью команды `ssh` войдите на **serverb** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@serverb
  ...output omitted...
  [student@serverb ~]$ 
  ```

  7.2.	Выполните команду `sudo -i`, чтобы переключиться на пользователя *root*. Пароль для пользователя *student* — *student*.

  ```
  [student@serverb ~]$ sudo -i
  [sudo] password for student: student
  [root@serverb ~]# 
  ```

  7.3.	Измените файл **ifcfg-Wired_connection_1**, добавив второй IPv4-адрес и префикс сети.

  ```
  [root@serverb ~]# echo \
  "IPADDR1=10.0.1.2" >> /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1
  [root@serverb ~]# echo \
  "PREFIX1=24" >> /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1
  ```

  7.4.	Перезагрузите измененную конфигурацию.

  ```
  [root@serverb ~]# nmcli con reload
  ```

  7.5.	Установите подключение с новыми параметрами.

  ```
  [root@serverb ~]# nmcli con up "Wired connection 1"
  Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)
  ```

  7.6.	Проверьте новый IP-адрес.

  ```
  [root@serverb ~]# ip addr show enX
  2: enX: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 52:54:00:00:fa:0b brd ff:ff:ff:ff:ff:ff
      inet 172.25.250.11/24 brd 172.25.250.255 scope global noprefixroute enX
        valid_lft forever preferred_lft forever
      inet 10.0.1.2/24 brd 10.0.1.255 scope global noprefixroute enX
        valid_lft forever preferred_lft forever
      inet6 fe80::74c:3476:4113:463f/64 scope link noprefixroute
        valid_lft forever preferred_lft forever
  ```
  </details>

8.	Проверьте подключения, используя новые сетевые адреса.

  8.1.	С **serverb** отправьте ping-запрос на новый адрес **servera**.

  ```
  [root@serverb ~]# ping -c3 10.0.1.1
  PING 10.0.1.1 (10.0.1.1) 56(84) bytes of data.
  64 bytes from 10.0.1.1: icmp_seq=1 ttl=64 time=0.342 ms
  64 bytes from 10.0.1.1: icmp_seq=2 ttl=64 time=0.188 ms
  64 bytes from 10.0.1.1: icmp_seq=3 ttl=64 time=0.317 ms

  --- 10.0.1.1 ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss, time 35ms
  rtt min/avg/max/mdev = 0.188/0.282/0.342/0.068 ms
  ```

  8.2.	Выйдите с **serverb**, чтобы вернуться на **workstation**.

  ```
  [root@serverb ~]# exit
  logout
  [student@serverb ~]$ exit
  logout
  Connection to serverb closed.
  [student@workstation ~]$ 
  ```

  8.3.	На **workstation** выполните команду `ssh` для доступа к **servera** от имени пользователя *student*, чтобы отправить ping-запрос на новый адрес **serverb**.

  ```
  [student@workstation ~]$ ssh student@servera ping -c3 10.0.1.2
  PING 10.0.1.2 (10.0.1.2) 56(84) bytes of data.
  64 bytes from 10.0.1.2: icmp_seq=1 ttl=64 time=0.269 ms
  64 bytes from 10.0.1.2: icmp_seq=2 ttl=64 time=0.338 ms
  64 bytes from 10.0.1.2: icmp_seq=3 ttl=64 time=0.361 ms

  --- 10.0.1.2 ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss, time 48ms
  rtt min/avg/max/mdev = 0.269/0.322/0.361/0.044 ms
  ```

## Конец

На **workstation** запустите сценарий `lab net-edit finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab net-edit finish
```

Упражнение завершено.

