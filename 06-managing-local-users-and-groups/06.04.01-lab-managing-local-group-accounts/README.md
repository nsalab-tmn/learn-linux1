## Цели

Получение базовых навыков управления локальными учетными записями групп

## Задачи 

В этом упражнении вы создадите группы, будете использовать их в качестве дополнительных групп для некоторых пользователей, не изменяя основных групп этих пользователей, а также настроите одну из групп с правами `sudo` для выполнения команд от имени пользователя *root*.

## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab users-group-manage start`, чтобы начать упражнение. Этот сценарий создает необходимые учетные записи пользователей для настройки среды.

```
[student@workstation ~]$ lab users-group-manage start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

    ```
    [student@workstation ~]$ ssh student@servera
    ...output omitted...
    [student@servera ~]$ 
    ```

2.	На servera с помощью команды `sudo` переключитесь на пользователя *root*, чтобы унаследовать полную среду пользователя *root*.

    <details>
    <summary>Показать решение</summary>


    ```
    [student@servera ~]$ sudo su -
    [sudo] password for student: student
    [root@servera ~]# 
    ```
    </details>

3.	Создайте дополнительную группу *operators* с GID 30000.

    <details>
    <summary>Показать решение</summary>


    ```
    [root@servera ~]# groupadd -g 30000 operators
    ```
    </details>

4.	Создайте еще одну дополнительную группу с именем *admin*.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# groupadd admin
    ```
    </details>

5.	Убедитесь, что дополнительные группы *operators* и *admin* существуют.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# tail /etc/group
    ...output omitted...
    operators:x:30000:
    admin:x:30001:
    ```
    </details>

6.	Убедитесь, что пользователи *operator1*, *operator2* и *operator3* принадлежат к группе *operators*.

    6.1.	Добавьте пользователей *operator1*, *operator2* и *operator3* в группу *operators*.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# usermod -aG operators operator1
    [root@servera ~]# usermod -aG operators operator2
    [root@servera ~]# usermod -aG operators operator3
    ```
    </details>

    6.2.	Убедитесь, что пользователи были успешно добавлены в группу.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# id operator1
    uid=1002(operator1) gid=1002(operator1) groups=1002(operator1),30000(operators)
    [root@servera ~]# id operator2
    uid=1003(operator2) gid=1003(operator2) groups=1003(operator2),30000(operators)
    [root@servera ~]# id operator3
    uid=1004(operator3) gid=1004(operator3) groups=1004(operator3),30000(operators)
    ```
    </details>

7.	Убедитесь, что пользователи *sysadmin1*, *sysadmin2* и *sysadmin3* принадлежат к группе *admin*. Включите права администратора для всех участников группы *admin*. Убедитесь, что любой участник группы *admin* может выполнять административные команды.

    7.1.	Добавьте пользователей *sysadmin1*, *sysadmin2* и *sysadmin3* в группу *admin*.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# usermod -aG admin sysadmin1
    [root@servera ~]# usermod -aG admin sysadmin2
    [root@servera ~]# usermod -aG admin sysadmin3
    ```
    </details>

    7.2.	Убедитесь, что пользователи были успешно добавлены в группу.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# id sysadmin1
    uid=1005(sysadmin1) gid=1005(sysadmin1) groups=1005(sysadmin1),30001(admin)
    [root@servera ~]# id sysadmin2
    uid=1006(sysadmin2) gid=1006(sysadmin2) groups=1006(sysadmin2),30001(admin)
    [root@servera ~]# id sysadmin3
    uid=1007(sysadmin3) gid=1007(sysadmin3) groups=1007(sysadmin3),30001(admin)
    ```
    </details>

    7.3.	Проверьте участников дополнительных групп, просмотрев файл **/etc/group**.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# tail /etc/group
    ...output omitted...
    operators:x:30000:operator1,operator2,operator3
    admin:x:30001:sysadmin1,sysadmin2,sysadmin3
    ```
    </details>

    7.4.	Создайте файл **/etc/sudoers.d/admin**, чтобы у участников группы *admin* были полные административные права.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# echo "%admin ALL=(ALL) ALL" >> /etc/sudoers.d/admin
    ```
    </details>

    7.5.	Переключитесь на пользователя *sysadmin1* (участника группы *admin*) и убедитесь, что вы можете выполнить команду `sudo` как *sysadmin1*.

    <details>
    <summary>Показать решение</summary>

    ```
    [root@servera ~]# su - sysadmin1
    [sysadmin1@servera ~]$ sudo cat /etc/sudoers.d/admin
    [sudo] password for sysadmin1: redhat
    %admin ALL=(ALL) ALL
    ```
    </details>

    7.6.	Выйдите из оболочки пользователя *sysadmin1*, чтобы вернуться в оболочку пользователя *root*.

    ```
    [sysadmin1@servera ~]$ exit
    logout
    [root@servera ~]# 
    ```

    7.7.	Закройте оболочку пользователя *root*, чтобы вернуться в оболочку пользователя *student*.

    ```
    [root@servera ~]# exit
    logout
    [student@servera ~]$ 
    ```

    7.8.	Выйдите с **servera**.

    ```
    [student@servera ~]$ exit
    logout
    Connection to servera closed.
    [student@workstation ~]$ 
    ```

## Конец

На **workstation** запустите сценарий `lab users-group-manage finish`, чтобы закончить упражнение. Этот сценарий удаляет учетные записи пользователей, созданные в начале упражнения.

```
[student@workstation ~]$ lab users-group-manage finish
```

Упражнение завершено.