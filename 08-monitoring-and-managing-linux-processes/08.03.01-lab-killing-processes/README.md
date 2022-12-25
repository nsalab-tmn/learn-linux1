## Цели

Вы сможете запустить и остановить несколько процессов командной оболочки.

## Задачи

В этом упражнении вы будете использовать сигналы для управления процессами и их остановки.

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду ё. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети.

```
[student@workstation ~]$ lab processes-kill start
```

1.	На **workstation** откройте рядом два окна терминала. В этом разделе эти терминалы называются левым и правым. На каждом терминале с помощью команды `ssh` войдите на хост **servera** как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	В левом окне создайте новый каталог с именем **/home/student/bin**. В новом каталоге создайте сценарий оболочки с именем **killing**. Сделайте сценарий исполняемым.

    2.1.	С помощью команды `mkdir` создайте новый каталог с именем **/home/student/bin**.

    ```
    [student@servera ~]$ mkdir /home/student/bin
    ```

    2.2.	С помощью команды `vim` создайте сценарий с именем **killing** в каталоге **/home/student/bin**. Нажмите клавишу `i`, чтобы перейти в интерактивный режим Vim. Выполните команду `:wq`, чтобы сохранить файл.

    ```
    [student@servera ~]$ vim /home/student/bin/killing
    #!/bin/bash
    while true; do
    echo -n "$@ " >> ~/killing_outfile
    sleep 5
    done
    ```

    <details>
    <summary>Примечание</summary>

    Сценарий `killing` работает до тех пор, пока не будет завершен. Он добавляет аргументы командной строки в файл **~/killing_outfile** один раз в 5 секунд.
    </details>

    2.3.	Используйте команду `chmod`, чтобы сделать файл **killing** исполняемым.

    ```
    [student@servera ~]$ chmod +x /home/student/bin/killing
    ```

3.	В командной оболочке левого терминала выполните команду `cd`, чтобы перейти в каталог **/home/student/bin/**. Запустите три процесса **killing** с аргументами **network**, **interface** и **connection** соответственно. Запустите три процесса: **network**, **interface** и **connection**. Используйте амперсанд (**&**), чтобы запустить процессы в фоновом режиме.

    ```
    [student@servera ~]$ cd /home/student/bin
    [student@servera bin]$ killing network &
    [1] 3460
    [student@servera bin]$ killing interface &
    [2] 3482
    [student@servera bin]$ killing connection & 
    [3] 3516
    ```

    У ваших процессов будут другие номера PID.

4.	В командной оболочке правого терминала выполните команду `tail` с опцией `-f`, чтобы убедиться, что все три процесса добавляют вывод в файл **/home/student/killing_outfile**.

```
[student@servera ~]$ tail -f ~/killing_outfile
network interface network connection interface network connection interface network
...output omitted...
```

5.	В командной оболочке левого терминала выполните команду `jobs` для отображения списка заданий.

```
[student@servera bin]$ jobs
[1]   Running                 killing network &
[2]-  Running                 killing interface &
[3]+  Running                 killing connection & 
```

6.	Используйте сигналы, чтобы приостановить процесс **network**. Убедитесь, что процесс network остановлен (**Stopped**). В командной оболочке правого терминала убедитесь, что процесс **network** больше не добавляет вывод в файл **~/killing_output**.

    6.1.	Выполните команду `kill` с опцией `-SIGSTOP`, чтобы остановить процесс **network**. Выполните команду `jobs`, чтобы убедиться, что процесс остановлен.

    ```
    [student@servera bin]$ kill -SIGSTOP %1
    [1]+  Stopped                 killing network
    [student@servera bin]$ jobs
    [1]+  Stopped                 killing network
    [2]   Running                 killing interface &
    [3]-  Running                 killing connection & 
    ```

    6.2.	В командной оболочке правого терминала просмотрите вывод команды `tail`. Убедитесь, что слово **network** больше не добавляется в файл **~/killing_outfile**.

    ```
    ...output omitted...
    interface connection interface connection interface connection interface 
    ```

7.	В командной оболочке левого терминала завершите процесс **interface** с помощью сигналов. Убедитесь, что процесс **interface** больше не отображается. В командной оболочке правого терминала убедитесь, что процесс **interface** больше не добавляет вывод в файл **~/killing_outfile**.

    7.1.	Выполните команду `kill` с опцией `-SIGTERM`, чтобы завершить процесс **interface**. Выполните команду `jobs`, чтобы убедиться, что процесс был завершен.

    ```
    [student@servera bin]$ kill -SIGTERM %2
    [student@servera bin]$ jobs
    [1]+  Stopped                 killing network
    [2]   Terminated              killing interface
    [3]-  Running                 killing connection & 
    ```

    7.2.	В командной оболочке правого терминала просмотрите вывод команды `tail`. Убедитесь, что слово **interface** больше не добавляется в файл **~/killing_outfile**.

    ```
    ...output omitted...
    connection connection connection connection connection connection connection connection 
    ```

8.	В командной оболочке левого терминала возобновите процесс **network** с помощью сигналов. Убедитесь, что процесс **network** выполняется (**Running**). В правом окне убедитесь, что вывод процесса **network** добавляется в файл **~/killing_outfile**.

    8.1.	Выполните команду `kill` с опцией `-SIGCONT`, чтобы возобновить процесс **network**. Выполните команду `jobs`, чтобы убедиться, что процесс выполняется (**Running**).

    ```
    [student@servera bin]$ kill -SIGCONT %1
    [student@servera bin]$ jobs
    [1]+  Running                 killing network &
    [3]-  Running                 killing connection & 
    ```

    8.2.	В командной оболочке правого терминала просмотрите вывод команды `tail`. Убедитесь, что слово **network** добавляется в файл **~/killing_outfile**.

    ```
    ...output omitted...
    network connection network connection network connection network connection network connection 
    ```

9.	В командной оболочке левого терминала завершите оставшиеся два задания. Убедитесь, что больше заданий нет и вывод данных прекратился.

    9.1.	Выполните команду `kill` с опцией `-SIGTERM`, чтобы завершить процесс **network**. Используйте эту же команду для завершения процесса **connection**.

    ```
    [student@servera bin]$ kill -SIGTERM %1
    [student@servera bin]$ kill -SIGTERM %3
    [1]+  Terminated              killing network
    [student@servera bin]$ jobs
    [3]+  Terminated              killing connection 
    ```

10.	В командной оболочке левого терминала отобразите процессы `tail`, запущенные во всех открытых терминальных оболочках. Завершите запущенные процессы `tail`. Убедитесь, что процесс больше не выполняется.

    10.1.	Выполните команду `ps` с опцией `-ef`, чтобы отобразить список всех запущенных процессов `tail`. Уточните поиск с помощью команды `grep`.

    ```
    [student@servera bin]$ ps -ef | grep tail
    student   4581 31358  0 10:02 pts/0    00:00:00 tail -f killing_outfile
    student   4869  2252  0 10:33 pts/1    00:00:00 grep --color=auto tail
    ```

    10.2.	Выполните команду `pkill` с опцией `-SIGTERM`, чтобы завершить процесс `tail`. Выполните команду `ps`, чтобы убедиться, что процесс больше не отображается.

    ```
    [student@servera bin]$ pkill -SIGTERM tail
    [student@servera bin]$ ps -ef | grep tail
    student   4874  2252  0 10:36 pts/1    00:00:00 grep --color=auto tail 
    ```

    10.3.	В командной оболочке правого терминала убедитесь, что команда `tail` больше не выполняется.

    ```
    ...output omitted...
    network connection network connection network connection Terminated
    [student@servera ~]$ 
    ```

11.	Выйдите из окон обоих терминалов. Если не выйти из всех сеансов, финальный сценарий завершится ошибкой.

```
[student@servera bin]$ exit
logout
Connection to servera closed.
[student@workstation ~]$ 
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$ 
```

## Конец

На **workstation** запустите сценарий `lab processes-kill finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab processes-kill finish
```

Упражнение завершено.
