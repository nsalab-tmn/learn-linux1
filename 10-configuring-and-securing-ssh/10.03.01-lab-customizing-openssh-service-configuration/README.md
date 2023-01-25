## Цели

Получение базовых навыков изменения конфигурации службы OpenSSH

## Задачи

В этом упражнении вы отключите прямой вход в систему от имени пользователя root, а также аутентификацию на основе пароля для службы OpenSSH на одном из ваших серверов.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab ssh-customize start`, чтобы начать упражнение. Этот сценарий создает необходимые учетные записи пользователей и файлы.

```
[student@workstation ~]$ lab ssh-customize start
```

1.	На **workstation** установите SSH-подключение к **serverb** как пользователь *student*.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@workstation ~]$ ssh student@serverb
  ...output omitted...
  [student@serverb ~]$ 
  ```
  </details>

2.	Выполните команду, чтобы переключиться на пользователя *operator2* на **serverb**. Используйте *redhat* в качестве пароля пользователя *operator2*.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@serverb ~]$ su - operator2
  Password: redhat
  [operator2@serverb ~]$ 
  ```
  </details>

3.	Выполните команду `ssh-keygen`, чтобы создать ключи SSH. Не вводите парольную фразу для ключей.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator2@serverb ~]$ ssh-keygen
  Generating public/private rsa key pair.
  Enter file in which to save the key (/home/operator2/.ssh/id_rsa): Enter
  Created directory '/home/operator2/.ssh'.
  Enter passphrase (empty for no passphrase): Enter
  Enter same passphrase again: Enter
  Your identification has been saved in /home/operator2/.ssh/id_rsa.
  Your public key has been saved in /home/operator2/.ssh/id_rsa.pub.
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

4.	Выполните команду `ssh-copy-id`, чтобы отправить открытый ключ из пары ключей SSH пользователю *operator2* на **servera**. Используйте *redhat* в качестве пароля пользователя *operator2* на **servera**.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator2@serverb ~]$ ssh-copy-id operator2@servera
  /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/operator1/.ssh/id_rsa.pub"
  The authenticity of host 'servera (172.25.250.10)' can't be established.
  ECDSA key fingerprint is SHA256:ERTdjooOIrIwVSZQnqD5or+JbXfidg0udb3DXBuHWzA.
  Are you sure you want to continue connecting (yes/no)? yes
  /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
  /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
  operator2@servera's password: redhat
  Number of key(s) added: 1

  Now try logging into the machine, with:   "ssh 'operator2@servera'"
  and check to make sure that only the key(s) you wanted were added.
  ```
  </details>

5.	Убедитесь, что вы можете войти на **servera** как пользователь *operator2*, используя ключи SSH.

  <details>
  <summary>Показать решение</summary>

  Установите SSH-подключение к **servera** как пользователь *operator2*.

  ```
  [operator2@serverb ~]$ ssh operator2@servera
  ...output omitted...
  [operator2@servera ~]$ 
  ```

  Обратите внимание, что предыдущая команда `ssh` использовала ключи SSH для аутентификации.
  </details>

  Выйдите с **servera**.

6.	Убедитесь, что вы можете войти на **servera** как пользователь *root* с паролем *redhat*.

  <details>
  <summary>Показать решение</summary>
  
  Установите SSH-подключение к **servera** как пользователь *root* с паролем *redhat*.

  ```
  [operator2@serverb ~]$ ssh root@servera
  root@servera's password: redhat
  ...output omitted...
  [root@servera ~]# 
  ```

  Обратите внимание, что предыдущая команда `ssh` использовала пароль привилегированного пользователя для аутентификации, поскольку для этого пользователя не существует ключей SSH.
  </details>

  Выйдите с **servera**.

7.	Убедитесь, что вы можете войти на **servera** как пользователь *operator3* с паролем *redhat*.

  <details>
  <summary>Показать решение</summary>
  
  Установите SSH-подключение к **servera** как пользователь *operator3* с паролем *redhat*.

  ```
  [operator2@serverb ~]$ ssh operator3@servera
  operator3@servera's password: redhat
  ...output omitted...
  [operator3@servera ~]$ 
  ```

  Обратите внимание, что предыдущая команда `ssh` использовала пароль пользователя *operator3* для аутентификации, поскольку для пользователя *operator3* не существует ключей SSH.
  </details>

  Выйдите с **servera**.

8.	Настройте **sshd** на **servera**, чтобы пользователи не могли входить в систему от имени пользователя *root*. При необходимости используйте *redhat* в качестве пароля привилегированного пользователя.

  8.1.	Установите SSH-подключение к **servera** как пользователь *operator2*, используя ключи SSH.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator2@serverb ~]$ ssh operator2@servera
  ...output omitted...
  [operator2@servera ~]$ 
  ```
  </details>

  8.2.	На **servera** переключитесь на пользователя *root*. Используйте *redhat* в качестве пароля пользователя *root*.

  <details>
  <summary>Показать решение</summary>
  ```
  [operator2@servera ~]$ su -
  Password: redhat
  [root@servera ~]# 
  ```
  </details>

  8.3.	Задайте для параметра **PermitRootLogin** значение **no** в файле **/etc/ssh/sshd_config** и перезагрузите службу **sshd**. Используйте `vim /etc/ssh/sshd_config`, чтобы отредактировать файл конфигурации службы **sshd**.

  <details>
  <summary>Показать решение</summary>
  ```
  ...output omitted...
  PermitRootLogin no
  ...output omitted...
  [root@servera ~]# systemctl reload sshd
  ```
  </details>

  8.4.	Откройте еще один терминал на **workstation** и установите SSH-подключение к **serverb** как пользователь *operator2*. С **serverb** попробуйте войти на **servera** как пользователь *root*. Эта попытка должна закончиться неудачей, поскольку на предыдущем шаге вы отключили возможность входа в систему через SSH от имени пользователя *root*.

  <details>
  <summary>Примечание</summary>

  Для вашего удобства в учебной аудитории уже настроен вход без пароля между **workstation** и **serverb**.
  </details>

  ```
  [student@workstation ~]$ ssh operator2@serverb
  ...output omitted...
  [operator2@serverb ~]$ ssh root@servera
  root@servera's password: redhat
  Permission denied, please try again.
  root@servera's password: redhat
  Permission denied, please try again.
  root@servera's password: redhat
  root@servera: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
  ```

  По умолчанию команда `ssh` пытается сначала пройти аутентификацию на основе ключей, а в случае неудачи ― аутентификацию на основе пароля.


9.	Настройте **sshd** на **servera**, чтобы разрешить пользователям проходить аутентификацию только с помощью ключей SSH, но не паролей.

  9.1.	Вернитесь на первый терминал, где активна оболочка пользователя *root* на **servera**. Задайте для параметра **PasswordAuthentication** значение **no** в файле /**etc/ssh/sshd_config** и перезагрузите службу **sshd**. Используйте `vim /etc/ssh/sshd_config`, чтобы отредактировать файл конфигурации службы **sshd**.

  <details>
  <summary>Показать решение</summary>
  ```
  ...output omitted...
  PasswordAuthentication no
  ...output omitted...
  [root@servera ~]# systemctl reload sshd
  ```
  </details>

  9.2.	Перейдите на второй терминал, где активна оболочка пользователя *operator2* на **serverb**, и попробуйте войти на **servera** как пользователь *operator3*. Эта попытка должна завершиться неудачей, поскольку ключи SSH не настроены для пользователя *operator3*, а служба **sshd** на **servera** не позволяет использовать пароли для аутентификации.

  ```
  [operator2@serverb ~]$ ssh operator3@servera
  operator3@servera: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
  ```

  <details>
  <summary>Примечание</summary>

  Для большей точности можно использовать явные опции `-o PubkeyAuthentication=no` и `-o PasswordAuthentication=yes` с командой `ssh`. Так вы сможете переопределить значения по умолчанию для команды `ssh`, а также определить, что предыдущая команда не была выполнена, на основе настроек, которые вы изменили в файле **/etc/ssh/sshd_config** на предыдущем шаге.
  </details>

  9.3.	Вернитесь на первый терминал, где активна оболочка пользователя *root* на **servera**. Убедитесь, что параметр **PubkeyAuthentication** включен в файле **/etc/ssh/sshd_config**. Используйте `vim /etc/ssh/sshd_config` для просмотра файла конфигурации службы **sshd**.

  ```
  ...output omitted...
  #PubkeyAuthentication yes
  ...output omitted...
  ```

  Обратите внимание, что строка **PubkeyAuthentication** закомментирована. Любая закомментированная строка в этом файле использует значение по умолчанию. Закомментированные строки указывают на значения параметра по умолчанию. Аутентификация на основе открытого ключа SSH активна по умолчанию, как указывает закомментированная строка.

  9.4.	Вернитесь на второй терминал, где активна оболочка пользователя *operator2* на **serverb**, и попробуйте войти на **servera** как пользователь *operator2*. Вам удастся это сделать, поскольку ключи SSH настроены для входа пользователя *operator2* на **servera** с сервера **serverb**.

  ```
  [operator2@serverb ~]$ ssh operator2@servera
  ...output omitted...
  [operator2@servera ~]$ 
  ```

  9.5.	На втором терминале выйдите из оболочки пользователя *operator2* на обоих серверах **servera** и **serverb**.

  ```
  [operator2@servera ~]$ exit
  logout
  Connection to servera closed.
  [operator2@serverb ~]$ exit
  logout
  Connection to serverb closed.
  [student@workstation ~]$ 
  ```

  9.6.	Закройте второй терминал на **workstation**.

  ```
  [student@workstation ~]$ exit
  ```

  9.7.	На первом терминале выйдите из оболочки пользователя *root* на **servera**.

  ```
  [root@servera ~]# exit
  logout
  ```

  9.8.	На первом терминале выйдите из оболочки пользователя *operator2* на обоих серверах **servera** и **serverb**.

  ```
  [operator2@servera ~]$ exit
  logout
  Connection to servera closed.
  [operator2@serverb ~]$ exit
  logout
  [student@serverb ~]$ 
  ```

  9.9.	Выйдите с **serverb** и вернитесь в оболочку пользователя *student* на **workstation**.

  ```
  [student@serverb ~]$ exit
  logout
  Connection to serverb closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab ssh-customize finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab ssh-customize finish
```

Упражнение завершено.


