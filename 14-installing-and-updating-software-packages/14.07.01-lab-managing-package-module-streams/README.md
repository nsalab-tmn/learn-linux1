## Цели

Получение базовых навыков управления потоками модулей

## Задачи

В этом упражнении вы отобразите список доступных модулей, включите определенный поток модуля и установите пакеты из этого потока.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab software-module start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети. Сценарий также проверяет доступность необходимых репозиториев программного обеспечения и устанавливает модуль **postgresql:9.6**.

```
[student@workstation ~]$ lab software-module start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Переключитесь на пользователя *root* в командной строке.

  ```
  [student@servera ~]$ sudo -i
  [sudo] password for student: student
  [root@servera ~]# 
  ```

3.	Отобразите доступные модули, потоки и установленные модули. Просмотрите информацию о модуле **python36**.

  3.1.	Выполните команду `yum module list`, чтобы отобразить список доступных модулей и потоков.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module list
  Red Hat Enterprise Linux 8.2 AppStream (dvd)
  Name          Stream         Profiles             Summary
  ...output omitted...
  python27      2.7 [d]        common [d]           Python ... version 2.7
  python36      3.6 [d][e]     build, common [d]    Python ... version 3.6
  python38      3.8 [d]        build, common [d]    Python ... version 3.8
  ...output omitted...
  Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
  ```
  </details>

  3.2.	Выполните команду `yum module list --installed`, чтобы отобразить список установленных модулей и потоков.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module list --installed
  Red Hat Enterprise Linux 8.2 AppStream (dvd)
  Name         Stream    Profiles                 Summary
  postgresql   9.6 [e]   client, server [d] [i]   PostgreSQL server and client ...

  Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
  ```
  </details>

  3.3.	Выполните команду `yum module info`, чтобы просмотреть информацию о модуле **python36**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module info python36
  Name             : python36
  Stream           : 3.6 [d][e][a]
  Version          : 8010020190724083915
  Context          : a920e634
  Architecture     : x86_64
  Profiles         : build, common [d]
  Default profiles : common
  Repo             : rhel-8.2-for-x86_64-appstream-rpms
  Summary          : Python programming language, version 3.6
  ...output omitted...
  Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled, [a]ctive]
  ```
  </details>

4.	Установите модуль **python36** из потока 3.6 и профиля **common**. Проверьте текущее состояние модуля.

  4.1.	Выполните команду `yum module install`, чтобы установить модуль **python36**. Используйте синтаксис `имя:поток/профиль`, чтобы установить модуль **python36** из потока **3.6** и профиля **common**.

  <details>
  <summary>Примечание</summary>

  Вы можете опустить /профиль, чтобы использовать профиль по умолчанию, и :поток, чтобы использовать поток по умолчанию.
  </details>

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module install python36:3.6/common
  ...output omitted...
  Is this ok [y/N]: y
  ...output omitted...
  Complete!
  ```
  </details>

  4.2.	Просмотрите текущее состояние модуля **python36**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module list python36
  Red Hat Enterprise Linux 8.2 AppStream (dvd)
  Name         Stream         Profiles                   Summary
  python36     3.6 [d][e]     build, common [d] [i]      Python ... version 3.6

  Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
  ```
  </details>

5.	Переключитесь на модуль **postgresql** профиля **server**, чтобы использовать поток 10.

  5.1.	Выполните команду `yum module list` для отображения модуля и потока **postgresql**. Обратите внимание, что поток модуля **postgresql:9.6** в настоящее время установлен.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module list postgresql
  Red Hat Enterprise Linux 8.2 AppStream (dvd)
  Name         Stream    Profiles                Summary
  postgresql   9.6 [e]   client, server [d] [i]  PostgreSQL server and client ...
  postgresql   10 [d]    client, server [d]      PostgreSQL server and client ...
  postgresql   12        client, server [d]      PostgreSQL server and client ...

  Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
  ```
  </details>

  5.2.	Удалите и отключите поток модуля **postgresql** вместе со всеми пакетами, установленными профилем.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module remove postgresql
  ...output omitted...
  Is this ok [y/N]: y
  ...output omitted...
  Removed:
    postgresql-server-9.6.10-1.module+el8+2470+d1bafa0e.x86_64   libpq-10.5-1.el8.x86_64  postgresql-9.6.10-1.module+el8+2470+d1bafa0e.x86_64
  Complete
  ```
  </details>

  5.3.	Выполните сброс модуля PostgreSQL и его потоков.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module reset postgresql
  =================================================================
  Package       Arch             Version     Repository      Size
  =================================================================
  Resetting modules:
  postgresql

  Transaction Summary
  =================================================================

  Is this ok [y/N]: y
  Complete!
  ```
  </details>

  5.4.	Выполните команду `yum module install`, чтобы переключиться на поток модуля **postgresql:10**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module install postgresql:10
  ...output omitted...
  Is this ok [y/N]: y
  ...output omitted...
  Complete!
  ```
  </details>

  5.5.	Убедитесь, что модуль **postgresql** переключен на поток **10**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module list postgresql
  Red Hat Enterprise Linux 8.2 AppStream (dvd)
  Name         Stream    Profiles                Summary
  postgresql   9.6       client, server [d]      PostgreSQL server and client ...
  postgresql   10 [d][e] client, server [d] [i]  PostgreSQL server and client ...
  postgresql   12        client, server [d]      PostgreSQL server and client ...

  Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
  ```
  </details>

6.	Удалите и отключите поток модуля **postgresql** вместе со всеми пакетами, установленными профилем.

  6.1.	Выполните команду `yum module remove`, чтобы удалить модуль **postgresql**. Эта команда также удаляет все пакеты, установленные из этого модуля.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module remove postgresql
  ...output omitted...
  Is this ok [y/N]: y
  ...output omitted...
  Complete!
  ```
  </details>

  6.2.	Отключите поток модуля **postgresql**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module disable postgresql
  ...output omitted...
  Is this ok [y/N]: y
  ...output omitted...
  Complete!
  ```
  </details>

  6.3.	Убедитесь, что поток модуля **postgresql** удален и отключен.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum module list postgresql
  Red Hat Enterprise Linux 8.2 AppStream (dvd)
  Name          Stream     Profiles               Summary
  postgresql    9.6 [x]    client, server [d]     PostgreSQL server and client ...
  postgresql    10 [d][x]  client, server [d]     PostgreSQL server and client ...
  postgresql    12 [x]     client, server [d]     PostgreSQL server and client ...

  Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
  ```
  </details>

7.	Выйдите с **servera**.

  ```
  [root@servera ~]# exit
  logout
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab software-module finish`, чтобы закончить упражнение. Этот сценарий удаляет все модули, установленные на servera во время упражнения.

```
[student@workstation ~]$ lab software-module finish
```

Упражнение завершено.