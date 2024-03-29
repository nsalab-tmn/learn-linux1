## Цели


После завершения этого раздела вы сможете найти каталог в иерархии файловой системы и определить, на каком устройстве он хранится.

## Концепции управления хранилищами

Файлы на сервере Linux доступны через иерархию файловой системы ― единое инвертированное дерево каталогов. Эта иерархия состоит из файловых систем, предоставляемых запоминающими устройствами в вашей системе. Каждая файловая система — это запоминающее устройство, отформатированное для хранения файлов.

В иерархии файловой системы Linux набор файловых систем, находящихся на отдельных запоминающих устройствах, можно представить в виде одного набора файлов на одном гигантском запоминающем устройстве, по которому вы можете перемещаться. В большинстве случаев вам не нужно знать, на каком запоминающем устройстве находится конкретный файл, достаточно знать каталог.

Однако в некоторых случаях такая информация необходима. Например, может потребоваться определить, насколько заполнено запоминающее устройство и какие каталоги файловой системы затронуты. Если в log-файлах есть записи об ошибках на запоминающих устройствах, может потребоваться узнать, какие файловые системы подвержены риску. А при создании жесткой ссылки между двумя файлами необходимо знать, находятся они в одной файловой системе или нет.

### Файловые системы и точки монтирования

Чтобы сделать содержимое файловой системы доступным в иерархии файловой системы, необходимо смонтировать ее в пустой каталог. Такой каталог называется точкой монтирования. Если после монтирования вы выполните команду ls для отображения содержимого каталога, то увидите содержимое смонтированной файловой системы и сможете обращаться к этим файлам и использовать их обычным образом. Многие файловые системы монтируются автоматически в ходе начальной загрузки системы.

Эта концепция принципиально отличается от букв дисков в Microsoft Windows. Отчасти она похожа на функцию монтируемых папок NTFS.

### Файловые системы, запоминающие и блочные устройства

Низкоуровневый доступ к запоминающим устройствам в Linux обеспечивается специальным типом файла, который называется блочным устройством. Эти блочные устройства необходимо отформатировать под определенную файловую систему, прежде чем их можно будет смонтировать.

Файлы блочных устройств хранятся в каталоге /dev вместе с другими файлами устройств. Файлы устройств автоматически создаются операционной системой. В Linux первый обнаруженный жесткий диск SATA/PATA, SAS, SCSI или USB называется /dev/sda, второй — /dev/sdb и т. д. Эти имена представляют целый жесткий диск.

Другие типы хранилищ именуются другим образом.

**Таблица 15.1.1. Именование блочных устройств**

| **Тип** **устройства** | **Шаблон** **именования** **устройств** |
| --- | --- |
| Хранилище, подключенное по SATA/SAS/USB | /dev/sda, /dev/sdb ... |
| Паравиртуализированное хранилище <br><br>virtio-blk (виртуальное блочное устройство) | /dev/vda, /dev/vdb ... |
| Хранилище, подключенное по NVMe (много SSD-накопителей) | /dev/nvme0, /dev/nvme1 ... |
| Хранилище SD/MMC/eMMC (SD-карты) | /dev/mmcblk0, /dev/mmcblk1 ... |

<details>
<summary>Примечание</summary>

Многие виртуальные машины используют более новое паравиртуализированное хранилище virtio-scsi, которое именуется в формате /dev/sd*.
</details>

### Разделы дисков

Как правило, вы не занимаете все запоминающее устройство одной файловой системой. Запоминающие устройства обычно разбиваются на меньшие блоки данных, которые называются разделами.

Разделы позволяют поделить диск на части: разные разделы можно отформатировать под разные файловые системы и использовать для разных целей. Например, один раздел может содержать домашние каталоги пользователей, а другой — системные данные и log-файлы. Если пользователь полностью заполнит раздел домашних каталогов данными, в системном разделе все равно может остаться свободное место.

Разделы сами по себе являются блочными устройствами. На запоминающем устройстве, подключенном к SATA, первый раздел на первом диске — **/dev/sda1**. Третий раздел на втором диске — **/dev/sdb3** и т.д. У паравиртуализированных запоминающих устройств аналогичная система именования.
На SSD-устройствах, подключенных по NVMe, разделы называются по-другому. Первый раздел на первом диске — **/dev/nvme0p1**. Третий раздел на втором диске — **/dev/nvme1p3** и т. д. У карт SD и MMC похожая система именования.

Длинный формат отображения файла устройства **/dev/sda1** на хосте **host** показывает его особый тип файла как b, что означает блочное устройство.

```bash
[user@host ~]$ ls -l /dev/sda1
brw-rw----. 1 root disk 8, 1 Feb 22 08:00 /dev/sda1
```

### Логические тома

Еще один способ упорядочения дисков и разделов — с помощью системы управления логическими томами (LVM). **LVM** дает возможность агрегировать одно или несколько блочных устройств в единый пул хранения данных, который называется группой томов. Дисковое пространство в группе томов затем распределяется по одному или нескольким логическим томам, которые являются функциональным эквивалентом раздела на физическом диске.

Система LVM присваивает имена группам томов и логическим томам при создании. LVM создает каталог в **/dev**, который соответствует имени группы, а затем создает символьную ссылку в этом новом каталоге с тем же именем, что и у логического тома. Этот файл логического тома будет доступен для монтирования. Например, если группа томов называется **myvg**, а логический том в ней называется **mylv**, тогда полным путем к файлу устройства логического тома будет **/dev/myvg/mylv**.

<details>
<summary>Примечание</summary>

Упомянутое выше имя устройства логического тома реализуется в виде символьной ссылки на фактический файл устройства, используемый для доступа к логическому тому, и может изменяться при перезагрузке системы. Существует еще один распространенный вид именования устройства логического тома. Он связан с файлами **/dev/mapper** и также представляет собой символьные ссылки на фактический файл устройства.
</details>

## Получение сведений о файловых системах

Чтобы получить общие сведения об устройствах локальных и удаленных файловых систем и объемах свободного пространства, выполните команду `df`. Если команда `df` выполняется без аргументов, она отображает общий объем места на диске, объем занятого и свободного дискового пространства, а также процент занятого дискового пространства во всех смонтированных обычных файловых системах. Отображаются сведения как для локальных, так и для удаленных файловых систем.

В следующем примере показаны файловые системы и точки монтирования в системе **host**.

```bash
[user@host ~]$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
devtmpfs          912584       0    912584   0% /dev
tmpfs             936516       0    936516   0% /dev/shm
tmpfs             936516   16812    919704   2% /run
tmpfs             936516       0    936516   0% /sys/fs/cgroup
/dev/vda3        8377344 1411332   6966012  17% /
/dev/vda1        1038336  169896    868440  17% /boot
tmpfs             187300       0    187300   0% /run/user/1000
```

Схема разделов на **host** показывает две физические файловые системы, которые смонтированы в **/** и **/boot**. Это обычная ситуация для виртуальных машин. Устройства **tmpfs** и **devtmpfs** — это файловые системы в системной памяти. Все файлы, записываемые в **tmpfs** и **devtmpfs**, исчезают после перезагрузки системы.

Для повышения читаемости размеров в выводе предлагаются две опции, понятные для пользователя: `-h` и `-H`. Разница между этими двумя опциями в том, что `-h` отображает результаты в КиБ (2<sup>10</sup>), МиБ (2<sup>20</sup>) и ГиБ (2<sup>30</sup>), а опция `-H` отображает результаты в единицах СИ: КБ (10<sup>3</sup>), МБ (10<sup>6</sup>), ГБ (10<sup>9</sup>) и т. д. Производители жестких дисков обычно используют единицы СИ при продвижении своей продукции.

Отобразите отчет о файловых системах на host с преобразованием всех единиц в формат, понятный для пользователя.

```bash
[user@host ~]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        892M     0  892M   0% /dev
tmpfs           915M     0  915M   0% /dev/shm
tmpfs           915M   17M  899M   2% /run
tmpfs           915M     0  915M   0% /sys/fs/cgroup
/dev/vda3       8.0G  1.4G  6.7G  17% /
/dev/vda1      1014M  166M  849M  17% /boot
tmpfs           183M     0  183M   0% /run/user/1000
```

Чтобы получить более подробные сведения об использовании дискового пространства определенным деревом каталогов, используйте команду `du`. У команды `du` есть опции `-h` и `-H` для преобразования вывода в формат, понятный для пользователя. Команда `du` рекурсивно отображает размер всех файлов в текущем дереве каталогов.

Отобразите отчет об использовании диска для каталога **/usr/share** на **host**.

```bash
[root@host ~]# du /usr/share    
...output omitted...
176 /usr/share/smartmontools
184 /usr/share/nano
8 /usr/share/cmake/bash-completion
8 /usr/share/cmake
356676  /usr/share
```

Отобразите отчет об использовании диска для каталога **/usr/share** на **host** в формате, понятном для пользователя.

```bash
[root@host ~]# du -h /var/log
...output omitted...
176K  /usr/share/smartmontools
184K  /usr/share/nano
8.0K  /usr/share/cmake/bash-completion
8.0K  /usr/share/cmake
369M  /usr/share
```
