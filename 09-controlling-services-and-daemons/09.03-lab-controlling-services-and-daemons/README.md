## Цели

Проверка базовых навыков управления службами и демонами

## Задачи

В этой лабораторной работе вы настроите несколько служб на включение или отключение, запуск или остановку на основе предоставленных вам данных.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab services-review start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **serverb** в сети. Сценарий также обеспечивает соответствующую настройку служб **psacct** и **rsyslog** на **serverb**.

```
[student@workstation ~]$ lab services-review start
```

1.	На **serverb** запустите службу **psacct**.
2.	Настройте службу **psacct** на запуск во время начальной загрузки системы.
3.	Остановите службу **rsyslog**.
4.	Настройте службу **rsyslog**, чтобы она не запускалась во время начальной загрузки системы.
5.	Перезагрузите **serverb** перед оценкой лабораторной работы.

## Оценка

На **workstation** запустите сценарий `lab services-review grade`, чтобы проверить, правильно ли была выполнена лабораторная работа.

```
[student@workstation ~]$ lab services-review grade
```

## Конец

На **workstation** запустите сценарий `lab services-review finish`, чтобы закончить лабораторную работу.

```
[student@workstation ~]$ lab services-review finish
```

Лабораторная работа завершена.