## Цели

Получение базовых навыков настройки системных часов и синхронизации с источником времени NTP

## Задачи

В этом упражнении вы настроите часовой пояс на сервере и убедитесь, что его системные часы синхронизированы с источником времени NTP.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На **workstation** запустите сценарий `lab log-maintain start`, чтобы начать упражнение. Этот сценарий гарантирует, что синхронизация времени отключена в системе **servera**, чтобы вы могли вручную обновить настройки системы и включить синхронизацию времени.

```
[student@workstation ~]$ lab log-maintain start
```

1.	На **workstation** установите SSH-подключение к **servera** как пользователь *student*.

  ```
  [student@workstation ~]$ ssh student@servera
  ...output omitted...
  [student@servera ~]$ 
  ```

2.	В этом упражнении представьте, что система **servera** была перенесена на Гаити, и вам необходимо соответствующим образом изменить часовой пояс. Обновите часовой пояс с помощью команды `timedatectl`. При этом используйте команду `sudo`, чтобы повысить права пользователя *student*. При необходимости используйте *student* в качестве пароля.

  <details>
  <summary>Показать решение</summary>

  2.1.	Выполните команду `tzselect`, чтобы определить часовой пояс для Гаити.

  
  ```
  [student@servera ~]$ tzselect
  Please identify a location so that time zone rules can be set correctly.
  Please select a continent, ocean, "coord", or "TZ".
  1) Africa
  2) Americas
  3) Antarctica
  4) Asia
  5) Atlantic Ocean
  6) Australia
  7) Europe
  8) Indian Ocean
  9) Pacific Ocean
  10) coord - I want to use geographical coordinates.
  11) TZ - I want to specify the time zone using the Posix TZ format.
  #? 2
  Please select a country whose clocks agree with yours.
  1) Anguilla              19) Dominican Republic    37) Peru
  2) Antigua & Barbuda     20) Ecuador               38) Puerto Rico
  3) Argentina             21) El Salvador           39) St Barthelemy
  4) Aruba                 22) French Guiana         40) St Kitts & Nevis
  5) Bahamas               23) Greenland             41) St Lucia
  6) Barbados              24) Grenada               42) St Maarten (Dutch)
  7) Belize                25) Guadeloupe            43) St Martin (French)
  8) Bolivia               26) Guatemala             44) St Pierre & Miquelon
  9) Brazil                27) Guyana                45) St Vincent
  10) Canada                28) Haiti                 46) Suriname
  11) Caribbean NL          29) Honduras              47) Trinidad & Tobago
  12) Cayman Islands        30) Jamaica               48) Turks & Caicos Is
  13) Chile                 31) Martinique            49) United States
  14) Colombia              32) Mexico                50) Uruguay
  15) Costa Rica            33) Montserrat            51) Venezuela
  16) Cuba                  34) Nicaragua             52) Virgin Islands (UK)
  17) Curaçao               35) Panama                53) Virgin Islands (US)
  18) Dominica              36) Paraguay
  #? 28
  The following information has been given:

  Haiti

  Therefore TZ='America/Port-au-Prince' will be used.
  Selected time is now: Tue Feb 19 00:51:05 EST 2019.
  Universal Time is now: Tue Feb 19 05:51:05 UTC 2019.
  Is the above information OK?
  1) Yes
  2) No
  #? 1

  You can make this change permanent for yourself by appending the line
  TZ='America/Port-au-Prince'; export TZ
  to the file '.profile' in your home directory; then log out and log in again.

  Here is that TZ value again, this time on standard output so that you
  can use the /usr/bin/tzselect command in shell scripts:
  America/Port-au-Prince
  ```

  Обратите внимание, что предыдущая команда `tzselect` показала правильный часовой пояс для Гаити.

  2.2.	Выполните команду `timedatectl`, чтобы изменить часовой пояс на сервере **servera** на America/Port-au-Prince.

  ```
  [student@servera ~]$ sudo timedatectl set-timezone \
  America/Port-au-Prince
  [sudo] password for student: student
  2.3.	Выполните команду timedatectl, чтобы убедиться, что часовой пояс был изменен на America/Port-au-Prince.
  [student@servera ~]$ timedatectl
            Local time: Tue 2019-02-19 01:16:29 EST
            Universal time: Tue 2019-02-19 06:16:29 UTC
                  RTC time: Tue 2019-02-19 06:16:29
                  Time zone: America/Port-au-Prince (EST, -0500)
  System clock synchronized: no
                NTP service: inactive
            RTC in local TZ: no
  ```
  </details>

3.	Настройте службу **chronyd** на **servera** для синхронизации системного времени с источником времени NTP **classroom.example.com**.

  <details>
  <summary>Показать решение</summary>

  3.1.	Отредактируйте файл **/etc/chrony.conf**, чтобы указать сервер **classroom.example.com** в качестве источника времени NTP. Используйте команду `sudo vim /etc/chrony.conf`, чтобы отредактировать файл конфигурации. В следующем выводе показана строка, которую необходимо добавить в файл конфигурации:

  ```
  ...output omitted...
  server classroom.example.com iburst
  ...output omitted...
  ```

  Предыдущая строка в файле конфигурации **/etc/chrony.conf** включает опцию **iburst** для ускорения начальной синхронизации времени.

  3.2.	Выполните команду `timedatectl`, чтобы включить синхронизацию времени на **servera**.

  ```
  [student@servera ~]$ sudo timedatectl set-ntp yes
  ```

  Предыдущая команда `timedatectl` активирует сервер NTP с измененными настройками в файле конфигурации **/etc/chrony.conf**. Предыдущая команда `timedatectl` может активировать службу **chronyd** или службу **ntpd** в зависимости от того, что в данный момент установлено в системе.
  </details>

4.	Убедитесь, что в настройках времени на **servera** задана синхронизация с источником времени **classroom.example.com** в учебной аудитории.

  4.1.	Выполните команду `timedatectl`, чтобы убедиться, что на servera включена синхронизация времени.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ timedatectl
                Local time: Tue 2019-02-19 01:52:17 EST
            Universal time: Tue 2019-02-19 06:52:17 UTC
                  RTC time: Tue 2019-02-19 06:52:17
                  Time zone: America/Port-au-Prince (EST, -0500)
  System clock synchronized: yes
                NTP service: active
            RTC in local TZ: no
  ```


  Если предыдущий вывод показывает, что часы не синхронизированы, подождите две секунды и повторно выполните команду timedatectl. Для успешной синхронизации настроек времени с источником времени требуется несколько секунд.
  </details>

  4.2.	Выполните команду `chronyc`, чтобы убедиться, что система **servera** в настоящее время синхронизирует свои настройки времени с источником времени **classroom.example.com**.

  <details>
  <summary>Показать решение</summary>
  ```
  [student@servera ~]$ chronyc sources -v
  210 Number of sources = 1

    .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
  / .- Source state '*' = current synced, '+' = combined , '-' = not combined,
  | /   '?' = unreachable, 'x' = time may be in error, '~' = time too variable.
  ||                                                 .- xxxx [ yyyy ] +/- zzzz
  ||      Reachability register (octal) -.           |  xxxx = adjusted offset,
  ||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
  ||                                \     |          |  zzzz = estimated error.
  ||                                 |    |           \
  MS Name/IP address         Stratum Poll Reach LastRx Last sample               
  ===============================================================================
  ^* classroom.example.com         2   6   377    62   +105us[ +143us] +/-   14ms
  ```

  Обратите внимание, что в предыдущем выводе в поле состояния источника (**S**) для источника времени NTP **classroom.example.com** отображается звездочка (`*`). Звездочка указывает, что время локальной системы в настоящее время успешно синхронизируется с источником времени NTP.
  </details>

  4.3.	Выйдите с **servera**.

  ```
  [student@servera ~]$ exit
  logout
  Connection to servera closed.
  [student@workstation ~]$ 
  ```

## Конец

На **workstation** запустите сценарий `lab log-maintain finish`, чтобы закончить упражнение. Этот сценарий восстанавливает на servera исходный часовой пояс, а также все исходные настройки времени.

```
[student@workstation ~]$ lab log-maintain finish
```

Упражнение завершено.



