## Цели

Получение базовых навыков поиска файлов в файловой системе

## Задачи

В этом упражнении вы найдете определенные файлы в смонтированных файловых системах, используя команды `find` и `locate`.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab fs-locate start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети.

```
[student@workstation ~]$ lab fs-locate start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Используйте команду `locate` для поиска файлов на **servera**.

  2.1.	Хотя база данных `locate` обновляется автоматически каждый день, убедитесь, что она находится в актуальном состоянии, для чего вручную запустите обновление на **servera**. 

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ sudo updatedb
  [sudo] password for student: student
  [student@servera ~]$ 
  ```
  </details>

  2.2.	Найдите файл конфигурации **logrotate.conf**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ locate logrotate.conf
  /etc/logrotate.conf
  /usr/share/man/man5/logrotate.conf.5.gz
  ```
  </details>

  2.3.	Найдите файл конфигурации **networkmanager.conf** без учета регистра.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ locate -i networkmanager.conf
  /etc/NetworkManager/NetworkManager.conf
  /etc/dbus-1/system.d/org.freedesktop.NetworkManager.conf
  /usr/share/man/man5/NetworkManager.conf.5.gz
  ```
  </details>

3.	С помощью команды `find` выполните поиск файлов в реальном времени на **servera** в соответствии со следующими требованиями.

  3.1.	Найдите все файлы в каталоге **/var/lib**, принадлежащие пользователю *chrony*.

  <details>
  <summary>Показать решение</summary>

  Используйте команду `sudo`, так как файлы внутри каталога **/var/lib** принадлежат пользователю *root*.

  ```
  [student@servera ~]$ sudo find /var/lib -user chrony
  [sudo] password for student: student
  /var/lib/chrony
  /var/lib/chrony/drift
  ```
  </details>

  3.2.	Отобразите все файлы в каталоге **/var**, владельцем которых являются пользователь *root* и группа *mail*.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ sudo find /var -user root -group mail
  /var/spool/mail
  ```
  </details>

  3.3.	Отобразите все файлы в каталоге **/usr/bin**, размер которых превышает 50 КБ.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ find /usr/bin -size +50k
  /usr/bin/iconv
  /usr/bin/locale
  /usr/bin/localedef
  /usr/bin/cmp
  ...output omitted...
  ```
  </details>

  3.4.	Найдите все файлы в каталоге **/home/student**, которые не изменялись в последние 120 минут.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ find /home/student -mmin +120
  /home/student/.bash_logout
  /home/student/.bash_profile
  /home/student/.bashrc
  ...output omitted...
  ```
  </details>

  3.5.	Отобразите все файлы блочных устройств в каталоге **/dev**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ find /dev -type b
  /dev/vdd
  /dev/vdc
  /dev/vdb
  /dev/vda3
  /dev/vda2
  /dev/vda1
  /dev/vda
  ```
  </details>

4.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab fs-locate finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab fs-locate finish
```

Упражнение завершено.