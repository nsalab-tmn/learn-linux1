## Цели

Получение базовых навыков безопасной передачи файлов между системами

## Задачи

В этом упражнении вы скопируете файлы из удаленной системы в локальный каталог с помощью команды scp.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab archive-transfer start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хостов **servera** и **serverb** в сети. Сценарий также проверяет, что файл и каталог, которые будут созданы в упражнении, не существуют на **servera**.

```
[student@workstation ~]$ lab archive-transfer start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	С помощью команды `scp` скопируйте каталог **/etc/ssh** с хоста **serverb** в каталог **/home/student/serverbackup** на хосте **servera**.

  <details>
  <summary>Показать решение</summary>


  2.1.	На **servera** создайте каталог с именем **/home/student/serverbackup**.

  ```
  [student@servera ~]$ mkdir ~/serverbackup
  ```

  2.2.	С помощью команды `scp` рекурсивно скопируйте каталог **/etc/ssh** с хоста **serverb** в каталог **/home/student/serverbackup** на хосте **servera**. При запросе введите пароль *redhat*. Обратите внимание, что только пользователь *root* может читать все содержимое в каталоге **/etc/ssh**.

  ```
  [student@servera ~]$ scp -r root@serverb:/etc/ssh ~/serverbackup
  The authenticity of host 'serverb (172.25.250.11)' can't be established.
  ECDSA key fingerprint is SHA256:qaS0PToLrqlCO2XGklA0iY7CaP7aPKimerDoaUkv720.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added 'serverb,172.25.250.11' (ECDSA) to the list of known hosts.
  root@serverb's password: redhat
  moduli                                       100%  550KB  57.9MB/s   00:00
  ssh_config                                   100% 1727     1.4MB/s   00:00
  05-redhat.conf                               100%  690     1.6MB/s   00:00
  01-training.conf                             100%   36    80.5KB/s   00:00
  ssh_host_ed25519_key                         100%  387     1.2MB/s   00:00
  ssh_host_ed25519_key.pub                     100%   82   268.1KB/s   00:00
  ssh_host_ecdsa_key                           100%  492     1.5MB/s   00:00
  ssh_host_ecdsa_key.pub                       100%  162   538.7KB/s   00:00
  ssh_host_rsa_key                             100% 1799     4.9MB/s   00:00
  ssh_host_rsa_key.pub                         100%  382     1.2MB/s   00:00
  sshd_config                                  100% 4469     9.5MB/s   00:00
  ```


  2.3.	Убедитесь, что каталог **/etc/ssh** с хоста **serverb** скопирован в каталог **/home/student/serverbackup** на хосте **servera**.

  ```
  [student@servera ~]$ ls -lR ~/serverbackup
  /home/student/serverbackup:
  total 0
  drwxr-xr-x. 3 student student 245 Feb 11 18:35 ssh

  /home/student/serverbackup/ssh:
  total 588
  -rw-r--r--. 1 student student 563386 Feb 11 18:35 moduli
  -rw-r--r--. 1 student student   1727 Feb 11 18:35 ssh_config
  drwxr-xr-x. 2 student student     28 Feb 11 18:35 ssh_config.d
  -rw-------. 1 student student   4469 Feb 11 18:35 sshd_config
  -rw-r-----. 1 student student    492 Feb 11 18:35 ssh_host_ecdsa_key
  -rw-r--r--. 1 student student    162 Feb 11 18:35 ssh_host_ecdsa_key.pub
  -rw-r-----. 1 student student    387 Feb 11 18:35 ssh_host_ed25519_key
  -rw-r--r--. 1 student student     82 Feb 11 18:35 ssh_host_ed25519_key.pub
  -rw-r-----. 1 student student   1799 Feb 11 18:35 ssh_host_rsa_key
  -rw-r--r--. 1 student student    382 Feb 11 18:35 ssh_host_rsa_key.pub

  /home/student/serverbackup/ssh/ssh_config.d:
  total 8
  -rw-r--r--. 1 student student  36 Feb 11 18:35 01-training.conf
  -rw-r--r--. 1 student student 690 Feb 11 18:35 05-redhat.conf
  ```
  </details>

3.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab archive-transfer finish`, чтобы закончить упражнение.

```
[student@workstation ~]$ lab archive-transfer finish
```

Упражнение завершено.

