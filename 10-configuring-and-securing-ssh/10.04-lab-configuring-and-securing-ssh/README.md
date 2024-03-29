## Цели

Проверка базовых навыков по настройке и защите SSH

## Задачи

В этой лабораторной работе вы настроите аутентификацию на основе ключей для пользователей, а также отключите прямой вход в систему от имени пользователя *root* и аутентификацию на основе пароля для службы OpenSSH на одном из ваших серверов.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab ssh-review start`, чтобы начать упражнение. Этот сценарий создает необходимые учетные записи пользователей и файлы.

```
[student@workstation ~]$ lab ssh-review start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.
2.	Выполните команду `su`, чтобы переключиться на пользователя *production1* на **servera**.
3.	Выполните команду `ssh-keygen`, чтобы создать ключи SSH, не защищенные парольной фразой, для пользователя *production1* на **servera**.
4.	Выполните команду `ssh-copy-id`, чтобы отправить открытый ключ из пары ключей SSH пользователю *production1* на **serverb**.
5.	Убедитесь, что пользователь *production1* может войти на **serverb**, используя ключи SSH.
6.	Настройте **sshd** на **serverb**, чтобы пользователи не могли входить в систему от имени пользователя *root*. Используйте *redhat* в качестве пароля привилегированного пользователя.
7.	Настройте **sshd** на **serverb**, чтобы разрешить пользователям проходить аутентификацию только с помощью ключей SSH, но не паролей.

## Оценка

На **workstation** выполните команду `lab ssh-review grade`, чтобы проверить, правильно ли было выполнено упражнение.

```
[student@workstation ~]$ lab ssh-review grade
```

## Конец

На **workstation** запустите сценарий `lab ssh-review finish`, чтобы закончить лабораторную работу.

```
[student@workstation ~]$ lab ssh-review finish
```

Лабораторная работа завершена.


