## Цели

Проверка базовых навыков архивации и передачи файлов

## Задачи

В этом упражнении вам необходимо:

* синхронизировать удаленный каталог с локальным каталогом;
* создать архив содержимого синхронизированного каталога;
* безопасно копировать архив на удаленный хост;
* извлечь содержимое архива.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab archive-review start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хостов **servera** и **serverb** в сети. Сценарий также проверяет, что файлы и каталоги, которые будут созданы в ходе лабораторной работы, не существуют на **serverb** и **workstation**.

```
[student@workstation ~]$ lab archive-review start
```

1.	На **serverb** синхронизируйте дерево каталогов **/etc** с хоста **servera** в каталог **/configsync**.
2.	Используйте сжатие **gzip**, чтобы создать архив **configfile-backup-servera.tar.gz** с содержимым каталога **/configsync**.
3.	Как пользователь *student* безопасно скопируйте архивный файл **/root/configfile-backup-servera.tar.gz** с хоста **serverb** в каталог **/home/student** на **workstation**. Используйте пароль *student*.
4.	На **workstation** извлеките содержимое архива **/home/student/configfile-backup-servera.tar.gz** в каталог **/tmp/savedconfig/**.
5.	На **workstation** вернитесь в домашний каталог пользователя *student*.

  ```
  [student@workstation savedconfig]$ cd
  ```

## Оценка

На **workstation** запустите сценарий `lab archive-review grade`, чтобы проверить, правильно ли была выполнена лабораторная работа.

```
[student@workstation ~]$ lab archive-review grade
```

## Конец

На **workstation** запустите сценарий `lab archive-review finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab archive-review finish
```

Лабораторная работа завершена.