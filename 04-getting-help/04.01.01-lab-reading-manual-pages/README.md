## Цель

Получение базовых навыков использования системы справочной документации Linux man и нахождения полезной информации с помощью функций поиска и просмотра.

## Задачи

В этом упражнении вы потренируетесь находить нужную информацию с помощью опций и аргументов man.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.
На **workstation** выполните команду `lab help-manual start`. Она создает файл с именем manual.

```
[student@workstation ~]$ lab help-manual start
```

1.	На **workstation** просмотрите man-страницу *gedit*. Просмотрите опции для редактирования определенного файла из командной строки с помощью команды `gedit`.

    Используйте одну из опций на man-странице *gedit*, чтобы с помощью команды `gedit` открыть файл **/home/student/manual** с курсором в конце файла.

    1.1.	Просмотрите man-страницу *gedit*.

    ```shell
    [student@workstation ~]$ man gedit
    GEDIT(1)    General Commands Manual      GEDIT(1)
    NAME
    gedit - text editor for the GNOME Desktop

    SYNOPSIS
    gedit [OPTION...] [FILE...] [+LINE[:COLUMN]]
    gedit [OPTION...] -
    ...output omitted...
    ```

    1.2.	На man-странице *gedit* просмотрите опции для редактирования определенного файла из командной строки.

    ```shell
    ...output omitted...
        FILE  Specifies the file  to open when gedit starts.
    ...output omitted...
        +LINE  For the first file, go to the line specified by LINE (do not insert a space between the "+" sign and the number). If LINE is missing, go to the last line.
    ...output omitted...
    ```

    Нажмите q для выхода с man-страницы.

    1.3.	С помощью команды `gedit +` откройте файл **manual**. Недостающий номер строки рядом с опцией `+` открывает файл, переданный в качестве аргумента, с курсором в конце последней строки.

    ```
    [student@workstation ~]$ gedit + manual
    the quick brown fox just came over to greet the lazy poodle!
    ```

    Убедитесь, что файл открыт и курсор находится в конце последней строки файла. Нажмите Ctrl+q, чтобы закрыть приложение.

2.	Прочитайте man-страницу *su(1)*.

    Обратите внимание, что, если пользователь не указан, команда su считает, что пользователь ― root. Если за командой su следует один дефис (-), она запускает дочернюю регистрационную оболочку (оболочку входа). Если дефис не используется, создается нерегистрационная дочерняя оболочка, соответствующая текущей среде пользователя.

    ```
    [student@workstation ~]$ man 1 su
    SU(1)              User Commands                      SU(1)
    NAME
        su - run a command with substitute user and group ID

    SYNOPSIS
        su [options] [-] [user [argument...]]

    DESCRIPTION
        su allows to run commands with a substitute user and group ID.
        When called without arguments, su defaults to running an interactive
        shell as root.
    ...output omitted...
    OPTIONS
    ...output omitted...
    -, -l, --login
        Start the shell as a login shell with an environment similar to a real login
    ...output omitted...
    ```

    <details>
    <summary>Примечание</summary>

    Обратите внимание, что разделенные запятой опции в одной строке (например, `-` , `-l` и `--login`) приводят к одному и тому же результату.

    </details>

    Нажмите q для выхода с man-страницы.

3.	У команды man также есть свои страницы руководства.

    ```
    [student@workstation ~]$ man man
    MAN(1)             Manual pager utils                                 MAN(1)

    NAME
    man - an interface to the on-line reference manuals
    ...output omitted...
    DESCRIPTION
        man is the system's manual pager. Each page argument given to man is
        normally the name of a program, utility or function. The manual page
        associated  with  each  of these arguments is then found and displayed.
        A section, if provided, will direct man to look only in that section
        of the manual.
    ...output omitted...
    ```

Нажмите q для выхода с man-страницы.

4.	Все man-страницы находятся в **/usr/share/man**. Двоичные файлы, файлы исходного кода и страницы руководства, находящиеся в каталоге **/usr/share/man**, можно найти с помощью команды `whereis`.

    ```
    [student@workstation ~]$ whereis passwd
    passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz /usr/share/man/man5/passwd.5.gz
    ```

5.	Используйте команду `man -k zip`, чтобы просмотреть подробные сведения о ZIP-архиве.

    ```
    [student@workstation ~]$ man -k zip
    ...output omitted...
    zipinfo (1)          - list detailed information about a ZIP archive
    zipnote (1)          - write the comments in zipfile to stdout, edit comments and rename files in zipfile
    zipsplit (1)         - split a zipfile into smaller zipfiles
    ```


6.	Используйте команду `man -k boot` для отображения man-страницы со списком параметров, которые могут быть переданы в ядро во время начальной загрузки.

    ```
    [student@workstation ~]$ man -k boot
    ...output omitted...
    bootctl (1)          - Control the firmware and boot manager settings
    bootparam (7)        - introduction to boot time parameters of the Linux kernel
    bootup (7)           - System bootup process
    ...output omitted...
    ```

7.	Используйте команду man -k ext4 для поиска команды, которая используется для настройки параметров файловой системы ext4.

    ```
    [student@workstation ~]$ man -k ext4
    ...output omitted...
    resize2fs (8)        - ext2/ext3/ext4 file system resizer
    tune2fs (8)          - adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems
    ```

## Конец

На **workstation** запустите сценарий `lab help-manual finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab help-manual finish
```

Упражнение завершено.
