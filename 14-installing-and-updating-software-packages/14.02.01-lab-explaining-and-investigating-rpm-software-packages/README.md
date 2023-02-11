## Цели

Получение базовых навыков управления пакетами с помощью rpm

## Задачи

В этом упражнении вы соберете информацию о пакете стороннего поставщика, извлечете из него файлы для проверки, а затем установите их на сервере.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab software-rpm start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети. Сценарий также загружает пакет **rhcsa-script-1.0.0-1.noarch.rpm** в каталог **/home/student** на хосте **servera**.

```
[student@workstation ~]$ lab software-rpm start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	Посмотрите информацию о пакете и отобразите список файлов в пакете **rhcsa-script-1.0.0-1.noarch.rpm**. Кроме того, просмотрите сценарий, который запускается при установке или удалении пакета.

  <details>
  <summary>Показать решение</summary>

  2.1.	Посмотрите информацию о пакете **rhcsa-script-1.0.0-1.noarch.rpm**.

  ```
  [student@servera ~]$ rpm -q -p rhcsa-script-1.0.0-1.noarch.rpm -i
  Name        : rhcsa-script
  Version     : 1.0.0
  Release     : 1
  Architecture: noarch
  Install Date: (not installed)
  Group       : System
  Size        : 1056
  License     : GPL
  Signature   : (none)
  Source RPM  : rhcsa-script-1.0.0-1.src.rpm
  Build Date  : Wed 06 Mar 2019 03:59:46 PM IST
  Build Host  : foundation0.ilt.example.com
  Relocations : (not relocatable)
  Packager    : Snehangshu Karmakar
  URL         : http://example.com
  Summary     : RHCSA Practice Script
  Description :
  A RHCSA practice script.
  The package changes the motd.
  ```

  2.2.	Отобразите список файлов в пакете **rhcsa-script-1.0.0-1.noarch.rpm**.

  ```
  [student@servera ~]$ rpm -q -p rhcsa-script-1.0.0-1.noarch.rpm -l
  /opt/rhcsa-script/mymotd
  ```

  2.3.	Просмотрите сценарий, который запускается при установке или удалении пакета **rhcsa-script-1.0.0-1.noarch.rpm**.

  ```
  [student@servera ~]$ rpm -q -p rhcsa-script-1.0.0-1.noarch.rpm --scripts
  preinstall scriptlet (using /bin/sh):
  if [ "$1" == "2" ]; then
    if [ -e /etc/motd.orig ]; then
      mv -f /etc/motd.orig /etc/motd
    fi
  fi
  postinstall scriptlet (using /bin/sh):
  ...output omitted...
  ```
  </details>

3.	Извлеките содержимое пакета **rhcsa-script-1.0.0-1.noarch.rpm** в каталог **/home/student**.

  3.1.	С помощью команд `rpm2cpio` и `cpio -tv` отобразите список файлов в пакете **rhcsa-script-1.0.0-1.noarch.rpm**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ rpm2cpio rhcsa-script-1.0.0-1.noarch.rpm | cpio -tv
  -rw-r--r--   1 root     root    1056 Mar  6 15:59 ./opt/rhcsa-script/mymotd
  3 blocks
  ```
  </details>

  3.2.	Извлеките все файлы из пакета **rhcsa-script-1.0.0-1.noarch.rpm** в каталоге **/home/student**. Используйте команды `rpm2cpio` и `cpio -idv` для извлечения файлов и создания ведущих каталогов, где это необходимо, в режиме детализации.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ rpm2cpio rhcsa-script-1.0.0-1.noarch.rpm | cpio -idv
  ./opt/rhcsa-script/mymotd
  3 blocks
  ```
  </details>

  3.3.	Отобразите список для проверки извлеченных файлов в каталоге **/home/student/opt**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ ls -lR opt
  opt:
  total 0
  drwxrwxr-x. 2 student student 20 Mar  7 14:44 rhcsa-script

  opt/rhcsa-script:
  total 4
  -rw-r--r--. 1 student student 1056 Mar  7 14:44 mymotd
  ```
  </details>

4.	Установите пакет **rhcsa-script-1.0.0-1.noarch.rpm**. Используйте команду `sudo`, чтобы получить права привилегированного пользователя для установки пакета.

  <details>
  <summary>Показать решение</summary>

  4.1.	С помощью команды `sudo rpm -ivh` установите RPM-пакет **rhcsa-script-1.0.0-1.noarch.rpm**.

  ```
  [student@servera ~]$ sudo rpm -ivh rhcsa-script-1.0.0-1.noarch.rpm
  [sudo] password for student: student
  Verifying...                          ################################# [100%]
  Preparing...                          ################################# [100%]
  Updating / installing...
    1:rhcsa-script-1.0.0-1             ################################# [100%]
  [student@servera ~]$ 
  ```

  4.2.	Выполните команду `rpm`, чтобы убедиться, что пакет установлен.

  ```
  [student@servera ~]$ rpm -q rhcsa-script
  rhcsa-script-1.0.0-1.noarch
  ```
  </details>

5.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab software-rpm finish`, чтобы закончить упражнение. Этот сценарий удаляет все пакеты, установленные на servera во время упражнения.

```
[student@workstation ~]$ lab software-rpm finish
```

Упражнение завершено.