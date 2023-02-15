## Цели

Проверка базовых навыков управления файлами из командной строки

## Задачи

В этой работе вы будете управлять файлами, перемещать набор строк из текстового файла в другой файл и редактировать текстовые файлы.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На workstation запустите сценарий `lab rhcsa-rh124-review1 start`, чтобы начать обзорную работу. Этот сценарий создает необходимые файлы для настройки среды.

```
[student@workstation ~]$ lab rhcsa-rh124-review1 start
```


Для выполнения этого упражнения необходимо выполнить следующие задачи на serverb:

1.	Создайте новый каталог с именем **/home/student/grading**.
2.	Создайте в каталоге **/home/student/grading** три пустых файла: **grade1**, **grade2** и **grade3**.
3.	Запишите первые пять строк файла **/home/student/bin/manage-files** в файл **/home/student/grading/manage-files.txt**.
4.	Добавьте последние три строки файла **/home/student/bin/manage-files** в файл **/home/student/grading/manage-files.txt**. Не перезаписывайте текст, который уже присутствует в файле **/home/student/grading/manage-files.txt**.
5.	Скопируйте файл **/home/student/grading/manage-files.txt** в **/home/student/grading/manage-files-copy.txt**.
6.	Отредактируйте файл **/home/student/grading/manage-files-copy.txt**, чтобы в нем шли подряд две строки с текстом `Test JJ`.
7.	Отредактируйте файл **/home/student/grading/manage-files-copy.txt**, чтобы строки с текстом `Test HH` не было в файле.
8.	Отредактируйте файл **/home/student/grading/manage-files-copy.txt**, чтобы строка `A new line` находилась между строкой `Test BB` и строкой `Test CC`.
9.	Создайте жесткую ссылку с именем **/home/student/hardlink** на файл **/home/student/grading/grade1**.
10.	Создайте символьную ссылку с именем **/home/student/softlink** на файл **/home/student/grading/grade2**.
11.	Сохраните вывод команды, которая отображает содержимое каталога **/boot**, в файл **/home/student/grading/longlisting.txt**. Вывод должен быть «длинным списком» и включать такие сведения, как файловые разрешения, пользователь-владелец и группа-владелец, размер и дата изменения каждого файла.

## Оценка

На **workstation** выполните команду `lab rhcsa-rh124-review1 grade`, чтобы проверить, правильно ли было выполнено упражнение.

```
[student@workstation ~]$ lab rhcsa-rh124-review1 grade
```

## Конец

На **workstation** запустите сценарий `lab rhcsa-rh124-review1 finish`, чтобы завершить обзорную работу. Этот сценарий удаляет файлы и каталоги, созданные в начале обзорной работы, и обеспечивает очистку среды **serverb**.

```
[student@workstation ~]$ lab rhcsa-rh124-review1 finish
```

Обзорная работа завершена.