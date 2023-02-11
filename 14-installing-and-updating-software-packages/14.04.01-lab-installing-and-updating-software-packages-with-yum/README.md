## Цели

Получение базовых навыков управления пакетами с помощью yum

## Задачи

В этом упражнении вы установите и удалите пакеты и группы пакетов.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab software-yum start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети.

```
[student@workstation ~]$ lab software-yum start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*. Системы настроены на использование ключей SSH для аутентификации, поэтому пароль для входа на **servera** не требуется.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	С помощью команды `sudo -i` переключитесь на пользователя *root* в командной строке.

  ```
  [student@servera ~]$ sudo -i
  Password: student
  [root@servera ~]#  
  ```

3.	Найдите определенный пакет.

  3.1.	Попробуйте выполнить команду `guile`. Вы обнаружите, что она не установлена.

  ```
  [root@servera ~]# guile
  -bash: guile: command not found 
  ```

  3.2.	С помощью команды `yum search` найдите пакеты, в имени или сводке которых есть **guile**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum search guile
  ========================= Name Exactly Matched: guile ===============
  guile.i686 : A GNU implementation of Scheme for application extensibility
  guile.x86_64 : A GNU implementation of Scheme for application extensibility

  3.3.	С помощью команды yum info получите дополнительную информацию о пакете guile.
  [root@servera ~]# yum info guile
  Available Packages
  Name         : guile
  Epoch        : 5
  Version      : 2.0.14
  Release      : 7.el8
  ...output omitted...
  ```
  </details>

4.	Выполните команду `yum install`, чтобы установить пакет **guile**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum install guile
  ...output omitted...
  Dependencies resolved.
  ===============================================================================
  Package       Arch   Version          Repository                         Size
  ===============================================================================
  Installing:
  guile         x86_64 5:2.0.14-7.el8   rhel-8.2-for-x86_64-appstream-rpms 3.5 M
  Installing dependencies:
  gc            x86_64 7.6.4-3.el8      rhel-8.2-for-x86_64-appstream-rpms 109 k
  libatomic_ops x86_64 7.6.2-3.el8      rhel-8.2-for-x86_64-appstream-rpms  38 k
  libtool-ltdl  x86_64 2.4.6-25.el8     rhel-8.2-for-x86_64-baseos-rpms     58 k

  Transaction Summary
  ===============================================================================
  Install  4 Packages

  Total download size: 3.7 M
  Installed size: 12 M
  Is this ok [y/N]: y
  ...output omitted...
  Complete!
  ```
  </details>


5.	Удалите пакеты.

  5.1.	Выполните команду `yum remove`, чтобы удалить пакет **guile**, но ответьте **no**, когда появится соответствующий запрос. Сколько пакетов было бы удалено?

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum remove guile
  ...output omitted...
  Dependencies resolved.
  ===============================================================================
  Package       Arch   Version         Repository                          Size
  ===============================================================================
  Removing:
  guile         x86_64 5:2.0.14-7.el8  @rhel-8.2-for-x86_64-appstream-rpms  12 M
  Removing unused dependencies:
  gc            x86_64 7.6.4-3.el8     @rhel-8.2-for-x86_64-appstream-rpms 221 k
  libatomic_ops x86_64 7.6.2-3.el8     @rhel-8.2-for-x86_64-appstream-rpms  75 k
  libtool-ltdl  x86_64 2.4.6-25.el8    @rhel-8.2-for-x86_64-baseos-rpms     69 k

  Transaction Summary
  ===============================================================================
  Remove 4 Packages

  Freed space: 12 M
  Is this ok [y/N]: n
  Operation aborted.
  ```
  </details>

  5.2.	Выполните команду `yum remove`, чтобы удалить пакет **gc**, но ответьте **no**, когда появится соответствующий запрос. Сколько пакетов было бы удалено?

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum remove gc
  ...output omitted...
  Dependencies resolved.
  ===============================================================================
  Package       Arch   Version         Repository                          Size
  ===============================================================================
  Removing:
  gc            x86_64 7.6.4-3.el8     @rhel-8.2-for-x86_64-appstream-rpms 221 k
  Removing dependent packages:
  guile         x86_64 5:2.0.14-7.el8  @rhel-8.2-for-x86_64-appstream-rpms  12 M
  Removing unused dependencies:
  libatomic_ops x86_64 7.6.2-3.el8     @rhel-8.2-for-x86_64-appstream-rpms  75 k
  libtool-ltdl  x86_64 2.4.6-25.el8    @rhel-8.2-for-x86_64-baseos-rpms     69 k

  Transaction Summary
  ===============================================================================
  Remove  4 Packages 

  Freed space: 12 M
  Is this ok [y/N]: n
  Operation aborted.
  ```
  </details>

6.	Получите информацию о группе компонентов «Security Tools» и установите ее на **servera**.

  6.1.	Выполните команду `yum group list`, чтобы отобразить список всех доступных групп компонентов.

  ```
  [root@servera ~]# yum group list
  ```

  6.2.	С помощью команды `yum group info` получите дополнительную информацию о группе компонентов **Security Tools**, в том числе список включенных пакетов.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum group info "Security Tools"
  Group: Security Tools
  Description: Security tools for integrity and trust verification.
  Default Packages:
    scap-security-guide
  Optional Packages:
    aide
    hmaccalc
    openscap
    openscap-engine-sce
    openscap-utils
    scap-security-guide-doc
    scap-workbench
    tpm-quote-tools
    tpm-tools
    tpm2-tools
    trousers
    udica
  ```
  </details>

  6.3.	С помощью команды `yum group install` установите группу компонентов **Security Tools**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum group install "Security Tools"
  Dependencies resolved.
  ===============================================================================
  Package             Arch   Version      Repository                        Size
  ===============================================================================
  Installing group/module packages:
  scap-security-guide noarch 0.1.48-7.el8 rhel-8-for-x86_64-appstream-rpms 6.9 M
  Installing dependencies:
  GConf2              x86_64 3.2.6-22.el8 rhel-8-for-x86_64-appstream-rpms 1.0 M
  ...output omitted...

  Transaction Summary
  ===============================================================================
  Install  6 Packages

  Total download size: 12 M
  Installed size: 247 M
  Is this ok [y/N]: y
  ...output omitted...
  Installed:
    GConf2-3.2.6-22.el8.x86_64               libxslt-1.1.32-4.el8.x86_64
    openscap-1.3.2-6.el8.x86_64              openscap-scanner-1.3.2-6.el8.x86_64
    scap-security-guide-0.1.48-7.el8.noarch  xml-common-0.6.3-50.el8.noarch

  Complete!
  ```
  </details>

7.	Изучите опции истории (history) и отмены действий (undo) в команде `yum`.

  7.1.	Выполните команду `yum history`, чтобы отобразить историю последних операций yum.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum history
  ID     | Command line             | Date and time    | Action(s)      | Altered
  -------------------------------------------------------------------------------
      6 | group install Security T | 2019-02-26 17:11 | Install        |    7
      5 | install guile            | 2019-05-26 17:05 | Install        |    4
      4 | -y install @base firewal | 2019-02-04 11:27 | Install        |  127 EE
      3 | -C -y remove firewalld - | 2019-01-16 13:12 | Removed        |   11 EE
      2 | -C -y remove linux-firmw | 2019-01-16 13:12 | Removed        |    1
      1 |                          | 2019-01-16 13:05 | Install        |  447 EE
  ```
  </details>

  В вашей системе история, скорее всего, будет отличаться.

  7.2.	Выполните команду `yum history info`, чтобы убедиться, что последней операцией является установка группы. В следующей команде замените идентификатор операции идентификатором с предыдущего шага.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum history info 6
  Transaction ID : 6
  Begin time     : Tue 26 Feb 2019 05:11:25 PM EST
  Begin rpmdb    : 563:bf48c46156982a78e290795400482694072f5ebb
  End time       : Tue 26 Feb 2019 05:11:33 PM EST (8 seconds)
  End rpmdb      : 623:bf25b424ccf451dd0a6e674fb48e497e66636203
  User           : Student User <student>
  Return-Code    : Success
  Releasever     : 8
  Command Line   : group install Security Tools
  Packages Altered:
      Install libxslt-1.1.32-4.el8.x86_64       @rhel-8.2-for-x86_64-baseos-rpms
      Install xml-common-0.6.3-50.el8.noarch    @rhel-8.2-for-x86_64-baseos-rpms
  ...output omitted... 
  ```
  </details>

  7.3.	Выполните команду `yum history undo`, чтобы удалить набор пакетов, которые были установлены вместе с пакетом **guile**. В своей системе найдите правильный идентификатор операции в выводе команды `yum history` и используйте его в следующей команде.

  <details>
  <summary>Показать решение</summary>
  
  ```
  [root@servera ~]# yum history undo 5
  ```
  </details>

8.	Выйдите из системы **servera**.

  ```
  [root@servera ~]# exit
  logout
  [student@servera ~]$ exit 
  Connection to servera closed.
  [student@workstation ~]$
  ```

## Конец

На **workstation** запустите сценарий `lab software-yum finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab software-yum finish
```

Упражнение завершено.