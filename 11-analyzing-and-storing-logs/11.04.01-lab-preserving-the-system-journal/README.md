## Цели

Получение базовых навыков настройки системного журнала для сохранения данных

## Задачи

В этом упражнении вы настроите системный журнал на сохранение данных после перезагрузки.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab log-preserve start`, чтобы начать упражнение. Этот сценарий обеспечивает правильную настройку среды.

```
[student@workstation ~]$ lab log-preserve start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Как привилегированный пользователь убедитесь, что каталог **/var/log/journal** не существует. Используйте команду `ls` для отображения содержимого каталога **/var/log/journal**. Используйте команду `sudo`, чтобы повысить права пользователя *student*. При необходимости используйте *student* в качестве пароля.

  ```
  [student@servera ~]$ sudo ls /var/log/journal
  [sudo] password for student: student
  ls: cannot access '/var/log/journal': No such file or directory
  ```

  Поскольку каталог **/var/log/journal** не существует, служба systemd-journald не сохраняет свои журналы.

3.	Настройте службу systemd-journald на машине **servera** таким образом, чтобы журналы сохранялись при перезагрузке системы.

  3.1.	Раскомментируйте строку **Storage=auto** в файле **/etc/systemd/journald.conf** и задайте для переменной **Storage** значение *persistent*. Используйте команду `sudo vim /etc/systemd/journald.conf`, чтобы отредактировать файл конфигурации. Введите `/ Storage=auto` в командном режиме `vim`, чтобы найти строку **Storage=auto**.

  ```
  ...output omitted...
  [Journal]
  Storage=persistent
  ...output omitted...
  ```

  3.2.	С помощью команды `systemctl` перезапустите службу **systemd-journald**, чтобы изменения конфигурации вступили в силу.

  ```
  [student@servera ~]$ sudo systemctl restart systemd-journald.service
  ```

4.	Убедитесь, что журналы службы systemd-journald на **servera** сохраняются при перезагрузке.

  4.1.	Выполните команду `systemctl reboot`, чтобы перезапустить **servera**.

  ```
  [student@servera ~]$ sudo systemctl reboot
  Connection to servera closed by remote host.
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

  Обратите внимание, что SSH-подключение было прервано, как только вы перезапустили систему **servera**.

  4.2.	Повторно установите SSH-подключение к **servera**.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

  4.3.	Выполните команду `ls`, чтобы убедиться, что каталог **/var/log/journal** существует. Каталог **/var/log/journal** содержит подкаталог с длинным шестнадцатеричным именем. Файлы журнала находятся в этом каталоге. Имя подкаталога в вашей системе будет другим.

  ```
  [student@servera ~]$ sudo ls /var/log/journal
  [sudo] password for student: student
  73ab164e278e48be9bf80e80714a8cd5
  [student@servera ~]$ sudo ls \
  /var/log/journal/73ab164e278e48be9bf80e80714a8cd5
  system.journal  user-1000.journal
  ```

  4.4.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  ```

## Конец

На **workstation** запустите сценарий `lab log-preserve finish`, чтобы закончить упражнение. Этот сценарий обеспечивает очистку среды.

```
[student@workstation ~]$ lab log-preserve finish
```

Упражнение завершено.



