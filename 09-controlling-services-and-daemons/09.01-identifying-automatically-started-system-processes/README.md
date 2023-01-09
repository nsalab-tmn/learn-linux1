## Цели

После завершения данного раздела вы сможете отобразить список системных демонов и сетевых служб, запущенных службой systemd и юнитами сокетов.

## Знакомство с `systemd`

Демон _systemd_ управляет запуском системы Linux, включая запуск служб и управление службами в целом. Он активирует системные ресурсы, серверные демоны и другие процессы как во время начальной загрузки, так и в работающей системе.

Демоны ― это процессы, которые ожидают или работают в фоновом режиме, выполняя различные задачи. Как правило, демоны запускаются автоматически во время начальной загрузки системы и продолжают выполняться, пока система не будет выключена или они не будут остановлены вручную. По соглашению имена многих демонов заканчиваются на букву d.

Под _службой_ в контексте systemd часто подразумевается один или несколько демонов, однако запуск или остановка службы может вызвать единовременное изменение состояния системы, не предполагающее оставления процесса демона в запущенном состоянии (так называемый oneshot).

Во многих современных дистрибутивах Linux первым запускается процесс systemd (PID 1). Вот некоторые возможности, предоставляемые службой systemd:

* возможности распараллеливания (одновременный запуск нескольких служб), которые увеличивают скорость начальной загрузки системы;
    
* запуск демонов по запросу без использования отдельной службы;
    
* автоматическое управление зависимостями служб для предотвращения длительных периодов ожидания; (например, зависимая от сети служба не будет пытаться запуститься, пока сеть не будет доступна);
    
* совместное отслеживание связанных процессов с использованием контрольных групп (control group) Linux.
    

## Описание юнитов служб

systemd использует _юниты_ для управления различными типами объектов. Ниже приведены некоторые распространенные типы юнитов.

* Юниты служб имеют расширение .service и представляют системные службы. Этот тип юнита используется для запуска демонов, к которым часто происходит обращение, например веб-сервера.
    
* Юниты сокетов имеют расширение .socket и представляют сокеты для межпроцессного взаимодействия (IPC), которые служба systemd должна отслеживать. Если клиент подключается к сокету, systemd запускает демон и передает ему соединение. Юниты сокетов используются для отложенного запуска служб во время начальной загрузки системы, а также для запуска редко используемых служб по запросу.
    
* Юниты путей имеют расширение .path и используются для отложенной активации служб, которая выполняется при определенном изменении в файловой системе. Такая схема обычно применяется для служб, которые используют каталоги очереди, например в системе печати.
    

Для управления юнитами используется команда `systemctl`. Например, с помощью команды `systemctl -t help` можно отобразить доступные типы юнитов.

<details>
<summary>Важно</summary>

При использовании `systemctl` можно сокращать имена юнитов, элементы дерева процессов и описания юнитов.
</details>

## Отображение юнитов служб

Используйте команду `systemctl` для просмотра текущего состояния системы. Например, следующая команда отображает список всех загруженных в данный момент юнитов служб, разбивая вывод на страницы при помощи команды `less`.

```bash
[root@host ~]# systemctl list-units --type=service
UNIT                               LOAD   ACTIVE SUB     DESCRIPTION
atd.service                        loaded active running Job spooling tools
auditd.service                     loaded active running Security Auditing Service
chronyd.service                    loaded active running NTP client/server
crond.service                      loaded active running Command Scheduler
dbus.service                       loaded active running D-Bus System Message Bus
...output omitted...
```

В приведенном выше выводе опция `--type=service` ограничивает отображаемые типы юнитов юнитами служб. В выводе используются следующие столбцы.

**Столбцы в выводе команды `systemctl list-units`:**

* **UNIT** - Имя юнита службы.

* **LOAD** - Указывает, смогла ли служба systemd правильно проанализировать конфигурацию юнита и загрузить юнит в память.

* **ACTIVE** - Общие сведения о состоянии активации юнита. Указывает, был ли юнит успешно запущен.

* **SUB** - Дополнительные сведения о состоянии активации юнита. Здесь представлены более подробные сведения о юните, которые зависят от типа юнита, его состояния и способа выполнения.

* **DESCRIPTION** - Это краткое описание юнита.

По умолчанию команда `systemctl list-units --type=service` отображает только юниты служб с состоянием активации active. Опция `--all` отображает все юниты служб, независимо от состояния активации. Используйте опцию `--state=` для фильтрации по значениям полей LOAD, ACTIVE и SUB.

```bash
[root@host ~]# systemctl list-units --type=service --all
UNIT                          LOAD      ACTIVE   SUB     DESCRIPTION
  atd.service                 loaded    active   running Job spooling tools
  auditd.service              loaded    active   running Security Auditing ...
  auth-rpcgss-module.service  loaded    inactive dead    Kernel Module ...
  chronyd.service             loaded    active   running NTP client/server
  cpupower.service            loaded    inactive dead    Configure CPU power ...
  crond.service               loaded    active   running Command Scheduler
  dbus.service                loaded    active   running D-Bus System Message Bus
● display-manager.service     not-found inactive dead    display-manager.service
...output omitted...
```

Команда `systemctl` без аргументов отображает список юнитов, которые загружены и при этом активны.

```bash
[root@host ~]# systemctl
UNIT                                 LOAD   ACTIVE SUB       DESCRIPTION
proc-sys-fs-binfmt_misc.automount    loaded active waiting   Arbitrary...
sys-devices-....device               loaded active plugged   Virtio network...
sys-subsystem-net-devices-ens3.deviceloaded active plugged   Virtio network...
...
-.mount                              loaded active mounted   Root Mount
boot.mount                           loaded active mounted   /boot
...
systemd-ask-password-plymouth.path   loaded active waiting   Forward Password...
systemd-ask-password-wall.path       loaded active waiting   Forward Password...
init.scope                           loaded active running   System and Servi...
session-1.scope                      loaded active running   Session 1 of...
atd.service                          loaded active running   Job spooling tools
auditd.service                       loaded active running   Security Auditing...
chronyd.service                      loaded active running   NTP client/server
crond.service                        loaded active running   Command Scheduler
...output omitted...
```

Команда `systemctl list-units` отображает юниты, которые служба systemd пытается анализировать и загрузить в память. Она не отображает установленные, но отключенные службы. Чтобы просмотреть состояние всех установленных файлов юнитов, используйте команду `systemctl list-unit-files`. Пример:

```bash
[root@host ~]# systemctl list-unit-files --type=service
UNIT FILE                                   STATE
arp-ethers.service                          disabled
atd.service                                 enabled
auditd.service                              enabled
auth-rpcgss-module.service                  static
autovt@.service                             enabled
blk-availability.service                    disabled
...output omitted...
```

В выводе команды `systemctl list-units-files` допустимые значения в поле STATE ― enabled, disabled, static и masked.

## Просмотр состояний служб

Просмотреть состояние конкретного юнита можно с помощью `systemctl status name.type`. Если тип юнита не указан, команда `systemctl` отображает состояние юнита службы, если он существует.

```bash
[root@host ~]# systemctl status sshd.service
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2019-02-14 12:07:45 IST; 7h ago
 Main PID: 1073 (sshd)
   CGroup: /system.slice/sshd.service
           └─1073 /usr/sbin/sshd -D ...

Feb 14 11:51:39 host.example.com systemd[1]: Started OpenSSH server daemon.
Feb 14 11:51:39 host.example.com sshd[1073]: Could not load host key: /et...y
Feb 14 11:51:39 host.example.com sshd[1073]: Server listening on 0.0.0.0 ....
Feb 14 11:51:39 host.example.com sshd[1073]: Server listening on :: port 22.
Feb 14 11:53:21 host.example.com sshd[1270]: error: Could not load host k...y
Feb 14 11:53:22 host.example.com sshd[1270]: Accepted password for root f...2
...output omitted...
```

Эта команда отображает текущее состояние службы. Ниже поясняются значения полей.

**Таблица 9.1.1. Информация о юните службы**

| Поле | Описание |
| --- | --- |
| Loaded | Указывает, загружен ли юнит службы в память. |
| Active | Указывает, запущен ли юнит службы, и если да, то как долго он работает. |
| Main PID | Основной идентификатор процесса службы, включая имя команды. |
| Status | Дополнительная информация о службе. |

Далее приведены некоторые ключевые слова, указывающие на состояние службы, которые можно найти в выводе при запросе состояния.

**Таблица 9.1.2. Состояния служб в выводе команды systemctl**

| Ключевое слово | Описание |
| --- | --- |
| loaded | Файл конфигурации юнита был обработан. |
| active (running) | Служба запущена с одним или несколькими выполняющимися процессами. |
| active (exited) | Единовременная настройка успешно завершена. |
| active (waiting) | Служба запущена, но ожидает события. |
| inactive | Служба не запущена. |
| enabled | Служба запускается во время начальной загрузки системы. |
| disabled | Служба не настроена для запуска во время начальной загрузки системы. |
| статического | Служба не может быть включена, но может быть автоматически запущена включенным юнитом. |

<details>
<summary>Примечание</summary>

Команда `systemctl status NAME` заменяет команду `service NAME status`, которая использовалась в более ранних дистрибутивах.
</details>

## Проверка состояния службы

Команда `systemctl` позволяет проверить определенные состояния службы. Например, используйте следующую команду, чтобы убедиться, что юнит службы в данный момент активен (**active**).

```bash
[root@host ~]# systemctl is-active sshd.service
active
```

Команда возвращает состояние юнита службы. Обычно это **active** или **inactive**.

Выполните следующую команду, чтобы проверить, включен ли юнит службы для автоматического запуска во время начальной загрузки системы.

```bash
[root@host ~]# systemctl is-enabled sshd.service
enabled
```

Команда возвращает значение, которое указывает, включен ли юнит службы для запуска во время начальной загрузки. Обычно это **enabled** или **disabled**.

Чтобы проверить, не произошел ли сбой юнита во время запуска, выполните следующую команду.

```bash
[root@host ~]# systemctl is-failed sshd.service
active
```

Команда возвращает значение **active**, если юнит работает надлежащим образом, или **failed**, если во время запуска произошла ошибка. Если юнит был остановлен, команда возвращает значение **unknown** или **inactive**.

Чтобы отобразить все проблемные юниты (с состоянием **failed**), запустите команду `systemctl --failed --type=service`.
