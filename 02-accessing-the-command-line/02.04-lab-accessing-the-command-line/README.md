## Цель

Получение базовых навыков использования командной оболочки Bash для выполнения команд.

## Задачи

* Успешный запуск простых программ с помощью командной строки Bash
* Выполнение команд, с помощью которых можно определять типы файлов и отображать фрагменты текстовых файлов
* Тренировка в использовании некоторых сочетаний клавиш Bash для более эффективного повтора команд или части команд

## Инструкции

Войдите на **workstation** как пользователь **student** с паролем **student**.

На **workstation** запустите сценарий `lab cli-review start` для настройки среды лабораторной работы. Сценарий также копирует файл zcat в домашний каталог пользователя student.

```shell
[student@workstation ~]$ lab cli-review start
```

1.	Используйте команду date для отображения даты и времени.

  ```shell
  [student@workstation ~]$ date
  Thu Jan 22 10:13:04 PDT 2019
  ```

2.	Отобразите текущее время в 12-часовом формате (например, 11:42:11 AM). 

  <details>
  <summary>Поcмотреть решение</summary>

  Такой формат отображения данных обеспечивает аргумент %r.
  Используйте аргумент +%r с командой date для отображения текущего времени в 12-часовом формате.


  ```shell
  [student@workstation ~]$ date +%r
  10:14:07 AM
  ```
  </details>

3.	Что из себя представляет файл /home/student/zcat? Предназначен ли он для чтения человеком?

  <details>
  <summary>Поcмотреть решение</summary>

  Определите тип этого файла с помощью команды file.

  ```shell
  [student@workstation ~]$ file zcat
  zcat: POSIX shell script, ASCII text executable
  ```
  </details>

4.	Используйте команду wc и сочетания клавиш Bash для отображения размера файла zcat.

  <details>
  <summary>Поcмотреть решение</summary>

  Команда wc может использоваться для отображения количества строк, слов и байтов в сценарии zcat. Вместо повторного ввода имени файла воспользуйтесь сочетанием клавиш истории Bash **Esc+.** (одновременно нажмите клавиши Esc и .), чтобы применить аргумент из предыдущей команды.

  ```shell
  [student@workstation ~]$ wc Esc+.
  [student@workstation ~]$ wc zcat
    51  299 1983 zcat
  ```
  </details>

5.	Отобразите первые 10 строк файла zcat.

  <details>
  <summary>Поcмотреть решение</summary>

  Команда `head` отображает начальную часть файла. Попробуйте снова использовать сочетание клавиш **Esc+.**.

  ```shell
  [student@workstation ~]$ head Esc+.
  [student@workstation ~]$ head zcat
  #!/bin/sh
  # Uncompress files to standard output.

  # Copyright (C) 2007, 2010-2018 Free Software Foundation, Inc.

  # This program is free software; you can redistribute it and/or modify
  # it under the terms of the GNU General Public License as published by
  # the Free Software Foundation; either version 3 of the License, or
  # (at your option) any later version.
  ```
  </details>

6.	Отобразите последние 10 строк файла zcat.

  <details>
  <summary>Поcмотреть решение</summary>

  Используйте команду `tail` для отображения последних 10 строк файла zcat.

  ```shell
  [student@workstation ~]$ tail Esc+.
  [student@workstation ~]$ tail zcat
  With no FILE, or when FILE is -, read standard input.
  Report bugs to <bug-gzip@gnu.org>."
  case $1 in
  --help)    printf '%s\n' "$usage"   || exit 1;;
  --version) printf '%s\n' "$version" || exit 1;;
  esac
  exec gzip -cd "$@"
  ```
  </details>

7.	Повторите предыдущую команду, нажимая клавиши три раза или меньше.

  <details>
  <summary>Поcмотреть решение</summary>

  В точности повторите предыдущую команду. Нажмите клавишу Стрелка вверх один раз, чтобы прокрутить историю команд назад на одну команду, а затем нажмите **Enter** (это два нажатия клавиш) или введите команду быстрого доступа `!!`, а затем нажмите Enter (это три нажатия клавиш) для выполнения самой последней команды в истории команд. (Попробуйте оба варианта.)

  ```shell
  [student@workstation]$ !!
  tail zcat
  With no FILE, or when FILE is -, read standard input.

  Report bugs to <bug-gzip@gnu.org>."

  case $1 in
  --help)    printf '%s\n' "$usage"   || exit 1;;
  --version) printf '%s\n' "$version" || exit 1;;
  esac

  exec gzip -cd "$@"
  ```
  </details>

8.	Повторите предыдущую команду, но используйте опцию `-n 20` для отображения последних 20 строк файла. Используйте функцию редактирования командной строки для достижения результата с минимальным количеством нажатий клавиш.

  <details>
  <summary>Поcмотреть решение</summary>

  Нажмите стрелку вверх, чтобы отобразить предыдущую команду. Нажмите **Ctrl+A**, чтобы переместить курсор в начало строки. Нажмите **Ctrl+Стрелка вправо**, чтобы перейти к следующему слову, затем введите опцию `-n 20` и нажмите **Enter**, чтобы выполнить команду.

  ```shell
  [student@workstation ~]$ tail -n 20 zcat
    -l, --list        list compressed file contents
    -q, --quiet       suppress all warnings
    -r, --recursive   operate recursively on directories
    -S, --suffix=SUF  use suffix SUF on compressed files
        --synchronous synchronous output (safer if system crashes, but slower)
    -t, --test        test compressed file integrity
    -v, --verbose     verbose mode
        --help        display this help and exit
        --version     display version information and exit

  With no FILE, or when FILE is -, read standard input.

  Report bugs to <bug-gzip@gnu.org>."

  case $1 in
  --help)    printf '%s\n' "$usage"   || exit 1; exit;;
  --version) printf '%s\n' "$version" || exit 1; exit;;
  esac

  exec gzip -cd "$@"
  ```
  </details>

9.	Используйте историю оболочки для повтора команды `date +%r`.

  <details>
  <summary>Поcмотреть решение</summary>

  Используйте команду `history`, чтобы отобразить список предыдущих команд и определить конкретную команду `date`, которую необходимо выполнить. Используйте `!number`, чтобы выполнить команду, где *number* — это номер команды, которую необходимо использовать, из вывода команды history.

  Обратите внимание, что история вашей оболочки может отличаться от следующего примера. Определите номер используемой команды на основе вывода вашей команды `history`.

  ```shell
  [student@workstation ~]$ history
  1   date
  2   date +%r
  3   file zcat
  4   wc zcat
  5   head zcat
  6   tail zcat
  7   tail -n 20 zcat
  8   history
  [student@workstation ~]$ !2
  date +%r
  10:49:56 AM
  ```
  </details>

## Оценка

На workstation запустите сценарий `lab cli-review grade`, чтобы проверить, правильно ли было выполнено упражнение.

```shell
[student@workstation ~]$ lab cli-review grade
```

## Конец

На workstation запустите сценарий `lab cli-review finish`, чтобы закончить лабораторную работу.

```shell
[student@workstation ~]$ lab cli-review finish
```

Лабораторная работа завершена.
