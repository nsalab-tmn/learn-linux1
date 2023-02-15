## Цели

Проверка базовых навыков монтирования файловых систем и поиска файлов

## Задачи

В этой работе вы смонтируете файловую систему и будете искать файлы на основе различных критериев.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab rhcsa-rh124-review5 start`, чтобы начать обзорную работу. Этот сценарий создает файловую систему, учетные записи пользователей и учетные записи групп.

```
[student@workstation ~]$ lab rhcsa-rh124-review5 start
```


Для выполнения этого упражнения необходимо выполнить следующие задачи на **serverb**.

1.	На **serverb** смонтируйте неиспользуемое блочное устройство, содержащее файловую систему XFS, в каталог **/review5-disk**.
2.	На **serverb** найдите файл с именем **review5-path**. Запишите его абсолютный путь в текстовый файл **/review5-disk/review5-path.txt**.
3.	На **serverb** найдите все файлы с пользователем-владельцем *contractor1* и группой-владельцем *contractor*. У этих файлов должны быть восьмеричные разрешения 640. Запишите абсолютные пути ко всем этим файлам в текстовый файл **/review5-disk/review5-perms.txt**.
4.	На **serverb** найдите все файлы размером 100 байтов. Запишите абсолютные пути ко всем этим файлам в файл **/review5-disk/review5-size.txt**.

## Оценка

На **workstation** выполните команду `lab rhcsa-rh124-review5 grade`, чтобы проверить, правильно ли было выполнено упражнение.

```
[student@workstation ~]$ lab rhcsa-rh124-review5 grade
```

## Конец

На **workstation** запустите сценарий `lab rhcsa-rh124-review5 finish`, чтобы завершить обзорную работу. Этот сценарий удаляет файловую систему, учетные записи пользователей и учетные записи групп, созданные в начале обзорной работы, и обеспечивает очистку среды serverb.

```
[student@workstation ~]$ lab rhcsa-rh124-review5 finish
```

Обзорная работа завершена.