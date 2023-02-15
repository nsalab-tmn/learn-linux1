## Цели

Получение базовых навыков использования OpenSSH для входа в удалённую систему и выполнения в ней команд

## Задачи

В этом упражнении вы будете входить в удаленную систему от имени разных пользователей и выполнять команды.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab ssh-access start`, чтобы начать упражнение. Этот сценарий обеспечивает правильную настройку среды.

```
[student@workstation ~]$ lab ssh-access start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Установите SSH-подключение к **serverb** как пользователь *student*. Примите ключ хоста. Укажите *student* в качестве пароля, когда будет предложено ввести пароль для пользователя *student* на **serverb**.

  ```
  [student@servera ~]$ ssh student@serverb
  The authenticity of host 'serverb (172.25.250.11)' can't be established.
  ECDSA key fingerprint is SHA256:ERTdjooOIrIwVSZQnqD5or+JbXfidg0udb3DXBuHWzA.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added 'serverb,172.25.250.11' (ECDSA) to the list of known hosts.
  student@serverb's password: student
  ...output omitted...
  [student@serverb ~]$ 
  ```

  Ключ хоста записывается в файл **/home/student/.ssh/known_hosts** на **servera** для идентификации **serverb**, поскольку пользователь *student* запустил SSH-подключение с **servera**. Если файл **/home/student/.ssh/known_hosts** еще не существует, он будет создан с новой записью. Команда `ssh` не будет выполнена, если ключ удаленного хоста отличается от записанного ключа.

3.	Выполните команду `w`, чтобы отобразить пользователей, вошедших в систему **serverb**.

  ```
  [student@serverb ~]$ w
  18:49:29 up  2:55,  1 user,  load average: 0.00, 0.00, 0.00
  USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
  student  pts/0    172.25.250.10    18:33    0.00s  0.01s  0.00s w
  ```

  Приведенный выше вывод показывает, что пользователь *student* вошел в систему с хоста с IP-адресом 172.25.250.10, а это **servera** в сети учебной аудитории.

  <details>
  <summary>Примечание</summary>

  IP-адрес системы идентифицирует ее в сети. Вы узнаете об IP-адресах в следующей главе.
  </details>

4.	Выйдите из командной оболочки пользователя *student* на **serverb**.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@serverb ~]$ exit
  logout
  Connection to serverb closed.
  [student@servera ~]$ 
  ```
  </details>

5.	Установите SSH-подключение к **serverb** как пользователь *root*. Используйте *redhat* в качестве пароля пользователя *root*.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ssh root@serverb
  root@serverb's password: redhat
  ...output omitted...
  [root@serverb ~]# 
  ```
  </details>

  Обратите внимание, что предыдущая команда `ssh` не просила принять ключ хоста, поскольку он был найден среди известных хостов. Если идентификационные данные serverb изменятся, OpenSSH предложит проверить и принять новый ключ хоста.
  

6.	Выполните команду `w`, чтобы отобразить пользователей, вошедших в систему **serverb**.

  ```
  [root@serverb ~]# w
  19:10:28 up  3:16,  1 user,  load average: 0.00, 0.00, 0.00
  USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
  root     pts/0    172.25.250.10    19:09    1.00s  0.01s  0.00s w
  ```

  Приведенный выше вывод показывает, что пользователь *root* вошел в систему с хоста с IP-адресом 172.25.250.10, а это **servera** в сети учебной аудитории.

7.	Выйдите из командной оболочки пользователя *root* на **serverb**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@serverb ~]# exit
  logout
  Connection to serverb closed.
  [student@servera ~]$ 
  ```
  </details>

8.	Удалите файл **/home/student/.ssh/known_hosts** с **servera**. Это приведет к тому, что **ssh** потеряет записанные идентификационные данные удаленных систем.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ rm /home/student/.ssh/known_hosts
  ```
  </details>

  Ключи хоста могут меняться по обоснованным причинам, например, если удаленный компьютер был заменен из-за аппаратного сбоя или удаленный компьютер был переустановлен. Обычно рекомендуется удалять только запись ключа определенного хоста в файле **known_hosts**. Поскольку этот конкретный файл **known_hosts** имеет только одну запись, вы можете удалить весь файл.

9.	Установите SSH-подключение к **serverb** как пользователь *student*. При запросе примите ключ хоста. Укажите *student* в качестве пароля, когда будет предложено ввести пароль для пользователя *student* на **serverb**.

  ```
  [student@servera ~]$ ssh student@serverb
  The authenticity of host 'serverb (172.25.250.11)' can't be established.
  ECDSA key fingerprint is SHA256:ERTdjooOIrIwVSZQnqD5or+JbXfidg0udb3DXBuHWzA.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added 'serverb,172.25.250.11' (ECDSA) to the list of known hosts.
  student@serverb's password: student
  ...output omitted...
  [student@serverb ~]$ 
  ```

  Обратите внимание, что команда `ssh` попросила вас подтвердить принятие или отклонение ключа хоста, поскольку не смогла найти ключ для удаленного хоста.

10.	Выйдите из командной оболочки пользователя *student* на **serverb** и убедитесь, что новый экземпляр **known_hosts** существует на **servera**.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@serverb ~]$ exit
  logout
  Connection to serverb closed.
  [student@servera ~]$ ls -l /home/student/.ssh/known_hosts
  -rw-r--r--. 1 student student 183 Feb  1 20:26 /home/student/.ssh/known_hosts
  ```
  </details>

11.	Убедитесь, что в новом экземпляре файла **known_hosts** есть ключ хоста **serverb**.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ cat /home/student/.ssh/known_hosts
  serverb,172.25.250.11 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBI9LEYEhwmU1rNqnbBPukH2Ba0/QBAu9WbS4m03B3MIhhXWKFFNa/UlNjY8NDpEM+hkJe/GmnkcEYMLbCfd9nMA=
  ```

  Фактический вывод может отличаться.
  </details>

12.	Удаленно выполните команду `hostname` на **serverb**, не обращаясь к интерактивной оболочке.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ssh student@serverb hostname
  student@serverb's password: student
  serverb.lab.example.com
  ```

  Предыдущая команда отображала полное имя хоста удаленной системы **serverb**.
  </details>

13.	Выйдите из командной оболочки пользователя *student* на **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  ```

## Конец

На **workstation** запустите сценарий `lab ssh-access finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab ssh-access finish
```

Упражнение завершено.


