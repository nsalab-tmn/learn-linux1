## Цели

Получение базовых навыков управления репозиториями yum

## Задачи

В этом упражнении вы настроите сервер для получения пакетов из удаленного репозитория Yum, а затем обновите или установите пакет из этого репозитория.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** выполните команду `lab software-repo start`. Эта команда запускает подготовительный сценарий, который проверяет доступность хоста **servera** в сети. Сценарий также гарантирует, что пакет yum установлен.

```
[student@workstation ~]$ lab software-repo start
```

1.	С помощью команды `ssh` войдите на **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	С помощью команды `sudo -i` переключитесь на пользователя *root* в командной строке.

  ```
  [student@servera ~]$ sudo -i
  [sudo] password for student: student
  [root@servera ~]# 
  ```

3.	Настройте репозитории программного обеспечения на **servera** для получения нестандартных пакетов и обновлений по следующим URL-адресам:

  * Нестандартные пакеты по адресу http://content.example.com/rhel8.2/x86_64/rhcsa-practice/rht
  * Обновления нестандартных пакетов по адресу http://content.example.com/rhel8.2/x86_64/rhcsa-practice/errata

  С помощью команды `yum config-manager` добавьте репозиторий нестандартных пакетов.

  ```
  [root@servera ~]# yum config-manager \
  --add-repo "http://content.example.com/rhel8.2/x86_64/rhcsa-practice/rht"
  Adding repo from: http://content.example.com/rhel8.2/x86_64/rhcsa-practice/rht
  ```

  Просмотрите файл репозитория программного обеспечения, созданный предыдущей командой в каталоге **/etc/yum.repos.d**. Используйте команду `vim` для редактирования файла и добавьте параметр **gpgcheck=0**, чтобы отключить проверку ключа GPG для репозитория.

  ```
  [root@servera ~]# vim \
  /etc/yum.repos.d/content.example.com_rhel8.2_x86_64_rhcsa-practice_rht.repo
  [content.example.com_rhel8.2_x86_64_rhcsa-practice_rht]
  name=created by dnf config-manager from http://content.example.com/rhel8.2/x86_64/rhcsa-practice/rht
  baseurl=http://content.example.com/rhel8.2/x86_64/rhcsa-practice/rht
  enabled=1
  gpgcheck=0
  ```

  Создайте файл **/etc/yum.repos.d/errata.repo**, чтобы включить репозиторий обновлений со следующим содержимым:

  ```
  [rht-updates]
  name=rht updates
  baseurl=http://content.example.com/rhel8.2/x86_64/rhcsa-practice/errata
  enabled=1
  gpgcheck=0
  Выполните команду yum repolist all, чтобы отобразить все репозитории в системе.
  [root@servera ~]# yum repolist all
  repo id                                                repo name       status
  content.example.com_rhel8.2_x86_64_rhcsa-practice_rht  created by .... enabled
  rht-updates                                            rht updates     enabled
  ...output omitted...
  ```

4.	Отключите репозиторий программного обеспечения **rht-updates** и установите пакет **rht-system**.

  4.1.	С помощью команды `yum config-manager --disable` отключите репозиторий **rht-updates**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum config-manager --disable rht-updates
  ```
  </details>

  4.2.	Отобразите и установите пакет **rht-system**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum list rht-system
  Available Packages
  rht-system.noarch  1.0.0-1 content.example.com_rhel8.2_x86_64_rhcsa-practice_rht
  [root@servera ~]# yum install rht-system
  Dependencies resolved.
  ================================================================================
  Package            Arch           Version                Repository       Size
  ================================================================================
  Installing:
  rht-system         noarch         1.0.0-1               content..._rht   3.7 k
  ...output omitted...
  Is this ok [y/N]: y
  ...output omitted...
  Installed:
    rht-system-1.0.0-1.noarch
  Complete!
  ```
  </details>

  4.3.	Убедитесь, что пакет **rht-system** установлен, и запомните номер версии пакета.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum list rht-system
  Installed Packages
  rht-system.noarch  1.0.0-1 @content.example.com_rhel8.2_x86_64_rhcsa-practice_rht
  ```
  </details>

5.	Включите репозиторий программного обеспечения **rht-updates** и обновите все соответствующие программные пакеты.

  5.1.	С помощью команды `yum config-manager --enable` включите репозиторий **rht-updates**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum config-manager --enable rht-updates
  ```
  </details>

  5.2.	Используйте команду `yum update` для обновления всех программных пакетов на **servera**.

  <details>
  <summary>Показать решение</summary>

  ```
  [root@servera ~]# yum update
  Dependencies resolved.
  ================================================================================
  Package            Arch           Version                Repository       Size
  ================================================================================
  Upgrading:
  rht-system         x86_64         1.0.0-2.el7            rht-updates      3.9 k
  ...output omitted...
  Is this ok [y/N]: y
  ...output omitted...
  Complete!
  5.3.	Убедитесь, что пакет rht-system обновлен, и запомните номер версии пакета.
  [root@servera ~]# yum list rht-system
  Installed Packages
  rht-system.noarch      1.0.0-2.el7         @rht-updates
  ```
  </details>

6.	Выйдите с servera.

  ```
  [root@servera ~]# exit  
  logout
  [student@servera ~]$ exit 
  logout
  Connection to servera closed.
  [student@workstation ~]$
  ```

## Конец

На **workstation** запустите сценарий `lab software-repo finish`, чтобы закончить упражнение. Этот сценарий удаляет все репозитории и пакеты, установленные на **servera** во время упражнения.

```
[student@workstation ~]$ lab software-repo finish
```

Упражнение завершено.