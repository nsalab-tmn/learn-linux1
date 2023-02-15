## Цели

Получение базовых навыков синхронизации файлов между системами

## Задачи

В этом упражнении вы синхронизируете содержимое локального каталога с копией на удаленном сервере, используя команду rsync и SSH.



## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab archive-sync start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хостов **servera** и **serverb** в сети. Сценарий также проверяет, что файл и каталог, которые будут созданы в упражнении, не существуют на **servera**.

```
[student@workstation ~]$ lab archive-sync start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	На **servera** создайте каталог с именем **/home/student/serverlogs**. Используйте команду `rsync` для безопасного создания начальной копии дерева каталогов **/var/log** с хоста **serverb** в каталоге **/home/student/serverlogs** на хосте **servera**.

  <details>
  <summary>Показать решение</summary>


  2.1.	На **servera** создайте целевой каталог с именем **/home/student/serverlogs** для хранения log-файлов, синхронизированных с хоста **serverb**.

  ```
  [student@servera ~]$ mkdir ~/serverlogs
  ```

  2.2.	С помощью команды `rsync` синхронизируйте дерево каталогов **/var/log** с хоста **serverb** в каталог **/home/student/serverlogs** на хосте **servera**. Обратите внимание, что только у пользователя *root* есть разрешение на чтение всего содержимого в каталоге **/var/log** на **serverb**. При начальной синхронизации переносятся все файлы.

  ```
  [student@servera ~]$ rsync -av root@serverb:/var/log ~/serverlogs
  root@serverb's password: redhat
  receiving incremental file list
  log/
  log/README
  log/boot.log
  ...output omitted...
  log/tuned/tuned.log

  sent 992 bytes  received 13,775,064 bytes  2,119,393.23 bytes/sec
  total size is 13,768,109  speedup is 1.00
  ```
  </details>

3.	От имени пользователя *root* на **serverb** выполните команду `logger "Log files synchronized"`, чтобы получить новую запись в log-файле **/var/log/messages**, отражающую время последней синхронизации.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ ssh root@serverb 'logger "Log files synchronized"'
  Password: redhat
  [student@servera ~]$ 
  ```
  </details>

4.	С помощью команды `rsync` безопасно синхронизируйте дерево каталогов **/var/log** с хоста **serverb** в каталог **/home/student/serverlog**s на хосте **servera**. Обратите внимание, что на этот раз переносятся только измененные log-файлы.

  <details>
  <summary>Показать решение</summary>

  ```
  [student@servera ~]$ rsync -av root@serverb:/var/log ~/serverlogs
  root@serverb's password: redhat 
  receiving incremental file list
  log/messages
  log/secure
  log/audit/audit.log

  sent 3,496 bytes  received 27,243 bytes  8,782.57 bytes/sec
  total size is 11,502,695  speedup is 374.21
  ```
  </details>

5.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab archive-sync finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab archive-sync finish
```

Упражнение завершено.
