## Цели

После завершения этого раздела вы сможете найти, установить и обновить программные пакеты с помощью команды `yum`.

## Управление программными пакетами с помощью Yum

Устанавливать пакеты можно с помощью низкоуровневой команды `rpm`, но она не предназначена для работы с репозиториями пакетов и автоматического разрешения зависимостей из нескольких источников.

Yum ― это более удобная система для управления установкой и обновлениями программного обеспечения на основе пакетов RPM. С помощью команды yum можно устанавливать, обновлять, удалять программные пакеты и их зависимости, а также получать сведения о них. Вы можете получить историю выполненных транзакций и работать с несколькими репозиториями программного обеспечения компании Red Hat и сторонних поставщиков.

Поиск программного обеспечения с помощью Yum:

* Команда `yum help` отображает сведения об использовании утилиты.
* Команда `yum list` отображает список установленных и доступных пакетов.

    ```bash
    [user@host ~]$ yum list 'http*'
    Available Packages
    http-parser.i686              2.8.0-2.el8                        rhel8-appstream
    http-parser.x86_64            2.8.0-2.el8                        rhel8-appstream
    httpcomponents-client.noarch  4.5.5-4.module+el8+2452+b359bfcd   rhel8-appstream
    httpcomponents-core.noarch    4.4.10-3.module+el8+2452+b359bfcd  rhel8-appstream
    httpd.x86_64                  2.4.37-7.module+el8+2443+605475b7  rhel8-appstream
    httpd-devel.x86_64            2.4.37-7.module+el8+2443+605475b7  rhel8-appstream
    httpd-filesystem.noarch       2.4.37-7.module+el8+2443+605475b7  rhel8-appstream
    httpd-manual.noarch           2.4.37-7.module+el8+2443+605475b7  rhel8-appstream
    httpd-tools.x86_64            2.4.37-7.module+el8+2443+605475b7  rhel8-appstream
    ```

* Команда `yum search KEYWORD` отображает список пакетов по ключевым словам, присутствующим только в полях имени и сводки.

    Для поиска пакетов, которые содержат слова «web server» в полях имени, сводки и описания, используйте команду `search all`.

    ```bash
    [user@host ~]$ yum search all 'web server'
    ================= Summary & Description Matched: web server ====================
    pcp-pmda-weblog.x86_64 : Performance Co-Pilot (PCP) metrics from web server logs
    nginx.x86_64 : A high performance web server and reverse proxy server
    ======================== Summary Matched: web server ===========================
    libcurl.x86_64 : A library for getting files from web servers
    libcurl.i686 : A library for getting files from web servers
    libcurl.x86_64 : A library for getting files from web servers
    ====================== Description Matched: web server =========================
    httpd.x86_64 : Apache HTTP Server
    git-instaweb.x86_64 : Repository browser in gitweb
    ...output omitted...
    ```

* Команда `yum info PACKAGENAME` отображает подробную информацию о пакете, включая объем дискового пространства, необходимый для установки.

    Для получения информации об HTTP-сервере Apache выполните следующую команду:

    ```bash
    [user@host ~]$ yum info httpd
    Available Packages
    Name         : httpd
    Version      : 2.4.37
    Release      : 7.module+el8+2443+605475b7
    Arch         : x86_64
    Size         : 1.4 M
    Source       : httpd-2.4.37-7.module+el8+2443+605475b7.src.rpm
    Repo         : rhel8-appstream
    Summary      : Apache HTTP Server
    URL          : https://httpd.apache.org/
    License      : ASL 2.0
    Description  : The Apache HTTP Server is a powerful, efficient, and extensible
                : web server.
    ```

* Команда `yum provides PATHNAME` отображает пакеты, соответствующие указанному пути (часто с метасимволами).

    Для поиска пакетов, которые предоставляют каталог /var/www/html, выполните следующую команду:

    ```bash
    [user@host ~]$ yum provides /var/www/html
    httpd-filesystem-2.4.37-7.module+el8+2443+605475b7.noarch : The basic directory layout for the Apache HTTP server
    Repo        : rhel8-appstream
    Matched from:
    Filename    : /var/www/html
    ```

### Установка и удаление программного обеспечения с помощью yum

* Команда `yum install PACKAGENAME` получает и устанавливает программный пакет, включая все зависимости.

    ```bash
    [user@host ~]$ yum install httpd
    Dependencies resolved.
    ================================================================================
    Package                  Arch       Version             Repository        Size
    ================================================================================
    Installing:
    httpd                    x86_64     2.4.37-7.module...  rhel8-appstream   1.4 M
    Installing dependencies:
    apr                      x86_64     1.6.3-8.el8         rhel8-appstream   125 k
    apr-util                 x86_64     1.6.1-6.el8         rhel8-appstream   105 k
    ...output omitted...
    Transaction Summary
    ================================================================================
    Install  9 Packages

    Total download size: 2.0 M
    Installed size: 5.4 M
    Is this ok [y/N]: y
    Downloading Packages:
    (1/9): apr-util-bdb-1.6.1-6.el8.x86_64.rpm           464 kB/s |  25 kB     00:00
    (2/9): apr-1.6.3-8.el8.x86_64.rpm                    1.9 MB/s | 125 kB     00:00
    (3/9): apr-util-1.6.1-6.el8.x86_64.rpm               1.3 MB/s | 105 kB     00:00
    ...output omitted...
    Total                                                8.6 MB/s | 2.0 MB     00:00
    Running transaction check
    Transaction check succeeded.
    Running transaction test
    Transaction test succeeded.
    Running transaction
    Preparing        :                                                         1/1
    Installing       : apr-1.6.3-8.el8.x86_64                                  1/9
    Running scriptlet: apr-1.6.3-8.el8.x86_64                                  1/9
    Installing       : apr-util-bdb-1.6.1-6.el8.x86_64                         2/9
    ...output omitted...
    Installed:
    httpd-2.4.37-7.module+el8+2443+605475b7.x86_64 apr-util-bdb-1.6.1-6.el8.x86_64
    apr-util-openssl-1.6.1-6.el8.x86_64            apr-1.6.3-8.el8.x86_64
    ...output omitted...
    Complete!
    ```

* Команда `yum update PACKAGENAME` получает и устанавливает новую версию указанного пакета, включая все зависимости. Как правило, в процессе обновления файлы конфигурации не заменяются, однако в некоторых случаях они могут быть переименованы, если сборщик пакета полагает, что старый файл не будет работать после обновления. Если имя пакета (PACKAGENAME) не задано, устанавливаются все соответствующие обновления.

    ```bash
    [user@host ~]$ sudo yum update
    ```

    Поскольку новое ядро может быть протестировано только после его загрузки, соответствующий пакет специально собирается таким образом, чтобы обеспечить возможность одновременной установки нескольких версий. Если новое ядро не загружается, старое ядро по-прежнему доступно. Использование команды `yum update kernel` фактически установит новое ядро. В файлах конфигурации хранятся списки пакетов, которые должны устанавливаться всегда, даже если администратор запрашивает только обновление.

    <details>
    <summary>Примечание</summary>

    Используйте команду `yum list kernel` для отображения всех установленных и доступных ядер. Для просмотра ядра, работающего в настоящий момент, используйте команду `uname`. Опция `-r` отображает только версию и релиз ядра, а опция `-a` — релиз ядра и дополнительную информацию.

    ```bash
    [user@host ~]$ yum list kernel
    Installed Packages
    kernel.x86_64         4.18.0-60.el8         @anaconda
    kernel.x86_64         4.18.0-67.el8         @rhel-8-for-x86_64-baseos-htb-rpms
    [user@host ~]$ uname -r
    4.18.0-60.el8.x86_64
    [user@host ~]$ uname -a
    Linux host.lab.example.com 4.18.0-60.el8.x86_64 #1 SMP Fri Jan 11 19:08:11 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
    ```
    </details>

* Команда `yum remove PACKAGENAME` удаляет установленный программный пакет, включая все вспомогательные пакеты.

    ```bash
    [user@host ~]$ sudo yum remove httpd
    ```

    <details>
    <summary>Предупреждение</summary>

    Команда `yum remove` удаляет указанные пакеты, а также все пакеты, зависящие от удаляемых пакетов (и пакеты, в свою очередь зависящие от этих пакетов, и т.д.). Это может привести к непредвиденному удалению пакетов, поэтому внимательно проверяйте список удаляемых пакетов.
    </details>

### Установка и удаление групп программного обеспечения с помощью yum

* В **yum** также используется концепция групп, которые представляют собой наборы связанного программного обеспечения, устанавливаемого совместно для определенной цели. В RedHat-based дистрибутивах есть два типа групп. Обычные группы ― это наборы пакетов. Группы окружения — это наборы обычных групп. Пакеты и группы, входящие в состав группы, могут быть обязательными (должны быть установлены при установке группы), стандартными (обычно устанавливаются при установке группы) и необязательными (без специального запроса не устанавливаются при установке группы).

    Как и `yum list`, команда `yum group list` отображает имена установленных и доступных групп.

    ```bash
    [user@host ~]$ yum group list
    Available Environment Groups:
    Server with GUI
    Minimal Install
    Server
    ...output omitted...
    Available Groups:
    Container Management
    .NET Core Development
    RPM Development Tools
    ...output omitted...
    ```

    Некоторые группы обычно устанавливаются посредством групп окружения и скрыты по умолчанию. Для отображения списка таких скрытых групп используйте команду `yum group list hidden`.

* Команда `yum group info` отображает сведения о группе, включая имена обязательных, стандартных и необязательных пакетов.

    ```bash
    [user@host ~]$ yum group info "RPM Development Tools"
    Group: RPM Development Tools
    Description: These tools include core development tools such rpmbuild.
    Mandatory Packages:
        redhat-rpm-config
        rpm-build
    Default Packages:
        rpmdevtools
    Optional Packages:
        rpmlint
    ```

* Команда `yum group install` устанавливает группу, которая устанавливает свои обязательные и стандартные пакеты, а также пакеты, от которых они зависят.

    ```bash
    [user@host ~]$ sudo yum group install "RPM Development Tools"
    ...output omitted...
    Installing Groups:
    RPM Development Tools

    Transaction Summary
    ===============================================================================
    Install  64 Packages

    Total download size: 21 M
    Installed size: 62 M
    Is this ok [y/N]: y
    ...output omitted...
    ```

<details>
<summary>Важно</summary>

Поведение групп Yum изменилось, начиная с Red Hat Enterprise Linux 7. В RHEL 7 и более поздних версиях группы считаются объектами и их отслеживание осуществляется на уровне системы. Если обновляется установленная группа и в группу через репозиторий Yum были добавлены обязательные или стандартные пакеты, эти новые пакеты устанавливаются при обновлении.

В RHEL 6 и более ранних версиях группа считалась установленной, если все ее обязательные пакеты были установлены, в группе нет обязательных пакетов либо были установлены любые стандартные или необязательные пакеты группы. Начиная с RHEL 7, группа считается установленной только в том случае, если для ее установки использовалась команда `yum group install`. С помощью команды `yum group mark install GROUPNAME` можно пометить группу как установленную, при этом все отсутствующие пакеты и их зависимости будут установлены при следующем обновлении.

И наконец, в RHEL 6 и более ранних версиях отсутствовали формы команд `yum group` из двух слов. Другими словами, в RHEL 6 команда `yum grouplist` присутствовала, а эквивалентная ей команда `yum group list`, доступная в RHEL 7 и RHEL 8, — нет.
</details>

### Просмотр истории операций

* Все операции по установке и удалению регистрируются в файле **/var/log/dnf.rpm.log**.

    ```bash
    [user@host ~]$ tail -5 /var/log/dnf.rpm.log
    2019-02-26T18:27:00Z SUBDEBUG Installed: rpm-build-4.14.2-9.el8.x86_64
    2019-02-26T18:27:01Z SUBDEBUG Installed: rpm-build-4.14.2-9.el8.x86_64
    2019-02-26T18:27:01Z SUBDEBUG Installed: rpmdevtools-8.10-7.el8.noarch
    2019-02-26T18:27:01Z SUBDEBUG Installed: rpmdevtools-8.10-7.el8.noarch
    2019-02-26T18:38:40Z INFO --- logging initialized ---
    ```

* Команда `yum history` отображает сводку по операциям установки и удаления.

    ```bash
    [user@host ~]$ sudo yum history
    ID     | Command line             | Date and time    | Action(s)      | Altered
    -------------------------------------------------------------------------------
        7 | group install RPM Develo | 2019-02-26 13:26 | Install        |   65
        6 | update kernel            | 2019-02-26 11:41 | Install        |    4
        5 | install httpd            | 2019-02-25 14:31 | Install        |    9
        4 | -y install @base firewal | 2019-02-04 11:27 | Install        |  127 EE
        3 | -C -y remove firewalld - | 2019-01-16 13:12 | Removed        |   11 EE
        2 | -C -y remove linux-firmw | 2019-01-16 13:12 | Removed        |    1
        1 |                          | 2019-01-16 13:05 | Install        |  447 EE
    ```

* Опция `history undo` отменяет операцию.

    ```bash
    [user@host ~]$ sudo yum history undo 5
    Undoing transaction 7, from Tue 26 Feb 2019 10:40:32 AM EST
        Install apr-1.6.3-8.el8.x86_64                              @rhel8-appstream
        Install apr-util-1.6.1-6.el8.x86_64                         @rhel8-appstream
        Install apr-util-bdb-1.6.1-6.el8.x86_64                     @rhel8-appstream
        Install apr-util-openssl-1.6.1-6.el8.x86_64                 @rhel8-appstream
        Install httpd-2.4.37-7.module+el8+2443+605475b7.x86_64      @rhel8-appstream
    ...output omitted...
    ```

## Сводка по командам Yum

Пакеты можно находить, устанавливать, обновлять и удалять по имени или группе пакетов.

| **Задача** | **Команда** |
| --- | --- |
| Отображение списка установленных и доступных пакетов по имени | `yum list [NAME-PATTERN]` |
| Отображение списка установленных и доступных групп | `yum group list` |
| Поиск пакета по ключевому слову | `yum search KEYWORD` |
| Отображение информации о пакете | `yum info PACKAGENAME` |
| Установка пакета | `yum install PACKAGENAME` |
| Установка группы пакетов | `yum group install GROUPNAME` |
| Обновление всех пакетов | `yum update` |
| Удаление пакета | `yum remove PACKAGENAME` |
| Отображение истории операций | `yum history` |