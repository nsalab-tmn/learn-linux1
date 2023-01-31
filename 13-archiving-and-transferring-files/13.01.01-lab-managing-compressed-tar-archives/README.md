## Цели

Получение базовых навыков работы с архивными файлами

## Задачи

В этом упражнении вы создадите архивные файлы и извлечете их содержимое с помощью команды tar.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На work**s**tation выполните команду `lab archive-manage start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети. Сценарий также проверяет, что файл и каталог, которые будут созданы в упражнении, не существуют на **servera**.

```
[student@workstation ~]$ lab archive-manage start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Переключитесь на пользователя *root*, поскольку только у пользователя *root* есть доступ ко всему содержимому каталога **/etc**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ su -
  Password: redhat
  [root@servera ~]# 
  ```
  </details>

3.	Выполните команду `tar` с опциями `-czf`, чтобы создать архив каталога **/etc** с использованием сжатия **gzip**. Сохраните файл архива как **/tmp/etc.tar.gz**.

  <details>
  <summary>Показать решение</summary>
  ```
  [root@servera ~]# tar -czf /tmp/etc.tar.gz /etc
  tar: Removing leading `/' from member names
  [root@servera ~]# 
  ```
  </details>

4.	Выполните команду `tar` с опциями `-tzf`, чтобы убедиться, что архив **etc.tar.gz** содержит файлы из каталога **/etc**.

  <details>
  <summary>Показать решение</summary>
  ```
  [root@servera ~]# tar -tzf /tmp/etc.tar.gz
  etc/
  etc/mtab
  etc/fstab
  etc/crypttab
  etc/resolv.conf
  ...output omitted...
  ```
  </details>

5.	На **servera** создайте каталог с именем **/backuptest**. Убедитесь, что файл резервной копии **etc.tar.gz** является допустимым архивом, для чего распакуйте его в каталог **/backuptest**.

  <details>
  <summary>Показать решение</summary>

  5.1.	Создайте каталог **/backuptest**.

  ```
  [root@servera ~]# mkdir /backuptest
  ```

  5.2.	Перейдите в каталог **/backuptest**.

  ```
  [root@servera ~]# cd /backuptest
  [root@servera backuptest]# 
  ```

  5.3.	Отобразите содержимое архива **etc.tar.gz** перед извлечением.

  ```
  [root@servera backuptest]# tar -tzf /tmp/etc.tar.gz
  etc/
  etc/mtab
  etc/fstab
  etc/crypttab
  etc/resolv.conf
  ...output omitted...
  ```

  5.4.	Извлеките содержимое архива **/tmp/etc.tar.gz** в каталог **/backuptest**.

  ```
  [root@servera backuptest]# tar -xzf /tmp/etc.tar.gz
  [root@servera backuptest]# 
  ```

  5.5.	Отобразите содержимое каталога **/backuptest**. Убедитесь, что каталог содержит файлы из каталога **/etc**.

  ```
  [root@servera backuptest]# ls -l
  total 12
  drwxr-xr-x. 95 root root 8192 Feb  8 10:16 etc
  [root@servera backuptest]# cd etc
  [root@servera etc]# ls -l
  total 1204
  -rw-r--r--.  1 root root       16 Jan 16 23:41 adjtime
  -rw-r--r--.  1 root root     1518 Sep 10 17:21 aliases
  drwxr-xr-x.  2 root root      169 Feb  4 21:58 alternatives
  -rw-r--r--.  1 root root      541 Oct  2 21:01 anacrontab
  ...output omitted...
  ```
  </details>

6.	Выйдите с **servera**.

  ```
  [root@servera backuptest]# exit
  logout
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab archive-manage finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab archive-manage finish
```

Упражнение завершено.

