## Цели 

После завершения этого раздела вы сможете войти в систему Linux и выполнять простые команды с помощью оболочки. 

## Введение в оболочку bash 

**Командная строка** — это текстовый интерфейс, который можно использовать для ввода инструкций в компьютерную систему. Командная строка Linux предоставляется программой, называемой **Shell**. За прошедшие годы были разработаны различные варианты программы-оболочки, и разные пользователи могут быть настроены на использование разных оболочек. Однако большинство пользователей придерживаются текущего значения по умолчанию. 

Оболочкой по умолчанию для пользователей большинства дистрибутивов Linux является GNU Bourne-Again Shell (bash). **Bash** — это улучшенная версия одной из самых успешных оболочек, используемых в UNIX-подобных системах, Bourne Shell (**sh**).

Когда оболочка используется в интерактивном режиме, она отображает строку, когда ожидает команду от пользователя. Это называется приглашением оболочки. Когда обычный пользователь запускает оболочку, приглашение по умолчанию заканчивается символом **$**, как показано ниже.

```shell
[user@host ~]$
```

Символ **$** заменяется символом **#**, если оболочка работает от имени суперпользователя *root*. Это делает более очевидным, что это оболочка суперпользователя, которая помогает избежать аварий и ошибок, которые могут повлиять на всю систему. Приглашение оболочки суперпользователя показано ниже.

```shell
[root@host ~]# 
```

Использование bash для выполнения команд даёт серьезные преимущества. Оболочка bash предоставляет язык сценариев, поддерживающий автоматизацию задач. Оболочка имеет дополнительные возможности, которые могут упростить или сделать возможными операции, которые трудно эффективно выполнить с помощью графических инструментов.

<details>
<summary>Примечание</summary>

Оболочка bash по своей концепции аналогична интерпретатору командной строки, используемому в Microsoft Windows (cmd.exe), хотя в bash используется более сложный язык сценариев. Администраторам Apple Mac, использующим утилиту Terminal, может быть приятно отметить, что bash является оболочкой по умолчанию в macOS.
</details>

## Основы shell

Команды, вводимые в командной строке, состоят из трех основных частей:

- *Команда* для запуска 
- *Опции* для настройки поведения команды 
- *Аргументы*, которые обычно являются целью команды 

*Команда* — это имя запускаемой программы. За ним может следовать одна или несколько опций, которые регулируют поведение команды или то, что она будет делать. Опции обычно начинаются с одного или двух дефисов (например, `-a` или `--all`), чтобы отличить их от аргументов. Команды также могут сопровождаться одним или несколькими аргументами, которые часто указывают цель, с которой должна работать команда.

Например, команда `usermod -L user01` содержит команду (`usermod`), опцию (`-L`) и аргумент (`user01`). Результатом этой команды является блокировка пароля учетной записи пользователя user01.

### Вход на локальный компьютер

Чтобы запустить оболочку, вам нужно войти в компьютер через терминал. Терминал — это текстовый интерфейс, используемый для ввода команд и вывода результатов из компьютерной системы. Есть несколько способов сделать это.

Компьютер может иметь аппаратную клавиатуру и дисплей для ввода и вывода, напрямую подключенные к нему. Это физическая консоль Linux-машины. Физическая консоль поддерживает несколько виртуальных консолей, на которых могут работать отдельные терминалы. Каждая виртуальная консоль поддерживает независимый сеанс входа в систему. Вы можете переключаться между ними, одновременно нажимая **Ctrl+Alt** и функциональную клавишу (от **F1** до **F6**). Большинство этих виртуальных консолей запускают терминал, предоставляющий текстовое приглашение для входа в систему, и если вы правильно введете свое имя пользователя и пароль, вы войдете в систему и получите приглашение оболочки.

Компьютер может отображать графическое приглашение для входа в систему на одной из виртуальных консолей. Вы можете использовать это для входа в графическую среду. Графическая среда также работает на виртуальной консоли. Чтобы получить приглашение оболочки, вы должны запустить терминальную программу в графической среде. Приглашение оболочки предоставляется в окне приложения вашей программы графического терминала.

<details>
<summary>Примечание</summary>

Многие системные администраторы предпочитают не запускать графическую среду на своих серверах. Это позволяет вместо этого использовать ресурсы, которые использовались бы графической средой, службами сервера.
</details>

В Red Hat Enterprise Linux 8, если доступна графическая среда, экран входа будет запущен на первой виртуальной консоли с именем **tty1**. На виртуальных консолях со второй по шестую доступно пять дополнительных текстовых приглашений для входа. 

Если вы входите в систему с помощью графического экрана входа, ваша графическая среда запускается на первой виртуальной консоли, которая в данный момент не используется сеансом входа. Обычно ваш графический сеанс заменяет запрос на вход во вторую виртуальную консоль (**tty2**). Однако, если эта консоль используется активным текстовым сеансом входа в систему (а не только запросом на вход), вместо нее используется следующая свободная виртуальная консоль. 

Графический экран входа в систему продолжает работать на первой виртуальной консоли (**tty1**). Если вы уже вошли в графический сеанс и вошли в систему как другой пользователь на графическом экране входа в систему или используете пункт меню «Переключить пользователя» для переключения пользователей в графической среде без выхода из системы, для этого пользователя будет запущена другая графическая среда следующая бесплатная виртуальная консоль. 

Когда вы выходите из графической среды, она закрывается, и физическая консоль автоматически переключается обратно на графический экран входа в систему на первой виртуальной консоли.

<details>
<summary>Примечание</summary>

В Red Hat Enterprise Linux 6 и 7 графический экран входа запускается на первой виртуальной консоли, но когда вы входите в исходную графическую среду, он заменяет экран входа на первой виртуальной консоли, а не запускается на новой виртуальной консоли.

В Red Hat Enterprise Linux 5 и более ранних версиях первые шесть виртуальных консолей всегда предоставляли текстовые приглашения для входа в систему. Если графическое окружение запущено, оно находится на виртуальной консоли номер семь (доступ через **Ctrl+Alt+F7**).
</details>

*Headless-сервер* не имеет постоянно подключенной к нему клавиатуры и дисплея. Центр обработки данных может быть заполнен множеством стоек headless-серверов, и отказ от предоставления каждой из них клавиатуры и дисплея экономит место и расходы. Чтобы администраторы могли войти в систему, headless-сервер может иметь приглашение для входа в систему, предоставляемое его *консольным интерфейсом*, работающей на последовательном (serial) порту, который подключен к сетевому консольному серверу для удаленного доступа к консоли.

Последовательная консоль обычно используется для исправления сервера, если его собственная сетевая карта неправильно настроена и вход в систему через собственное сетевое соединение становится невозможным. Однако в большинстве случаев доступ к headless-серверам осуществляется другими средствами по сети.

### Вход по сети

Пользователям и администраторам Linux часто требуется получить доступ к удаленной системе через оболочку, подключившись к ней по сети. В современной вычислительной среде многие headless-серверы на самом деле являются виртуальными машинами или работают как общедоступные или частные облачные экземпляры. Эти системы не являются физическими и не имеют реальных аппаратных консолей. Они могут даже не предоставлять доступ к своей (имитируемой) физической консоли или последовательной консоли. 

В Linux наиболее распространенный способ получить приглашение оболочки в удаленной системе — использовать Secure Shell (**SSH**). Большинство систем Linux и macOS предоставляют для этой цели программу командной строки - OpenSSH `ssh`. 

В этом примере пользователь *user* с приглашением оболочки на хосте компьютера использует `ssh` для входа на удаленный хост системы Linux в качестве пользователя *remoteuser*:

```shell
[user@host ~]$ ssh remoteuser@remotehost 
remoteuser@remotehost's password: password 
[remoteuser@remotehost ~]$ 
```

Команда `ssh` шифрует соединение, чтобы защитить его от перехвата паролей и содержимого. 

Некоторые системы (например, новые облачные экземпляры) не позволяют пользователям использовать пароль для входа в систему с помощью ssh для большей безопасности. Альтернативным способом аутентификации на удаленном компьютере без ввода пароля является аутентификация с открытым ключом. 

При использовании этого метода аутентификации у пользователей есть специальный файл идентификации, содержащий закрытый ключ, эквивалентный паролю, который они держат в секрете. Их учетная запись на сервере настроена с использованием соответствующего открытого ключа, который не обязательно должен быть секретным. При входе в систему пользователи могут настроить ssh для предоставления закрытого ключа, и если их соответствующий открытый ключ установлен в этой учетной записи на этом удаленном сервере, он будет входить в систему без запроса пароля. 

В следующем примере пользователь user с приглашением оболочки на хост-компьютере входит в удаленный хост как remoteuser, используя ssh, используя аутентификацию с открытым ключом. Опция -i используется для указания файла закрытого ключа пользователя, которым является mylab.pem. Соответствующий открытый ключ уже настроен как авторизованный ключ в учетной записи remoteuser. 

```shell
[user@host ~]$ ssh -i mylab.pem remoteuser@remotehost
[remoteuser@remotehost ~]$ 
```

Чтобы это работало, файл закрытого ключа должен быть доступен для чтения только пользователю, которому принадлежит файл. В предыдущем примере, где закрытый ключ находится в файле *mylab.pem*, для этого можно использовать команду `chmod 600 mylab.pem`. Как установить права доступа к файлам более подробно обсуждается в следующей главе. 

У пользователей также могут быть настроены закрытые ключи, которые пробуются автоматически, но это обсуждение выходит за рамки данного раздела.

<details>
<summary>Примечание</summary>

При первом входе на новую машину вам будет выдано предупреждение от ssh о том, что она не может установить подлинность хоста: 

```shell
[user@host ~]$ ssh -i mylab.pem remoteuser@remotehost 
The authenticity of host 'remotehost (192.0.2.42)' can't be established. 
ECDSA key fingerprint is 47:bf:82:cd:fa:68:06:ee:d8:83:03:1a:bb:29:14:a3. 
Are you sure you want to continue connecting (yes/no)? yes 
[remoteuser@remotehost ~]$ 
```

Каждый раз, когда вы подключаетесь к удаленному хосту с помощью ssh, удаленный хост отправляет ssh свой ключ хоста для аутентификации и помощи в настройке зашифрованной связи. Команда ssh сравнивает это со списком сохраненных ключей хоста, чтобы убедиться, что он не изменился. Если ключ хоста изменился, это может указывать на то, что кто-то пытается притвориться этим хостом, чтобы перехватить соединение, что также известно как атака «посредник». В SSH ключи хоста защищают от атак «посредник», эти ключи хоста уникальны для каждого сервера, и их необходимо периодически менять при подозрении на компрометацию. 

Вы получите это предупреждение, если на вашем локальном компьютере не сохранен ключ хоста для удаленного хоста. Если вы введете «да», ключ хоста, отправленный удаленным хостом, будет принят и сохранен для дальнейшего использования. Вход в систему продолжится, и вы больше не увидите это сообщение при подключении к этому хосту. Если вы введете «нет», ключ хоста будет отклонен, а соединение закрыто. 
Если на локальном компьютере сохранен ключ хоста, и он не совпадает с фактически отправленным удаленным хостом, соединение будет автоматически закрыто с предупреждением/
</details>

### Выход

Когда вы закончите использовать оболочку и захотите выйти, вы можете выбрать один из нескольких способов завершения сеанса. Вы можете ввести команду `exit`, чтобы завершить текущий сеанс оболочки. Либо завершите сеанс, нажав **Ctrl+D**.

Ниже приведен пример выхода пользователя из сеанса SSH: 

```shell
[remoteuser@remotehost ~]$ exit
logout
Connection to remotehost closed.
[user@host ~]$
```
