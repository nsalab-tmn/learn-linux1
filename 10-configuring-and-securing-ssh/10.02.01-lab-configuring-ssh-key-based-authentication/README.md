## Цели

Получение базовых навыков настройки аутентификации на основе ключей SSH

## Задачи

В этом упражнении вы настроите учетную запись пользователя для использования аутентификации на основе ключей SSH.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab ssh-configure start`, чтобы начать упражнение. Этот сценарий создает необходимые учетные записи пользователей.

```
[student@workstation ~]$ lab ssh-configure start
```

1.	На **workstation** установите SSH-подключение к **serverb** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@serverb
  ...output omitted...
  [student@serverb ~]$ 
  ```

2.	Выполните команду, чтобы переключиться на пользователя *operator1* на **serverb**. Используйте *redhat* в качестве пароля пользователя *operator1*.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@serverb ~]$ su - operator1
  Password: redhat
  [operator1@serverb ~]$ 
  ```
  </details>

3.	Выполните команду `ssh-keygen`, чтобы создать ключи SSH. Не вводите парольную фразу.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ ssh-keygen
  Generating public/private rsa key pair.
  Enter file in which to save the key (/home/operator1/.ssh/id_rsa): Enter
  Created directory '/home/operator1/.ssh'.
  Enter passphrase (empty for no passphrase): Enter
  Enter same passphrase again: Enter
  Your identification has been saved in /home/operator1/.ssh/id_rsa.
  Your public key has been saved in /home/operator1/.ssh/id_rsa.pub.
  The key fingerprint is:
  SHA256:JainiQdnRosC+xXhOqsJQQLzBNUldb+jJbyrCZQBERI operator1@serverb.lab.example.com
  The key's randomart image is:
  +---[RSA 2048]----+
  |E+*+ooo .        |
  |.= o.o o .       |
  |o.. = . . o      |
  |+. + * . o .     |
  |+ = X . S +      |
  | + @ +   = .     |
  |. + =   o        |
  |.o . . . .       |
  |o     o..        |
  +----[SHA256]-----+
  ```
  </details>

4.	Выполните команду `ssh-copy-id`, чтобы отправить открытый ключ из пары ключей SSH пользователю *operator1* на **servera**. Используйте *redhat* в качестве пароля пользователя *operator1* на **servera**.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ ssh-copy-id operator1@servera
  /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/operator1/.ssh/id_rsa.pub"
  The authenticity of host 'servera (172.25.250.10)' can't be established.
  ECDSA key fingerprint is SHA256:ERTdjooOIrIwVSZQnqD5or+JbXfidg0udb3DXBuHWzA.
  Are you sure you want to continue connecting (yes/no)? yes
  /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
  /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
  operator1@servera's password: redhat
  Number of key(s) added: 1

  Now try logging into the machine, with:   "ssh 'operator1@servera'"
  and check to make sure that only the key(s) you wanted were added.
  ```
  </details>

5.	Удаленно выполните команду `hostname` на **servera** с использованием SSH, не обращаясь к удаленной интерактивной оболочке.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ ssh operator1@servera hostname
  servera.lab.example.com
  ```
  </details>

  Обратите внимание, что предыдущая команда `ssh` не запрашивала пароль, поскольку она использовала не защищенный парольной фразой закрытый ключ с экспортированным открытым ключом, чтобы пройти аутентификацию на servera от имени пользователя *operator1*. Этот подход небезопасен, поскольку любой пользователь, имеющий доступ к файлу закрытого ключа, может войти на servera как пользователь *operator1*. Безопасной альтернативой является защита закрытого ключа парольной фразой. Это мы сделаем на следующем шаге.

  Выполните команду `ssh-keygen`, чтобы создать еще один набор ключей SSH, защищенных парольной фразой. Сохраните ключ как **/home/operator1/.ssh/key2**. Используйте *redhatpass* в качестве парольной фразы для закрытого ключа.

  <details>
  <summary>Предупреждение</summary>

  Если не указать файл, в котором должен быть сохранен ключ, будет использоваться файл по умолчанию (**/home/user/.ssh/id_rsa**). Вы уже использовали имя файла по умолчанию при создании ключей SSH на предыдущем шаге, поэтому важно указать нестандартный файл, в противном случае существующие ключи SSH будут перезаписаны.
  </details>

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ ssh-keygen -f .ssh/key2
  Generating public/private rsa key pair.
  Enter passphrase (empty for no passphrase): redhatpass
  Enter same passphrase again: redhatpass
  Your identification has been saved in .ssh/key2.
  Your public key has been saved in .ssh/key2.pub.
  The key fingerprint is:
  SHA256:OCtCjfPm5QrbPBgqbEIWCcw5AI4oSlMEbgLrBQ1HWKI operator1@serverb.lab.example.com
  The key's randomart image is:
  +---[RSA 2048]----+
  |O=X*             |
  |OB=.             |
  |E*o.             |
  |Booo   .         |
  |..= . o S        |
  | +.o   o         |
  |+.oo+ o          |
  |+o.O.+           |
  |+ . =o.          |
  +----[SHA256]-----+
  ```
  </details>

6.	Выполните команду `ssh-copy-id`, чтобы отправить открытый ключ из пары ключей, защищенной парольной фразой, пользователю *operator1* на **servera**.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ ssh-copy-id -i .ssh/key2.pub operator1@servera
  /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: ".ssh/key2.pub"
  /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
  /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys

  Number of key(s) added: 1

  Now try logging into the machine, with:   "ssh 'operator1@servera'"
  and check to make sure that only the key(s) you wanted were added.
  ```
  </details>

  Обратите внимание, что предыдущая команда `ssh-copy-id` не запрашивала пароль, поскольку она использовала открытый ключ для закрытого ключа, не защищенного парольной фразой, который вы экспортировали на **servera** на предыдущем шаге.

7.	Удаленно выполните команду `hostname` на **servera** с использованием SSH, не обращаясь к удаленной интерактивной оболочке. Используйте **/home/operator1/.ssh/key2** как файл идентификационных данных. Укажите *redhatpass* в качестве парольной фразы, которую вы настроили для закрытого ключа на предыдущем шаге.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ ssh -i .ssh/key2 operator1@servera hostname
  Enter passphrase for key '.ssh/key2': redhatpass
  servera.lab.example.com
  ```
  </details>

  Обратите внимание, что предыдущая команда `ssh` запросила у вас парольную фразу, которую вы использовали для защиты закрытого ключа из пары ключей SSH. Эта парольная фраза защищает закрытый ключ. Если злоумышленник получит доступ к закрытому ключу, он не сможет использовать его для доступа к другим системам, поскольку сам закрытый ключ защищен парольной фразой. Команда `ssh` использует парольную фразу, отличную от парольной фразы пользователя *operator1* на **servera** , и пользователям необходимо знать обе.

  Используйте команду `ssh-agent`, как на следующем шаге, чтобы избежать интерактивного ввода парольной фразы при входе в систему через SSH. Использование `ssh-agent` удобнее и безопаснее в ситуациях, когда администраторы регулярно входят в удаленные системы.

8.	Выполните команду `ssh-agent` в своей оболочке Bash и добавьте защищенный парольной фразой закрытый ключ (**/home/operator1/.ssh/key2**) из пары ключей SSH в сеанс оболочки.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ eval $(ssh-agent)
  Agent pid 21032
  [operator1@serverb ~]$ ssh-add .ssh/key2
  Enter passphrase for .ssh/key2: redhatpass
  Identity added: .ssh/key2 (operator1@serverb.lab.example.com)
  ```
  </details>

  Предыдущая команда `eval` запустила программу `ssh-agent` и настроила этот сеанс оболочки на ее использование. Затем вы использовали команду `ssh-add`, чтобы предоставить программе `ssh-agent` разблокированный закрытый ключ.

9.	Удаленно выполните команду `hostname` на **servera**, не обращаясь к удаленной интерактивной оболочке. Используйте **/home/operator1/.ssh/key2** как файл идентификационных данных.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator1@serverb ~]$ ssh -i .ssh/key2 operator1@servera hostname
  servera.lab.example.com
  ```
  </details>

  Обратите внимание, что предыдущая команда `ssh` не предложила ввести парольную фразу в интерактивном режиме.

10.	Откройте еще один терминал на **workstation** и установите SSH-подключение к **serverb** как пользователь *student*.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@workstation ~]$ ssh student@serverb
  ...output omitted...
  [student@serverb ~]$ 
  ```
  </details>

11.	На **serverb** переключитесь на пользователя *operator1* и установите SSH-подключение к **servera**. Используйте **/home/operator1/.ssh/key2** в качестве файла идентификационных данных для прохождения аутентификации с использованием ключей SSH.

  <details>
  <summary>Показать решение</summary>

  11.1.	Выполните команду `su`, чтобы переключиться на пользователя *operator1*. Используйте *redhat* в качестве пароля пользователя *operator1*.

  ```
  [student@serverb ~]$ su - operator1
  Password: redhat
  [operator1@serverb ~]$ 
  ```

  11.2.	Установите SSH-подключение к **servera** как пользователь *operator1*.

  ```
  [operator1@serverb ~]$ ssh -i .ssh/key2 operator1@servera
  Enter passphrase for key '.ssh/key2': redhatpass
  ...output omitted...
  [operator1@servera ~]$ 
  ```

  Обратите внимание, что предыдущая команда `ssh` запросила интерактивный ввод парольной фразы, поскольку вы попытались установить SSH-подключение не из оболочки, которую использовали для запуска `ssh-agent`.
  </details>

12.	Выйдите из всех оболочек, используемых на втором терминале.

  12.1.	Выйдите с **servera**.

  ```
  [operator1@servera ~]$ exit
  logout
  Connection to servera closed.
  [operator1@serverb ~]$ 
  ```

  12.2.	Выйдите из оболочек пользователей *operator1* и *student* на **serverb**, чтобы вернуться в оболочку пользователя *student* на **workstation**.

  ```
  [operator1@serverb ~]$ exit
  logout
  [student@serverb ~]$ exit
  logout
  Connection to serverb closed.
  [student@workstation ~]$ 
  ```

  12.3.	Закройте второй терминал на **workstation**.

  ```
  [student@workstation ~]$ exit
  ```

13.	Выйдите с **serverb** на первом терминале и завершите это упражнение.

  13.1.	На первом терминале выйдите из командной оболочки пользователя *operator1* на **serverb**.

  ```
  [operator1@serverb ~]$ exit
  logout
  [student@serverb ~]$ 
  ```

  Выполнение команды `exit` привело к выходу из командной оболочки пользователя *operator1*. Сеанс оболочки с активной программой `ssh-agent` был завершен, и вы вернулись в оболочку пользователя *student* на **serverb**.

  13.2.	Выйдите из командной оболочки пользователя *student* на **serverb**, чтобы вернуться в оболочку пользователя *student* на **workstation**.

  ```
  [student@serverb ~]$ exit
  logout
  Connection to serverb closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab ssh-configure finish,` чтобы закончить упражнение.

```
[student@workstation ~]$ lab ssh-configure finish
```

Упражнение завершено.

