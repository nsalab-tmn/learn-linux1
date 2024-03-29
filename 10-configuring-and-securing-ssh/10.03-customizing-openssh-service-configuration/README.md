## Цели

После завершения этого раздела вы сможете ограничить прямой вход в систему от имени пользователя root и отключить аутентификацию на основе пароля для службы OpenSSH.

## Настройка сервера OpenSSH

Служба OpenSSH предоставляется демоном **sshd**. Его основной файл конфигурации — **/etc/ssh/sshd_config**.

Стандартная конфигурация сервера OpenSSH работает хорошо. Однако вы можете внести некоторые изменения для повышения безопасности системы. Например, можно запретить удаленный вход напрямую в учетную запись *root*, а также запретить аутентификацию на основе пароля (заменив ее аутентификацией на основе закрытого ключа SSH).

## Запрет входа в систему с помощью SSH для привилегированного пользователя

Рекомендуется запретить вход напрямую в учетную запись пользователя *root* из удаленных систем. Ниже описаны некоторые риски, связанные со входом в систему напрямую от имени пользователя *root*:

* Имя пользователя *root* существует в системах Linux по умолчанию, поэтому потенциальному злоумышленнику достаточно подобрать только пароль, а не комбинацию имени пользователя и пароля. Это значительно упрощает задачу для злоумышленника.
* Пользователь *root* имеет неограниченные права, поэтому компрометация учетной записи может нанести максимальный урон системе.
* С точки зрения аудита может быть трудно отследить, какой авторизованный пользователь вошел в систему как *root* и внес изменения. Если пользователь входит в систему от имени обычного пользователя и переключается на учетную запись *root*, в log-файле регистрируется соответствующее событие, которое можно использовать для отчетности.

Сервер OpenSSH использует параметр **PermitRootLogin** в файле конфигурации **/etc/ssh/sshd_config**, чтобы разрешить или запретить пользователям входить в систему от имени пользователя *root*.

```
PermitRootLogin yes
```

Если для параметра **PermitRootLogin** установлено значение **yes** (по умолчанию), пользователи могут входить в систему как *root*. Чтобы этого не происходило, задайте значение **no**. Чтобы запретить аутентификацию на основе пароля, но разрешить аутентификацию на основе ключей для пользователя *root*, задайте для параметра **PermitRootLogin** значение **without-password**.

Чтобы изменения вступили в силу, необходимо перезагрузить SSH-сервер (sshd).

```bash
[root@host ~]# systemctl reload sshd
```

## Запрет аутентификации на основе пароля через SSH

Вход в удаленную командную строку только с помощью закрытых ключей предоставляет ряд преимуществ:

* Злоумышленники не могут использовать атаки с подбором паролей для удаленного взлома известных учетных записей в системе.
* При использовании закрытых ключей, защищенных парольными фразами, злоумышленнику требуются парольная фраза и копия закрытого ключа. При использовании паролей злоумышленнику нужен только пароль.
* Когда закрытые ключи, защищенные парольной фразой, используются вместе с программой `ssh-agent`, парольная фраза раскрывается реже, поскольку ее реже вводят, а вход в систему становится удобнее для пользователя.
  
Сервер OpenSSH использует параметр **PasswordAuthentication** в файле конфигурации **/etc/ssh/sshd_config**, чтобы разрешить или запретить пользователям применять аутентификацию на основе пароля при входе в систему.

```
PasswordAuthentication yes
```

Значение по умолчанию для параметра **PasswordAuthentication** в файле конфигурации **/etc/ssh/sshd_config** ― **yes**. В результате SSH-сервер разрешает пользователям выполнять аутентификацию на основе пароля при входе в систему. Значение **no** для **PasswordAuthentication** запрещает пользователям использовать аутентификацию на основе пароля.

Помните, что после каждого изменения файла **/etc/ssh/sshd_config** необходимо перезагрузить службу **sshd**, чтобы изменения вступили в силу.

<details>
<summary>Важно</summary>

Если вы выключите аутентификацию на основе пароля для `ssh`, вам потребуется обеспечить наличие открытого ключа в файле **~/.ssh/authorized_keys** пользователя на удаленном сервере, чтобы пользователь мог войти в систему.
</details>
