## Цели

Получение базовых навыков монтирования и размонтирования файловых систем

## Задачи

В этом упражнении вы смонтируете и размонтируете файловые системы.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab fs-mount start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети. Сценарий также создает раздел на втором диске, подключенном к **servera**.

```
[student@workstation ~]$ lab fs-mount start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	На второй диск (**/dev/vdb**) на **servera** был добавлен новый раздел с файловой системой. Смонтируйте новый раздел по UUID в созданную точку монтирования **/mnt/newspace**.

  <details>
  <summary>Показать решение</summary>


  2.1.	С помощью команды `sudo -i` переключитесь на пользователя *root*, так как только пользователь *root* может смонтировать устройство вручную.

  ```
  [student@servera ~]$ sudo -i
  [sudo] password for student: student
  [root@servera ~]# 
  ```

  2.2.	Создайте каталог **/mnt/newspace**.

  ```
  [root@servera ~]# mkdir /mnt/newspace
  ```

  2.3.	Выполните команду `lsblk` с опцией `-fp`, чтобы узнать UUID устройства **/dev/vdb1**.

  ```
  [root@servera ~]# lsblk -fp /dev/vdb
  NAME        FSTYPE LABEL UUID                                 MOUNTPOINT
  /dev/vdb                                                      
  └─/dev/vdb1 xfs          a04c511a-b805-4ec2-981f-42d190fc9a65
  ```

  2.4.	Смонтируйте файловую систему по UUID в каталог **/mnt/newspace**. Замените указанный UUID идентификатором диска **/dev/vdb1** из вывода предыдущей команды.

  ```
  [root@servera ~]# mount \
  UUID="a04c511a-b805-4ec2-981f-42d190fc9a65" /mnt/newspace
  ```

  2.5.	Убедитесь, что устройство **/dev/vdb1** смонтировано в каталог **/mnt/newspace**.

  ```
  [root@servera ~]# lsblk -fp /dev/vdb
  NAME        FSTYPE LABEL UUID                                 MOUNTPOINT
  /dev/vdb                                                      
  └─/dev/vdb1 xfs          a04c511a-b805-4ec2-981f-42d190fc9a65 /mnt/newspace
  ```
  </details>

3.	Перейдите в каталог **/mnt/newspace** и создайте новый каталог **/mnt/newspace/newdir** с пустым файлом **/mnt/newspace/newdir/newfile**.

  <details>
  <summary>Показать решение</summary>


  3.1.	Перейдите в каталог /mnt/newspace.

  ```
  [root@servera ~]# cd /mnt/newspace
  ```

  3.2.	Создайте новый каталог /mnt/newspace/newdir.

  ```
  [root@servera newspace]# mkdir newdir
  ```

  3.3.	Создайте новый пустой файл /mnt/newspace/newdir/newfile.

  ```
  [root@servera newspace]# touch newdir/newfile
  ```
  </details>

4.	Размонтируйте файловую систему, смонтированную в каталоге **/mnt/newspace**.

  <details>
  <summary>Показать решение</summary>


  4.1.	Выполните команду `umount`, чтобы размонтировать **/mnt/newspace**, пока текущим каталогом в командной оболочке остается **/mnt/newspace**. Команде `umount` не удастся размонтировать устройство.

  ```
  [root@servera newspace]# umount **/mnt/newspace**
  umount: /mnt/newspace: target is busy.
  ```

  4.2.	Измените текущий каталог в командной оболочке на **/root**.

  ```
  [root@servera newspace]# cd
  [root@servera ~]# 
  ```

  4.3.	Теперь успешно размонтируйте **/mnt/newspace**.

  ```
  [root@servera ~]# umount /mnt/newspace
  ```
  </details>

5.	Выйдите с **servera**.

  ```
  [root@servera ~]# exit
  logout
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab fs-mount finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab fs-mount finish
```

Упражнение завершено.