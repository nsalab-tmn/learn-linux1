## Цели

Получение базовых навыков использования функции управления заданиями для приостановки и перезапуска пользовательских процессов.

## Задачи

В этом упражнении вы будете запускать, приостанавливать, переводить в фоновый и активный режим несколько процессов с помощью функций управления заданиями.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab processes-control start`. Этот сценарий проверяет доступность хоста **servera**.

```
[student@workstation ~]$ lab processes-control start
```

1.	На **workstation** откройте рядом два окна терминала. В этом разделе эти два терминала называются левым и правым. На каждом терминале с помощью команды `ssh` войдите на хост **servera** как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	В левом окне создайте новый каталог с именем **/home/student/bin**. В новом каталоге создайте сценарий оболочки с именем **control**. Сделайте сценарий исполняемым.

    2.1.	С помощью команды `mkdir` создайте новый каталог с именем **/home/student/bin**.

    ```
    [student@servera ~]$ mkdir /home/student/bin
    ```

    2.2.	С помощью команды `vim` создайте сценарий с именем **control** в каталоге **/home/student/bin**. Чтобы перейти в интерактивный режим Vim, нажмите клавишу **i**. Выполните команду `:wq`, чтобы сохранить файл.

    ```
    [student@servera ~]$ vim /home/student/bin/control
    #!/bin/bash
    while true; do
    echo -n "$@ " >> ~/control_outfile
    sleep 1
    done 
    ```

    <details>
    <summary>Примечание</summary>

    Сценарий control работает до тех пор, пока не будет завершен. Он добавляет аргументы командной строки в файл **~/control_outfile** один раз в секунду.
    </details>

    2.3.	Используйте команду `chmod`, чтобы сделать файл **control** исполняемым.

    ```
    [student@servera ~]$ chmod +x /home/student/bin/control
    ```

3.	Запустите сценарий **control** с аргументом `technical`. Сценарий непрерывно добавляет слово «technical» и пробел в файл **~/control_outfile** с интервалом в одну секунду.

    <details>
    <summary>Примечание</summary>

    Вы можете выполнить сценарий control, поскольку он находится в вашем PATH и сделан исполняемым.
    </details>

    ```
    [student@servera ~]$ control technical 
    ```

4.	В командной оболочке правого терминала выполните команду `tail` с опцией `-f`, чтобы убедиться, что новый процесс производит запись в файл **/home/student/control_outfile**.

    ```
    [student@servera ~]$ tail -f ~/control_outfile
    technical technical technical technical
    ...output omitted... 
    ```

5.	В командной оболочке левого терминала нажмите **Ctrl+z**, чтобы приостановить запущенный процесс. При этом командная оболочка вернет идентификатор задания в квадратных скобках. В правом окне убедитесь, что вывод данных процесса прекратился.

    ```
    ^Z
    [1]+  Stopped                 control technical
    [student@servera ~]$ 
    technical technical technical technical
    ...no further output... 
    ```

6.	В командной оболочке левого терминала просмотрите список **jobs**. Помните, что знак **+** указывает на задание по умолчанию. Перезапустите задание в фоновом режиме. В командной оболочке правого терминала убедитесь, что вывод данных процесса снова активен.

    6.1.	С помощью команды `jobs` отобразите список заданий.

    ```
    [student@servera ~]$ jobs
    [1]+  Stopped                 control technical 
    ```

    6.2.	Выполните команду `bg`, чтобы запустить задание `control` в фоновом режиме.

    ```
    [student@servera ~]$ bg
    [1]+ control technical & 
    ```

    6.3.	С помощью команды `jobs` убедитесь, что задание `control` снова выполняется.

    ```
    [student@servera ~]$ jobs
    [1]+  Running                 control technical & 
    ```

    6.4.	В командной оболочке правого терминала убедитесь, что команда `tail` выводит данные.

    ```
    ...output omitted...
    technical technical technical technical technical technical technical technical 
    ```

7.	В командной оболочке левого терминала запустите еще два процесса `control` для добавления вывода в файл **~/output**. Используйте амперсанд (**&**), чтобы запустить процессы в фоновом режиме. Замените **technical** на **documents**, а затем на **database**. Замена аргументов помогает различить эти три процесса.

    ```
    [student@servera ~]$ control documents &
    [2] 6579
    [student@servera ~]$
    [student@servera ~]$ control database &
    [3] 6654 
    ```

    <details>
    <summary>Примечание</summary>

    Номер задания каждого нового процесса указывается в квадратных скобках. Второй номер ― это уникальный системный идентификационный номер процесса (PID).
    </details>

8.	В командной оболочке левого терминала выполните команду `jobs`, чтобы отобразить три запущенных процесса. В командной оболочке левого терминала убедитесь, что все три процесса добавляют вывод в файл.

```
[student@servera ~]$ jobs
[1]   Running                 control technical &
[2]-  Running                 control documents &
[3]+  Running                 control database &
...output omitted...
technical documents database technical documents database technical documents database technical documents database
```

9.	Приостановите процесс `control technical`. Убедитесь, что он был приостановлен. Завершите процесс `control documents` и убедитесь, что он прекращен.

    9.1.	В командной оболочке левого терминала выполните команду `fg` с идентификатором задания, чтобы перевести процесс `control technical` в активный режим. Нажмите **Ctrl+z**, чтобы приостановить процесс. Выполните команду `jobs`, чтобы убедиться, что процесс приостановлен.

    ```
    [student@servera ~]$ fg %1
    control technical
    ^Z
    [1]+  Stopped                 control technical
    [student@servera ~]$ jobs
    [1]+  Stopped                 control technical
    [2]   Running                 control documents &
    [3]-  Running                 control database &
    ```

    9.2.	В командной оболочке правого терминала убедитесь, что процесс `control technical` больше не выводит данные.

    ```
    database documents  database documents  database
    ...no further output... 
    ```

    9.3.	В командной оболочке левого терминала выполните команду `fg` с идентификатором задания, чтобы перевести процесс `control documents` в активный режим. Нажмите **Ctrl+c**, чтобы завершить процесс. Выполните команду `jobs`, чтобы убедиться, что процесс завершен.

    ```
    [student@servera ~]$ fg %2
    control documents
    ^C
    [student@servera ~]$ jobs
    [1]+  Stopped                 control technical
    [3]-  Running                 control database &
    ```

    9.4.	В командной оболочке правого терминала убедитесь, что процесс `control documents` больше не выводит данные.

    ```
    ...output omitted...
    database database database database database database database database
    ...no further output... 
    ```

10.	В левом окне выполните команду `ps` с опцией `jT` для отображения оставшихся заданий. Приостановленные задания имеют состояние **T**. Другие фоновые задания находятся в состоянии сна (**S**).

    ```
    [student@servera ~]$ ps jT
    PPID   PID  PGID   SID TTY      TPGID STAT   UID   TIME COMMAND
    27277 27278 27278 27278 pts/1    28702 Ss    1000   0:00 -bash
    27278 28234 28234 27278 pts/1    28702 T     1000   0:00 /bin/bash /home/student/bin/control technical
    27278 28251 28251 27278 pts/1    28702 S     1000   0:00 /bin/bash /home/student/bin/control database
    28234 28316 28234 27278 pts/1    28702 T     1000   0:00 sleep 1
    28251 28701 28251 27278 pts/1    28702 S     1000   0:00 sleep 1
    27278 28702 28702 27278 pts/1    28702 R+    1000   0:00 ps jT
    ```

11.	В левом окне выполните команду `jobs` для отображения текущих заданий. Завершите процесс `control database` и убедитесь, что он прекращен.

    ```
    [student@servera ~]$ jobs
    [1]+  Stopped                 control technical
    [3]-  Running                 control database &
    ```

    Выполните команду `fg` с идентификатором задания, чтобы перевести процесс control database в активный режим. Нажмите **Ctrl+c**, чтобы завершить процесс. Выполните команду `jobs`, чтобы убедиться, что процесс завершен.

    ```
    [student@servera ~]$ fg %3
    control database
    ^C
    [student@servera ~]$ jobs
    [1]+  Stopped                 control technical
    ```

12.	В командной оболочке правого терминала нажмите **Ctrl+c**, чтобы остановить команду `tail`. С помощью команды `rm` удалите файл **~/control_outfile.**

    ```
    ...output omitted...
    Ctrl+c
    [student@servera ~]$ rm ~/control_outfile
    ```

13.	Выйдите с хоста **servera** на обоих терминалах.

    ```
    [student@servera ~]$ exit
    logout
    Connection to servera closed.
    [student@servera ~]$ exit
    logout
    Connection to servera closed.
    ```

## Конец

На **workstation** запустите сценарий `lab processes-control finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab processes-control finish
```

Упражнение завершено.