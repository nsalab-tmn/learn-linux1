## Цели

После завершения этого раздела вы сможете создавать, копировать, перемещать и удалять файлы и каталоги.

## Управление файлами из командной строки

Управление файлами подразумевает их создание, удаление, копирование и перемещение. Кроме того, необходимо организовывать их в каталоги, которые также можно создавать, удалять, копировать и перемещать.

В следующей таблице приведены некоторые распространенные команды для управления файлами. В оставшейся части этого раздела мы рассмотрим использование этих команд более подробно.

**Таблица 3.3.1** Распространенные команды управления файлами

| Действие | Синтаксис команды |
| --- | --- |
| Создание каталога | **mkdir** _**directory**_ |
| Копирование файла | **cp** _**file**_ _**new-file**_ |
| Копирование каталога и его содержимого | **cp** **-r** _**directory**_ _**new-directory**_ |
| Перемещение или переименование файла или каталога | **mv** _**file**_ _**new-file**_ |
| Удаление файла | **rm** _**file**_ |
| Удаление каталога с файлами | **rm** **-r** _**directory**_ |
| Удаление пустого каталога | **rmdir** _**directory**_ |

## Создание каталогов

С помощью команды `mkdir` можно создавать каталоги и подкаталоги. В качестве аргументов она принимает список путей к каталогам, которые необходимо создать.

Команда `mkdir` завершится с ошибкой, если каталог уже существует или вы пытаетесь создать подкаталог в каталоге, который не существует. Опция `-p` (родительский) создает недостающие родительские каталоги для запрошенного целевого расположения. Будьте осторожны при использовании команды `mkdir -p`, так как опечатки могут привести к созданию неправильных каталогов, при этом не будет никаких сообщений об ошибках.

В следующем примере представьте, что вы пытались создать подкаталог с именем **Watched** в каталоге **Videos**, но случайно пропустили букву `s` в имени **Videos** в команде `mkdir`.

```shell
[user@host ~]$ mkdir Video/Watched
mkdir: cannot create directory `Video/Watched': No such file or directory
```

Выполнение команды `mkdir` завершилось ошибкой, так как каталог **Videos** был указан с ошибкой, а каталог **Video** не существует. Если бы вы использовали команду `mkdir` с опцией `-p`, был бы создан каталог **Video** (что не соответствует вашим намерениям) и подкаталог **Watched** был бы создан в этом неправильном каталоге.

Правильное написание имени родительского каталога **Videos** приведет к успешному созданию подкаталога Watched.

```shell
[user@host ~]$ mkdir Videos/Watched
[user@host ~]$ ls -R Videos
Videos/:
blockbuster1.ogg  blockbuster2.ogg  Watched

Videos/Watched:
```

В следующем примере файлы и каталоги организованы в каталоге **/home/user/Documents**. Используйте команду `mkdir` и список имен каталогов, разделенных пробелами, для создания нескольких каталогов.

```shell
[user@host ~]$ cd Documents
[user@host Documents]$ mkdir ProjectX ProjectY
[user@host Documents]$ ls
ProjectX  ProjectY
```

Используйте команду `mkdir -p` и список относительных путей к каждому подкаталогу, разделенных пробелами, для создания нескольких родительских каталогов с подкаталогами.

```shell
[user@host Documents]$ mkdir -p Thesis/Chapter1 Thesis/Chapter2 Thesis/Chapter3
[user@host Documents]$ cd
[user@host ~]$ ls -R Videos Documents
Documents:
ProjectX  ProjectY  Thesis

Documents/ProjectX:

Documents/ProjectY:

Documents/Thesis:
Chapter1  Chapter2  Chapter3
Documents/Thesis/Chapter1:
Documents/Thesis/Chapter2:
Documents/Thesis/Chapter3:
Videos:
blockbuster1.ogg  blockbuster2.ogg  Watched

Videos/Watched:
```

Последняя команда `mkdir` создала одновременно три подкаталога ChapterN. Опция `-p` создала отсутствующий родительский каталог **Thesis**.

## Копирование файлов

Команда `cp` копирует файл, создавая новый файл в текущем или указанном каталоге. Она также может скопировать несколько файлов в каталог.

<details>
<summary>Предупреждение</summary>
Если целевой файл уже существует, команда cp перезаписывает файл.
</details>

```shell
[user@host ~]$ cd Videos
[user@host Videos]$ cp blockbuster1.ogg blockbuster3.ogg
[user@host Videos]$ ls -l
total 0
-rw-rw-r--. 1 user user    0 Feb  8 16:23 blockbuster1.ogg
-rw-rw-r--. 1 user user    0 Feb  8 16:24 blockbuster2.ogg
-rw-rw-r--. 1 user user    0 Feb  8 16:34 blockbuster3.ogg
drwxrwxr-x. 2 user user 4096 Feb  8 16:05 Watched
[user@host Videos]$
```

При копировании нескольких файлов с помощью одной команды последний аргумент должен быть каталогом. Скопированные файлы сохраняют свои исходные имена в новом каталоге. Если файл с таким же именем существует в целевом каталоге, существующий файл будет перезаписан. По умолчанию `cp` не копирует каталоги, а игнорирует их.

В следующем примере показаны два каталога: **Thesis** и **ProjectX**. Только последний аргумент (ProjectX) является допустимым назначением. Каталог Thesis игнорируется.

```shell
[user@host Videos]$ cd ../Documents
[user@host Documents]$ cp thesis_chapter1.odf thesis_chapter2.odf Thesis ProjectX
cp: omitting directory `Thesis'
[user@host Documents]$ ls Thesis ProjectX
ProjectX:
thesis_chapter1.odf  thesis_chapter2.odf

Thesis:
Chapter1  Chapter2  Chapter3
```

В первой команде `cp` не удалось скопировать каталог Thesis, но удалось скопировать каталоги **thesis_chapter1.odf** и **thesis_chapter2.odf**.

Чтобы скопировать файл в текущий рабочий каталог, можно использовать специальный каталог «`.`».

```shell
[user@host ~]$ cp /etc/hostname .
[user@host ~]$ cat hostname
host.example.com
[user@host ~]$ 
```

Используйте команду копирования с опцией `-r` (рекурсивно), чтобы скопировать каталог **Thesis** и его содержимое в каталог **ProjectX**.

```shell
[user@host Documents]$ cp -r Thesis ProjectX
[user@host Documents]$ ls -R ProjectX
ProjectX:
Thesis  thesis_chapter1.odf  thesis_chapter2.odf
ProjectX/Thesis:
Chapter1  Chapter2  Chapter3
ProjectX/Thesis/Chapter1:
ProjectX/Thesis/Chapter2:
thesis_chapter2.odf
ProjectX/Thesis/Chapter3:
```

## Перемещение файлов

Команда `mv` перемещает файлы из одного места в другое. Если представить, что абсолютный путь к файлу ― это его полное имя, то перемещение файла аналогично переименованию файла. Содержимое файла остается неизменным.

Используйте команду `mv`, чтобы переименовать файл.

```shell
[user@host Videos]$ cd ../Documents
[user@host Documents]$ ls -l thesis*
-rw-rw-r--. 1 user user 0 Feb  6 21:16 thesis_chapter1.odf
-rw-rw-r--. 1 user user 0 Feb  6 21:16 thesis_chapter2.odf
[user@host Documents]$ mv thesis_chapter2.odf thesis_chapter2_reviewed.odf
[user@host Documents]$ ls -l thesis*
-rw-rw-r--. 1 user user 0 Feb  6 21:16 thesis_chapter1.odf
-rw-rw-r--. 1 user user 0 Feb  6 21:16 thesis_chapter2_reviewed.odf
```

Используйте команду `mv`, чтобы переместить файл в другой каталог.

```shell
[user@host Documents]$ ls Thesis/Chapter1
[user@host Documents]$
[user@host Documents]$ mv thesis_chapter1.odf Thesis/Chapter1
[user@host Documents]$ ls Thesis/Chapter1
thesis_chapter1.odf
[user@host Documents]$ ls -l thesis*
-rw-rw-r--. 1 user user 0 Feb  6 21:16 thesis_chapter2_reviewed.odf
```

## Удаление файлов и каталогов

Команда `rm` удаляет файлы. По умолчанию команда `rm` не удаляет каталоги, в которых есть файлы, если вы не добавите опцию `-r` или `--recursive`.

<details>
<summary>Важно</summary>

В командной строке возможность восстановления удаленных файлов не предусмотрена. Также нет корзины, из которой можно было бы восстановить удаленные файлы.
</details>

Перед удалением файла или каталога рекомендуется проверить текущий рабочий каталог.

```shell
[user@host Documents]$ pwd
/home/student/Documents
```

Используйте команду `rm` для удаления одного файла из рабочего каталога.

```shell
[user@host Documents]$ ls -l thesis*
-rw-rw-r--. 1 user user 0 Feb  6 21:16 thesis_chapter2_reviewed.odf
[user@host Documents]$ rm thesis_chapter2_reviewed.odf
[user@host Documents]$ ls -l thesis*
ls: cannot access 'thesis*': No such file or directory
```

Если вы попытаетесь использовать команду `rm` без опции `-r` для удаления каталога, ее выполнение завершится ошибкой.

```shell
[user@host Documents]$ rm Thesis/Chapter1
rm: cannot remove `Thesis/Chapter1': Is a directory
```

Используйте команду `rm -r`, чтобы удалить подкаталог и его содержимое.

```shell
[user@host Documents]$ ls -R Thesis
Thesis/:
Chapter1  Chapter2  Chapter3
Thesis/Chapter1:
thesis_chapter1.odf
Thesis/Chapter2:
thesis_chapter2.odf
Thesis/Chapter3:
[user@host Documents]$ rm -r Thesis/Chapter1
[user@host Documents]$ ls -l Thesis
total 8
drwxrwxr-x. 2 user user 4096 Feb 11 12:47 Chapter2
drwxrwxr-x. 2 user user 4096 Feb 11 12:48 Chapter3
```

Команда `rm -r` сначала перебирает каждый подкаталог, удаляя их файлы по отдельности перед удалением каждого каталога. Вы можете использовать команду `rm -ri` для запроса подтверждения перед удалением. Это, по сути, противоположно использованию опции `-f`, которая принудительно удаляет данные без запроса подтверждения.

```shell
[user@host Documents]$ rm -ri Thesis
rm: descend into directory `Thesis'? y
rm: descend into directory `Thesis/Chapter2'? y
rm: remove regular empty file `Thesis/Chapter2/thesis_chapter2.odf'? y
rm: remove directory `Thesis/Chapter2'? y
rm: remove directory `Thesis/Chapter3'? y
rm: remove directory `Thesis'? y
[user@host Documents]$
```

<details>
<summary>Предупреждение</summary>

Если указать опции `-i` и `-f` одновременно, опция `-f` будет иметь приоритет и вам не будет предложено подтвердить действие перед удалением файлов командой `rm`.
</details>

В следующем примере команда `rmdir` удаляет только пустой каталог. Как и в предыдущем примере, необходимо использовать команду `rm -r` для удаления каталога, который содержит данные.

```shell
[user@host Documents]$ pwd
/home/student/Documents
[user@host Documents]$ rmdir ProjectY
[user@host Documents]$ rmdir ProjectX
rmdir: failed to remove `ProjectX': Directory not empty
[user@host Documents]$ rm -r ProjectX
[user@host Documents]$ ls -lR
.:
total 0
[user@host Documents]$
```

Команда `rm` без опций не может удалить пустой каталог. Необходимо использовать такую команду `rmdir`, как `rm -d` (что эквивалентно `rmdir`) или `rm -r`.