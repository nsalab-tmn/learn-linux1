## Цели

После завершения этого раздела вы сможете изменить параметры сети, отредактировав файлы конфигурации.

## Описание файлов конфигурации подключения

По умолчанию изменения, сделанные с помощью команды `nmcli con mod name`, автоматически сохраняются в файле **/etc/sysconfig/network-scripts/ifcfg-name**. Этот файл также можно отредактировать вручную в текстовом редакторе. После внесения изменений выполните команду `nmcli con reload`, чтобы демон **NetworkManager** считал изменения конфигурации.

Для обеспечения обратной совместимости директивы, сохраненные в этом файле, имеют имена и синтаксис, отличные от имен `nm-settings(5)`. В следующей таблице сопоставлены имена некоторых ключевых параметров с директивами ifcfg-*.

**Таблица 12.4.1. Сопоставление параметров nm-settings и директив ifcfg-\***

| nmcli con mod | ifcfg-* file | Результат |
| --- | --- | --- |
| ipv4.method manual | BOOTPROTO=none | Статическая настройка IPv4-адресов. |
| ipv4.method auto | BOOTPROTO=dhcp | Поиск параметров конфигурации на сервере DHCPv4. Если также заданы статические адреса, они не будут задействованы, пока не будут получены сведения от DHCPv4. |
| ipv4.addresses 192.0.2.1/24 | IPADDR=192.0.2.1  <br> PREFIX=24 | Указание статического IPv4-адреса и сетевого префикса. Если для подключения задано несколько адресов, то второй из них определяется директивами IPADDR1 и PREFIX1, третий ― директивами IPADDR2 и PREFIX2 и т. д. |
| ipv4.gateway 192.0.2.254 | GATEWAY=192.0.2.254 | Указание шлюза по умолчанию. |
| ipv4.dns 8.8.8.8 | DNS1=8.8.8.8 | Изменяет файл /etc/resolv.conf, чтобы использовать этот сервер имен. |
| ipv4.dns-search example.com | DOMAIN=example.com | Изменяет файл /etc/resolv.conf, чтобы использовать этот домен в директиве search. |
| ipv4.ignore-auto-dns true | PEERDNS=no | Игнорирование сведений о DNS-сервере от DHCP-сервера. |
| ipv6.method manual | IPV6_AUTOCONF=no | Статическая настройка IPv6-адресов. |
| ipv6.method auto | IPV6_AUTOCONF=yes | Настройка сетевых параметров с помощью SLAAC из объявлений маршрутизатора. |
| ipv6.method dhcp | IPV6_AUTOCONF=no  <br> DHCPV6C=yes | Настройка сетевых параметров с помощью DHCPv6, но не SLAAC. |
| ipv6.addresses 2001:db8::a/64 | IPV6ADDR=2001:db8::a/64 | Указание статического IPv6-адреса и сетевого префикса. Если для подключения задано несколько адресов, переменная IPV6ADDR_SECONDARIES принимает заключенный в двойные кавычки список определений «адрес/префикс», разделенных пробелами. |
| ipv6.gateway 2001:db8::1 | IPV6_DEFAULTGW=2001:... | Указание шлюза по умолчанию. |
| ipv6.dns fde2:6494:1e09:2::d | DNS1=fde2:6494:... | Изменяет файл /etc/resolv.conf, чтобы использовать этот сервер имен. Аналогично IPv4. |
| ipv6.dns-search example.com | IPV6_DOMAIN=example.com | Изменяет файл /etc/resolv.conf, чтобы использовать этот домен в директиве search. |
| ipv6.ignore-auto-dns true | IPV6_PEERDNS=no | Игнорирование сведений о DNS-сервере от DHCP-сервера. |
| connection.autoconnect yes | ONBOOT=yes | Автоматическая активация этого подключения при начальной загрузке системы. |
| connection.id ens3 | NAME=ens3 | Имя этого подключения. |
| connection.interface-name ens3 | DEVICE=ens3 | Подключение привязано к сетевому интерфейсу с этим именем. |
| 802-3-ethernet.mac-address ... | HWADDR=... | Подключение привязано к сетевому интерфейсу с этим MAC-адресом. |

## Изменение конфигурации сети

Для настройки сети можно отредактировать файлы конфигурации интерфейса. Файлы конфигурации подключения управляют программными интерфейсами для отдельных сетевых устройств. Эти файлы обычно называются **/etc/sysconfig/network-scripts/ifcfg-name**, где `name` ― это имя устройства или подключения, которым управляет файл конфигурации. Ниже приведены стандартные переменные из файла, который используется для статической или динамической настройки IPv4.

**Таблица 12.4.2. Параметры конфигурации IPv4 для файла ifcfg**

| Статические | Динамические | Любой вариант |
| --- | --- | --- |
| BOOTPROTO=none<br>IPADDR0=172.25.250.10<br>PREFIX0=24<br>GATEWAY0=172.25.250.254<br>DEFROUTE=yes<br>DNS1=172.25.254.254 | BOOTPROTO=dhcp | DEVICE=ens3<br>NAME="static-ens3"<br>ONBOOT=yes<br>UUID=f3e8(...)ad3e<br>USERCTL=yes |

В статических параметрах переменные для IP-адресов, префикса и шлюза имеют число в конце. Это позволяет присваивать интерфейсу несколько наборов значений. Переменная DNS также заканчивается числом, которое используется для указания порядка поиска, если задано несколько серверов.

После изменения файлов конфигурации выполните команду `nmcli con reload`, чтобы демон **NetworkManager** считал изменения в конфигурации. Чтобы изменения вступили в силу, перезапустите интерфейс.

```bash
[root@host ~]# nmcli con reload
[root@host ~]# nmcli con down "static-ens3"
[root@host ~]# nmcli con up "static-ens3"
```

## Редактирование файлов конфигурации в Debian-based дистрибутивах

В дистрибутивах на основе Debian используются файлы конфигурации, отличные от описанных выше (в основном используются в RedHat-based дистрибутивах).

В Debian сеть настраивается автоматически во время первоначальной установки. Если установлен Network Manager (что обычно имеет место при полной установке рабочего стола), то вероятнее всего, не требуется никакой настройки (например, если вы полагаетесь на DHCP в проводном соединении и не предъявляете особых требований). Если требуется конфигурация (например, для интерфейса WiFi), то будет создан соответствующий файл в формате /etc/NetworkManager/system-connections/

В случае установленного NetworkManager (в последних версиях - он установлен по умолчанию) настройка сети также может осуществляться средствами, описанными в предыдущем разделе.

Если NetworkManager не установлен, программа установки настроит **ifupdown** , создав файл **/etc/network/interfaces**. При наличии большого количества интерфейсов рекомендуется хранить конфигурацию в разных файлах внутри каталога **/etc/network/interfaces.d/**

### Примеры конфигурации сети

**Настройка получения конфигурации по DHCP**

* Определите имена нужных интерфейсов, например с помощью `ip link show`
* От имени *root* откройте для редактирования файл **/etc/network/interfaces**
* Добавьте или отредактируйте конфигурацию до следующего вида
  
  ```bash
  auto <if_name>
  iface <if_name> inet dhcp
  ```

  * `auto` - устанавливает режим автоматической активации подключения при загрузке
  * `if_name` - имя интерфейса, параметры которого необходимо установить
  * `inet` - указывает на конфигурацию IPv4 (в совокупности со следующим параметром `dhcp` говорит о том, что нужно получить конфигурацию от DHCPv4 сервера). Если необходимо настроить IPv6 по DHCP, используйте комбинацию `inet6 auto`
* От имени *root* переподключите интерфейс командой `ifdown <if_name> && ifup <ifname>` или перезапустите сервис **networking** `systemctl restart networking`

**Настройка статической конфигурации**

* Определите имена нужных интерфейсов, например с помощью `ip link show`
* От имени *root* откройте для редактирования файл **/etc/network/interfaces**
* Добавьте или отредактируйте конфигурацию до следующего вида
  
  ```bash
  auto <if_name>
  iface <if_name> inet static
    address <ipv4addr/prefixlen>
    gateway <gw_addr>
    dns-nameservers <dns1> <dns2>
  ```

  * `auto` - устанавливает режим автоматической активации подключения при загрузке
  * `if_name` - имя интерфейса, параметры которого необходимо установить
  * `inet` - указывает на конфигурацию IPv4 (в совокупности со следующим параметром `static` говорит о том, что будет использоваться статическая настройка). Если необходимо настроить статический IPv6, используйте комбинацию `inet6 static`
  * `address` - параметр, которому нужно передать необходимый сетевой адрес. Это можно сделать двумя способами:
   
    1. Указание адреса в формате `ipv4addr/prefixlen`, например, строка может выглядеть так: `address 192.168.10.12/24`
    
    2. Указание адреса и маски в отдельных строках

      ```
      address 192.168.10.12
      netmask 255.255.255.0
      ```
  * `gateway` - адрес шлюза по умолчанию (если необходимо для данного интерфейса)
  * `dns-nameservers` - список dns-серверов через пробел (если необходимо для данного интерфейса)

* От имени *root* переподключите интерфейс командой `ifdown <if_name> && ifup <ifname>` или перезапустите сервис **networking** `systemctl restart networking`
