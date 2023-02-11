## Цели

После завершения этого раздела вы сможете включить и отключить использование сервером репозиториев Yum от Red Hat или сторонних поставщиков.

## Включение репозиториев программного обеспечения Red Hat

Регистрация системы в службе управления подписками автоматически настраивает доступ к репозиториям программного обеспечения на основе подключенных подписок. Для просмотра всех доступных репозиториев выполните следующую команду.

```bash
[user@host ~]$ yum repolist all
...output omitted...
rhel-8-for-x86_64-appstream-debug-rpms   ... AppStream (Debug RPMs)  disabled
rhel-8-for-x86_64-appstream-rpms         ... AppStream (RPMs)        enabled: 5,045
rhel-8-for-x86_64-appstream-source-rpms  ... AppStream (Source RPMs) disabled
rhel-8-for-x86_64-baseos-debug-rpms      ... BaseOS (Debug RPMs)     enabled: 2,270
rhel-8-for-x86_64-baseos-rpms            ... BaseOS (RPMs)           enabled: 1,963
...output omitted...
```

Команду `yum config-manager` можно использовать для включения и отключения репозиториев. Чтобы включить репозиторий, команда устанавливает для параметра **enabled** значение **1**. Например, следующая команда включает репозиторий **rhel-8-server-debug-rpms**:

```bash
[user@host ~]$ yum config-manager --enable rhel-8-server-debug-rpms
Updating Subscription Management repositories.
============= repo: rhel-8-for-x86_64-baseos-debug-rpms ============
[rhel-8-for-x86_64-baseos-debug-rpms]
bandwidth = 0
baseurl = [https://cdn.redhat.com/content/dist/rhel8/8/x86_64/baseos/debug]
cachedir = /var/cache/dnf
cost = 1000
deltarpm = 1
deltarpm_percentage = 75
enabled = 1
...output omitted...
```

Сторонние поставщики (не Red Hat) предоставляют программное обеспечение через сторонние репозитории. Доступ к ним возможен с помощью команды `yum` с веб-сайта, FTP-сервера или из локальной файловой системы. Например, Adobe предоставляет некоторое программное обеспечение для Linux через репозиторий Yum.

Чтобы включить поддержку нового стороннего репозитория, создайте файл в каталоге **/etc/yum.repos.d/**. У файлов конфигурации репозитория должно быть расширение **.repo**. В определения репозитория входят: URL-адрес репозитория, имя, информация о том, следует ли использовать GPG для проверки подписи пакетов, а также (если да) URL-адрес, указывающий на доверенный ключ GPG.

### Создание репозиториев Yum

Создавать репозитории Yum можно с помощью команды `yum config-manager`. Следующая команда создает файл **/etc/yum.repos.d/dl.fedoraproject.org_pub_epel_8_Everything_x86_64_.repo** с показанным ниже выводом.

```bash
[user@host ~]$ yum config-manager \
--add-repo="https://dl.fedoraproject.org/pub/epel/8/Everything/x86_64/"
Adding repo from: https://dl.fedoraproject.org/pub/epel/8/Everything/x86_64/

[dl.fedoraproject.org_pub_epel_8_Everything_x86_64_]
name=created by yum config-manager from https://dl.fedoraproject.org/pub/epel/8/Everything/x86_64/
baseurl=https://dl.fedoraproject.org/pub/epel/8/Everything/x86_64/
enabled=1
```

Измените этот файл, чтобы указать нестандартные значения и расположение ключа GPG. Ключи хранятся в разных местах на сайте удаленного репозитория, например в **http://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-8**. Администраторам следует загрузить ключ в локальный файл, а не разрешать yum получать ключ из внешнего источника. Пример:

```
[EPEL]
name=EPEL 8
baseurl=https://dl.fedoraproject.org/pub/epel/8/Everything/x86_64/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-8
```


### Пакеты конфигурации RPM для локальных репозиториев

Некоторые репозитории предоставляют файл конфигурации и открытый ключ GPG как часть пакета RPM, который можно загрузить и установить с помощью команды `yum localinstall`. Например, реализуемый на добровольной основе проект **EPEL** (Extra Packages for Enterprise Linux) предоставляет программное обеспечение, не поддерживаемое Red Hat, но совместимое с Red Hat Enterprise Linux.

Следующая команда устанавливает пакет репозитория EPEL для Red Hat Enterprise Linux 8:

```bash
[user@host ~]$ rpm --import \
http://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-8
[user@host ~]$ yum install \
https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

Файлы конфигурации часто содержат несколько ссылок на репозитории в одном файле. Каждая ссылка на репозиторий начинается с имени из одного слова в квадратных скобках.

```bash
[user@host ~]$ cat /etc/yum.repos.d/epel.repo
[epel]
name=Extra Packages for Enterprise Linux $releasever - $basearch
#baseurl=https://download.fedoraproject.org/pub/epel/$releasever/Everything/$basearch
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-8

[epel-debuginfo]
name=Extra Packages for Enterprise Linux $releasever - $basearch - Debug
#baseurl=https://download.fedoraproject.org/pub/epel/$releasever/Everything/$basearch/debug
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-8
gpgcheck=1

[epel-source]
name=Extra Packages for Enterprise Linux $releasever - $basearch - Source
#baseurl=https://download.fedoraproject.org/pub/epel/$releasever/Everything/SRPMS
metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-source-$releasever&arch=$basearch&infra=$infra&content=$contentdir
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-8
gpgcheck=1
```

Чтобы определить репозиторий, а не искать его по умолчанию, вставьте параметр **enabled=0**. Репозитории можно включить и отключить на постоянной основе с помощью команды `yum config-manager` или временно с помощью опций `--enablerepo=PATTERN` и `--disablerepo=PATTERN` команды `yum`.

<details>
<summary>Предупреждение</summary>

Установите ключ RPM GPG перед установкой подписанных пакетов. Это позволяет проверить, что пакеты принадлежат импортированному ключу. В случае отсутствия ключа команда `yum` не будет выполнена. Можно использовать опцию `--nogpgcheck`, чтобы игнорировать отсутствующие ключи GPG, но это может привести к тому, что в системе будут установлены поддельные или небезопасные пакеты, представляющие угрозу безопасности.
</details>