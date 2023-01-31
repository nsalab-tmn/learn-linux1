## Цели

Получение базовых навыков настройки статических имён хостов и разрешения имён

## Задачи

В этом упражнении вы вручную настроите статическое имя хоста системы, файл /etc/hosts и разрешение имен DNS.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab net-hostnames start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети.

```
[student@workstation ~]$ lab net-hostnames start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации, поэтому пароль для входа на **servera** не требуется.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Просмотрите текущие параметры имени хоста.

  2.1.	Отобразите текущее имя хоста.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ hostname
  servera.lab.example.com
  ```
  </details>

  2.2.	Отобразите состояние имени хоста.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ hostnamectl status
    Static hostname: n/a
  Transient hostname: servera.lab.example.com
          Icon name: computer-vm
            Chassis: vm
          Machine ID: f874df04639f474cb0a9881041f4f7d4
            Boot ID: 22ae5279f57049678eda547bdb39a19d
      Virtualization: kvm
    Operating System: Red Hat Enterprise Linux 8.2 (Ootpa)
        CPE OS Name: cpe:/o:redhat:enterprise_linux:8.2:GA
              Kernel: Linux 4.18.0-193.el8.x86_64
        Architecture: x86-64
  ```
  </details>

  Обратите внимание на временное имя хоста (`Transient hostname`), полученное от DHCP или mDNS.

3.	Задайте статическое имя хоста, чтобы оно соответствовало текущему временному имени хоста.

  3.1.	Измените имя хоста и файл конфигурации имен хостов.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ sudo hostnamectl set-hostname \
  servera.lab.example.com
  [sudo] password for student: student
  [student@servera ~]$ 
  ```
  </details>

  3.2.	Просмотрите содержимое файла **/etc/hostname**, предоставляющего имя хоста при запуске сети.

  ```
  servera.lab.example.com
  ```

  3.3.	Отобразите состояние имени хоста.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ hostnamectl status
    Static hostname: servera.lab.example.com
          Icon name: computer-vm
            Chassis: vm
          Machine ID: f874df04639f474cb0a9881041f4f7d4
            Boot ID: 22ae5279f57049678eda547bdb39a19d
      Virtualization: kvm
    Operating System: Red Hat Enterprise Linux 8.2 (Ootpa)
        CPE OS Name: cpe:/o:redhat:enterprise_linux:8.2:GA
              Kernel: Linux 4.18.0-193.el8.x86_64
        Architecture: x86-64
  ```
  </details>

  Обратите внимание, что временное имя хоста теперь не отображается, так как было настроено статическое имя хоста.

4.	Временно измените имя хоста.

  4.1.	Измените имя хоста на **testname**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ sudo hostname testname
  ```
  </details>

  4.2.	Отобразите текущее имя хоста.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ hostname
  testname
  ```
  </details>

  4.3.	Просмотрите содержимое файла **/etc/hostname**, предоставляющего имя хоста при запуске сети.

  ```
  servera.lab.example.com
  ```

  4.4.	Перезагрузите систему.

  ```
  [student@servera ~]$ sudo systemctl reboot
  Connection to servera closed by remote host.
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

  4.5.	С **workstation** войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

  4.6.	Отобразите текущее имя хоста.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ hostname
  servera.lab.example.com
  ```
  </details>

5.	Добавьте локальный псевдоним для сервера учебной аудитории.

  5.1.	Найдите IP-адрес хоста **classroom.example.com**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ host classroom.example.com
  classroom.example.com has address 172.25.254.254
  ```
  </details>

  5.2.	Измените файл **/etc/hosts** таким образом, чтобы можно было использовать дополнительное имя **class** для доступа к IP-адресу `172.25.254.254`.

  <details>
  <summary>Показать решение</summary>
  ```
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

  172.25.254.254 classroom.example.com classroom class
  172.25.254.254 content.example.com content
  ...content omitted...
  ```
  </details>

  5.3.	Найдите IP-адрес для **class**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ host class
  Host class not found: 2(SERVFAIL)
  [student@servera ~]$ getent hosts class
  172.25.254.254    classroom.example.com class
  ```
  </details>

  5.4.	Выполните `ping` для **class**.

  ```
  [student@servera ~]$ ping -c3 class
  PING classroom.example.com (172.25.254.254) 56(84) bytes of data.
  64 bytes from classroom.example.com (172.25.254.254): icmp_seq=1 ttl=64 time=0.397 ms
  64 bytes from classroom.example.com (172.25.254.254): icmp_seq=2 ttl=64 time=0.447 ms
  64 bytes from classroom.example.com (172.25.254.254): icmp_seq=3 ttl=64 time=0.470 ms

  --- classroom.example.com ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss, time 2000ms
  rtt min/avg/max/mdev = 0.397/0.438/0.470/0.030 ms
  ```

  5.5.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab net-hostnames finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab net-hostnames finish
```

Упражнение завершено.

