## Цели

После завершения этого раздела вы сможете настроить статическое имя хоста сервера и разрешение имен, а также протестировать результаты.

## Изменение имени хоста системы

Команда `hostname` отображает или временно изменяет полностью определенное имя хоста системы.

```bash
[root@host ~]# hostname
host.example.com
```

Статическое имя хоста можно указать в файле **/etc/hostname**. Команда `hostnamectl` используется для изменения этого файла, а также для просмотра состояния полностью определенного имени хоста. Если файл не существует, имя хоста задается обратным DNS-запросом после назначения интерфейсу IP-адреса.

```bash
[root@host ~]# hostnamectl set-hostname host.example.com
[root@host ~]# hostnamectl status
   Static hostname: host.example.com
         Icon name: computer-vm
           Chassis: vm
        Machine ID: f874df04639f474cb0a9881041f4f7d4
           Boot ID: 6a0abe03ef0b4a97bcb8afb7b281e4d3
    Virtualization: kvm
  Operating System: Red Hat Enterprise Linux 8.2 (Ootpa)
       CPE OS Name: cpe:/o:redhat:enterprise_linux:8.2:GA
            Kernel: Linux 4.18.0-193.el8.x86_64
      Architecture: x86-64
[root@host ~]# cat /etc/hostname
host.example.com
```

<details>
<summary>Важно</summary>

В современных дистрибутивах на основе Debian и RedHat статическое имя хоста хранится в файле **/etc/hostname**. В более ранних версиях, например в Red Hat Enterprise Linux 6, имя хоста сохраняется как переменная в файле **/etc/sysconfig/network**.
</details>

## Настройка разрешения имен

Простой преобразователь (**stub resolver**) используется для преобразования имен хостов в IP-адреса и наоборот. Он определяет место поиска на основе конфигурации в файле /**etc/nsswitch.conf**. По умолчанию содержимое файла **/etc/hosts** проверяется в первую очередь.

```bash
[root@host ~]# cat /etc/hosts
127.0.0.1       localhost localhost.localdomain localhost4 localhost4.localdomain4
::1             localhost localhost.localdomain localhost6 localhost6.localdomain6
172.25.254.254 classroom.example.com
172.25.254.254 content.example.com
```

Команду `getent hosts hostname` можно использовать для проверки разрешения имен хостов с помощью файла **/etc/hosts**.

Если запись в файле **/etc/hosts** отсутствует, то по умолчанию преобразователь ищет имя хоста с помощью сервера имен DNS. Файл **/etc/resolv.conf** определяет, как выполняется запрос:

* **search**: список доменных имен с коротким именем хоста. Этот параметр и domain не следует задавать в одном файле, в противном случае будет использоваться последний экземпляр. Подробнее см. на man-странице `resolv.conf(5)`.
* **nameserver**: IP-адрес запрашиваемого сервера имен. Можно указать до трех директив сервера имен, чтобы использовать резервные, если один из них станет недоступен.

```bash
[root@host ~]# cat /etc/resolv.conf
# Generated by NetworkManager
domain example.com
search example.com
nameserver 172.25.254.254
```

Демон **NetworkManager** обновляет файл **/etc/resolv.conf**, используя параметры DNS в файлах конфигурации подключения. Используйте команду `nmcli` для изменения подключений.

```bash
[root@host ~]# nmcli con mod ID ipv4.dns IP
[root@host ~]# nmcli con down ID
[root@host ~]# nmcli con up ID
[root@host ~]# cat /etc/sysconfig/network-scripts/ifcfg-ID
...output omitted...
DNS1=8.8.8.8
...output omitted...
```

По умолчанию команда `nmcli con mod ID ipv4.dns IP` заменяет предыдущие параметры DNS на новый список IP-адресов. Символ `+` или `-` перед аргументом **ipv4.dns** добавляет или удаляет отдельную запись.

```bash
[root@host ~]# nmcli con mod ID +ipv4.dns IP
```

Чтобы добавить DNS-сервер с IP-адресом IPv6 `2001:4860:4860::8888` в список серверов имен для использования с подключением **static-ens3**, выполните следующую команду:

```bash
[root@host ~]# nmcli con mod static-ens3 +ipv6.dns 2001:4860:4860::8888
```

<details>
<summary>Примечание</summary>

Статические параметры DNS IPv4 и IPv6 попадают в директивы nameserver файла **/etc/resolv.conf**. Необходимо убедиться, что в списке как минимум указан доступный по IPv4 сервер имен (для системы с двумя стеками). Лучше, чтобы был хотя бы один сервер имен, использующий IPv4, и еще один, использующий IPv6, в случае, если возникнут проблемы с сетью IPv4 или IPv6.
</details>


### Проверка разрешения имен DNS

Команда `host HOSTNAME` позволяет проверить подключение к DNS-серверу.

```bash
[root@host ~]# host classroom.example.com
classroom.example.com has address 172.25.254.254
[root@host ~]# host 172.25.254.254
254.254.25.172.in-addr.arpa domain name pointer classroom.example.com.
```

<details>
<summary>Важно</summary>

DHCP автоматически перезаписывает файл **/etc/resolv.conf** при запуске интерфейсов, если не указать **PEERDNS=no** в соответствующих файлах конфигурации интерфейсов. Задайте этот параметр при помощи команды `nmcli`.

```
[root@host ~]# nmcli con mod "static-ens3" ipv4.ignore-auto-dns yes
```
</details>