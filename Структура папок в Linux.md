# Структура папок в [[Ubuntu|Linux]]

[[Файловая структура Linux наглядно]]

## Описание

##### / - КОРЕНЬ

Это главный каталог в системе Linux. По сути, это и есть файловая система Linux. Здесь нет дисков или чего-то подобного, как в Windows. Вместо этого, адреса всех файлов начинаются с корня, а дополнительные разделы, флешки или оптические диски подключаются в папки корневого каталога.

Только пользователь root имеет право читать и изменять файлы в этом каталоге. Обратите внимание, что у пользователя root домашний каталог /root, но не сам /.

##### /BIN - (BINARIES) БИНАРНЫЕ ФАЙЛЫ ПОЛЬЗОВАТЕЛЯ

Этот каталог содержит исполняемые файлы. Здесь расположены программы, которые можно использовать в однопользовательском режиме или режиме восстановления. Одним словом, те утилиты, которые могут использоваться пока еще не подключен каталог /usr/. Это такие общие команды, как cat, ls, tail, ps и т д.

##### /SBIN - (SYSTEM BINARIES) СИСТЕМНЫЕ ИСПОЛНЯЕМЫЕ ФАЙЛЫ

Так же как и /bin, содержит двоичные исполняемые файлы, которые доступны на ранних этапах загрузки, когда не примонтирован каталог /usr. Но здесь находятся программы, которые можно выполнять только с правами суперпользователя. Это разные утилиты для обслуживания системы. Например, iptables, reboot, fdisk, ifconfig,swapon и т д.

##### /ETC - (ETCETERA) КОНФИГУРАЦИОННЫЕ ФАЙЛЫ

В этой папке содержатся конфигурационные файлы всех программ, установленных в системе. Кроме конфигурационных файлов, в системе инициализации Init Scripts, здесь находятся скрипты запуска и завершения системных демонов, монтирования файловых систем и автозагрузки программ. Структура каталогов linux в этой папке может быть немного запутанной, но предназначение всех их - настройка и конфигурация.

##### /DEV - (DEVICES) ФАЙЛЫ УСТРОЙСТВ

В Linux все, в том числе внешние устройства являются файлами. Таким образом, все подключенные флешки, клавиатуры, микрофоны, камеры - это просто файлы в каталоге /dev/. Этот каталог содержит не совсем обычную файловую систему. Структура файловой системы Linux и содержащиеся в папке /dev файлы инициализируются при загрузке системы, сервисом udev. Выполняется сканирование всех подключенных устройств и создание для них специальных файлов. Это такие устройства, как: /dev/sda, /dev/sr0, /dev/tty1, /dev/usbmon0 и т д.

##### /PROC - (PROCCESS) ИНФОРМАЦИЯ О ПРОЦЕССАХ

Это тоже необычная файловая система, а подсистема, динамически создаваемая ядром. Здесь содержится вся информация о запущенных процессах в реальном времени. По сути, это псевдофайловая система, содержащая подробную информацию о каждом процессе, его Pid, имя исполняемого файла, параметры запуска, доступ к оперативной памяти и так далее. Также здесь можно найти информацию об использовании системных ресурсов, например, /proc/cpuinfo, /proc/meminfo или /proc/uptime. Кроме файлов в этом каталоге есть большая структура папок linux, из которых можно узнать достаточно много информации о системе.

##### /VAR (VARIABLE) - ПЕРЕМЕННЫЕ ФАЙЛЫ

Название каталога /var говорит само за себя, он должен содержать файлы, которые часто изменяются. Размер этих файлов постоянно увеличивается. Здесь содержатся файлы системных журналов, различные кеши, базы данных и так далее. Дальше рассмотрим назначение каталогов Linux в папке /var/.

##### /VAR/LOG - ФАЙЛЫ ЛОГОВ

Здесь содержатся большинство файлов логов всех программ, установленных в операционной системе. У многих программ есть свои подкаталоги в этой папке, например, /var/log/apache - логи веб-сервера, /var/log/squid - файлы журналов кеширующего сервера squid. Если в системе что-либо сломалось, скорее всего, ответы вы найдете здесь.

##### /VAR/LIB - БАЗЫ ДАННЫХ

Еще один тип изменяемых файлов - это файлы баз данных, пакеты, сохраненные пакетным менеджером и т д.

##### /VAR/MAIL - ПОЧТА
В эту папку почтовый сервер складывает все полученные или отправленные электронные письма, здесь же могут находиться его логи и файлы конфигурации.

##### /VAR/SPOOL - ОЧЕРЕДИ

Изначально, эта папка отвечала за очереди печати на принтере и работу набора программ cups.

##### /VAR/LOCK - ФАЙЛЫ БЛОКИРОВОК

Здесь находятся файлы блокировок. Эти файлы означают, что определенный ресурс, файл или устройство занят и не может быть использован другим процессом. Apt-get, например, блокирует свою базу данных, чтобы другие программы не могли ее использовать, пока программа с ней работает.

##### /VAR/RUN - PID ПРОЦЕССОВ

Содержит файлы с PID процессов, которые могут быть использованы, для взаимодействия между программами. В отличие от каталога /run данные сохраняются после перезагрузки.

##### /TMP (TEMP) - ВРЕМЕННЫЕ ФАЙЛЫ

В этом каталоге содержатся временные файлы, созданные системой, любыми программами или пользователями. Все пользователи имеют право записи в эту директорию.

Файлы удаляются при каждой перезагрузке. Аналогом Windows является папка Windows\Temp, здесь тоже хранятся все временные файлы.

##### /USR - (USER APPLICATIONS) ПРОГРАММЫ ПОЛЬЗОВАТЕЛЯ

Это самый большой каталог с большим количеством функций. Тут наиболее большая структура каталогов Linux. Здесь находятся исполняемые файлы, исходники программ, различные ресурсы приложений, картинки, музыку и документацию.

##### /USR/BIN/ - ИСПОЛНЯЕМЫЕ ФАЙЛЫ

Содержит исполняемые файлы различных программ, которые не нужны на первых этапах загрузки системы, например, музыкальные плееры, графические редакторы, браузеры и так далее.

##### /USR/SBIN/

Содержит двоичные файлы программ для системного администрирования, которые нужно выполнять с правами суперпользователя. Например, таких как Gparted, sshd, useradd, userdel и т д.

##### /USR/LIB/ - БИБЛИОТЕКИ

Содержит библиотеки для программ из /usr/bin или /usr/sbin.

##### /USR/LOCAL - ФАЙЛЫ ПОЛЬЗОВАТЕЛЯ

Содержит файлы программ, библиотек, и настроек созданные пользователем. Например, здесь могут храниться программы собранные и установленные из исходников и скрипты, написанные вручную.

##### /HOME - ДОМАШНЯЯ ПАПКА

В этой папке хранятся домашние каталоги всех пользователей. В них они могут хранить свои личные файлы, настройки программ и т д. Например, /home/sergiy и т д. Если сравнивать с Windows, то это ваша папка пользователя на диске C, но в отличии от WIndows, home как правило размещается на отдельном разделе, поэтому при переустановке системы все ваши данные и настройки программ сохранятся.

##### /BOOT - ФАЙЛЫ ЗАГРУЗЧИКА

Содержит все файлы, связанные с загрузчиком системы. Это ядро vmlinuz, образ initrd, а также файлы загрузчика, находящие в каталоге /boot/grub.

##### /LIB (LIBRARY) - СИСТЕМНЫЕ БИБЛИОТЕКИ

Содержит файлы системных библиотек, которые используются исполняемыми файлами в каталогах /bin и /sbin.

Библиотеки имеют имена файлов с расширением *.so и начинаются с префикса lib*. Например, libncurses.so.5.7. Папка /lib64 в 64 битных системах содержит 64 битные версии библиотек из /lib. Эту папку можно сравнить с WIndows\system32, там тоже сгружены все библиотеки системы, только там они лежат смешанные с исполняемыми файлами, а здесь все отдельно.

##### /OPT (OPTIONAL APPLICATIONS) - ДОПОЛНИТЕЛЬНЫЕ ПРОГРАММЫ

В эту папку устанавливаются проприетарные программы, игры или драйвера. Это программы созданные в виде отдельных исполняемых файлов самими производителями. Такие программы устанавливаются в под-каталоги /opt/, они очень похожи на программы Windows, все исполняемые файлы, библиотеки и файлы конфигурации находятся в одной папке.

##### /MNT (MOUNT) - МОНТИРОВАНИЕ

В этот каталог системные администраторы могут монтировать внешние или дополнительные файловые системы.

##### /MEDIA - СЪЕМНЫЕ НОСИТЕЛИ

В этот каталог система монтирует все подключаемые внешние накопители - USB флешки, оптические диски и другие носители информации.

##### /SRV (SERVER) - СЕРВЕР

В этом каталоге содержатся файлы серверов и сервисов. Например, могут содержаться файлы веб-сервера apache.

##### /RUN - ПРОЦЕССЫ

Еще один каталог, содержащий PID файлы процессов, похожий на /var/run, но в отличие от него, он размещен в TMPFS, а поэтому после перезагрузки все файлы теряются.

##### /SYS (SYSTEM) - ИНФОРМАЦИЯ О СИСТЕМЕ

Назначение каталогов Linux из этой папки - получение информации о системе непосредственно от ядра. Это еще одна файловая система организуемая ядром и позволяющая просматривать и изменить многие параметры работы системы, например, работу swap, контролировать вентиляторы и многое другое.

[Источник](https://losst.ru/ctruktura-fajlovoj-sistemy-linux)






## Альтернативное описание
**/bin** - В этом каталоге хранятся основные команды, необходимые пользователю для работы в системе. Например, такие как командные оболочки и команды файловой системы (ls, cp и т.д.). Каталог /bin обычно не изменяется после установки. Если изменяется, то обычно лишь при обновлениях пакетов программ, предоставленных разработчиками операционной системы.  
  
**/boot** - В этом каталоге хранятся файлы, используемые загрузчиком ОС — LInux LOader (LILO). Этот каталог так же практически не изменяется после установки.  
  
**/dev** - В этом каталоге размещены описания устройств системы. В Linux всё рассматривается, как файл, даже различные устройства, такие как последовательные порты, жёсткие диски и сканеры. Для получения доступа к определённому устройству, необходимо чтобы существовал специальный файл, называемый device node. Все эти файлы находятся в каталоге /dev. Аналогично устроено большинство UNIX-подобных операционных систем.  
  
**/etc** - Этот каталог содержит файлы настроек: всё, от конфигурационных файлов системы X Window, базы данных пользователей и до стартовых сценариев.  
  
**/home** - В этом каталоге размещены домашние каталоги пользователей. Linux является многопользовательской системой и каждому пользователю присваивается имя и уникальный каталог для персональных файлов. Этот каталог называется "home" (домашним) каталогом пользователя.  
  
**/lib** - В этом каталоге находятся системные библиотеки, необходимые для основных программ: библиотека C, динамический загрузчик, библиотека ncurses, модули ядра и другое.  
  
**/lib/modules** - Подгружаемые модули для ядра (например, сетевые драйверы или поддержка дополнительных файловых систем).  
  
**/lost+found** - В этом каталоге сохраняются восстановленные части файловой системы. При загрузке системы происходит проверка файловых систем на наличие ошибок. Для исправления ошибок файловой системы запускается программа fsck.  
  
**/mnt** - Этот каталог предоставляется как временная точка монтирования для жёстких дисков, дискет, компакт-дисков или отключаемых устройств.  
  
**/opt** - В этом каталоге размещаются дополнительные пакеты программ. Особенность Linux в том, что все пакеты программ, устанавливаются в этот каталог, например /opt/<программный пакет>. В последствии если этот пакет больше не будет нужен, то достаточно всего лишь удалить соответствующий каталог. В дистрибутивах SlackWare некоторые программы изначально поставляются в каталоге /opt (например, KDE - в /opt/kde).  
  
**/proc** - Это специальный каталог не входящий в файловую систему. Каталог /proc представляет собой виртуальную файловую систему, которая предоставляет доступ к информации ядра. Различная информация, которую ядро может сообщить пользователям, находится в "файлах" каталога /proc. Например, в файле /proc/modules находится список загруженных модулей ядра. А в файле /proc/cpuinfo — информация о процессоре компьютера.  
  
**/root** - Это домашний каталог администратора, вместо /home/root. Это потому, что каталог /home может находиться в разделе, отличном от корневого (/) и если по какой-то причине /home не может быть подключён, то пользователь root вынужден будет войти в систему, чтобы решить проблему. И если его домашний каталог на другом диске, то это усложнит вход в систему.  
  
**/sbin** - В этом каталоге хранятся основные программы, выполняемые пользователем root а так же программы, выполняемые в процессе загрузки. Обычные пользователи не могут пользоваться этими программами.  
  
**/tmp** - Временное хранилище данных. Все пользователи имеют права чтения и записи в этом каталоге.  
  
**/usr** - Это один из самых больших каталогов в системе. Практически всё остальное расположено здесь. Программы, документация, исходный код ядра и система X Window. Именно в этот каталог, чаще всего, устанавливаются программы.  
  
**/var** - В этом каталоге хранятся системные лог-файлы, кэш-файлы и файлы-замки программ. Это каталог для часто меняющихся данных.  
  
Каталог /etc - В этом каталоге содержится довольно много различных конфигурационных файлов. Некоторые из них рассмотрены ниже. Здесь также располагаются файлы, используемые для конфигурирования сети.  
  
**/etc/rc.d** - Командные файлы, выполняемые при запуске системы или при смене ее уровня выполнения.  
  
**/etc/passwd** - База данных пользователей, в которой содержится информация об имени пользователя, его настоящем имени, личном каталоге, зашифрованный пароль и другие данные. Формат этого файла рассмотрен в man-руководстве к команде passwd.  
  
**/etc/fdprm** - Таблица параметров флоппи-дисковода, определяющая формат записи. Устанавливается программой setfdprm.  
  
**/etc/fstab** - Список файловых систем, автоматически монтируемых во время запуска системы командой mount -a (она запускается из командного файла /etc/rc.d/rc.S). Здесь также содержится информация о swaр-областях, автоматически устанавливаемых командой swapon -a.  
  
**/etc/group** - Подобен файлу /etc/рasswd, только здесь содержится информация о группах, а не о пользователях.  
  
**/etc/inittab** - Конфигурационный файл демона init.  
  
**/etc/issue** - Выводится программой getty перед приглашением login. Обычно здесь содержится краткое описание системы.  
  
**/etc/magic** - Конфигурационный файл команды file. Содержит описания различных форматов файлов, опираясь на которые эта команда определяет тип файла. Также см. руководства к magic и file.  
  
**/etc/motd** - Сообщение дня, автоматически выводится при успешном подключении к системе. Часто используется для информирования пользователей об изменениях в работе системы. Немного напоминает "совет дня" в Windows.  
  
**/etc/mtab** - Список смонтированных на данный момент файловых систем. Изначально устанавливается командными файлами при запуске, а затем автоматически модифицируется командой mount. Используется при необходимости получения доступа к смонтированным файловым системам (например, командой df).  
  
**/etc/shadow** - Теневая база данных пользователей. При этом информация из файла /etc/рasswd перемещается в /etc/shadow, который недоступен для чтению никому, кроме пользователя root. Это усложняет взлом системы.  
  
**/etc/login.defs** - Конфигурационный файл команды login.  
  
**/etc/printcap** - То же, что и /etc/termcap, только используется при работе с принтером.  
  
**/etc/profile** - Этот командный файл выполняется оболочкой Bourne Shell при запуске системы, что позволяет изменять системные установки для всех пользователей.  
  
**/etc/securetty** - Определяет терминалы, с которых может подключаться к системе пользователь root. Обычно это только виртуальные консоли, что усложняет взлом системы через модем или сеть.  
  
**/etc/shells** - Список рабочих оболочек. Команда chsh позволяет менять рабочую оболочку только на оболочки, находящиеся в этом файле. Процесс ftрd, предоставляющий работу с FTР, проверяет наличие оболочки пользователя в файле /etc/shells и не позволяет пользователю подключится к системе, пока ее имя не будет найдено в этом файле.  
  
**/etc/termcap** - База данных совместимости терминалов. Здесь находятся escape-последовательности для различных типов терминалов, что позволяет работать программам на разных типах терминалов.  
  
Каталог /dev - В этом каталоге находятся файлы устройств. Названия этих файлов соответствуют специальным положениям, рассмотренным в списке устройств (Device list). Файлы устройств создаются во время установки системы, а затем с помощью скрипта /dev/MAKEDEV. Файл /dev/MAKEDEV.local используется при создании локальных файлов устройств или ссылок (т.е. тех, что не соответствуют стандарту MAKEDEV).  
  
Каталог /usr - Обычно файловая система /usr достаточно большая по объему, так как многие программы установлены именно здесь. Вся информация в каталоге /usr помещается туда во время установки системы. Отдельно устанавливаемые пакеты программ и другие файлы размещаются в каталоге /usr/local. Некоторые подкаталоги системы /usr рассмотрены ниже (для более подробной информации см. описание стандарта FSSTND).  
  
**/usr/X11R6** - Все файлы, используемые системой X Window. Для упрощения установки и администрирования, файлы системы X Window размещаются в отдельной структуре каталогов, которая находится в /usr/X11R6 и идентична структуре /usr.  
  
**/usr/bin** - Практически все команды, хотя некоторые находятся в /bin или в /usr/local/bin.  
  
**/usr/sbin** - Команды, используемые при администрировании системы и не предназначенные для размещения в файловой системе root (например, здесь находится большинство программ-серверов).  
  
**/usr/man, /usr/info, /usr/doc** - Файлы man-руководств, документации GNU Info и другая документация.  
  
**/usr/include** - Подключаемые файлы библиотек для языка С.  
  
**/usr/src** - Исходные тексты программ, установленных в системе, в том числе ядра Linux.  
  
**/usr/lib** - Неизменяемые файлы данных для программ и подсистем, включая некоторые конфигурационные файлы. Имя lib происходит от library (библиотека); первоначально библиотеки подпрограмм для программирования хранились в /usr/lib.  
  
**/usr/local** - Здесь размещаются отдельно устанавливаемые пакеты программ и другие файлы.  
  
Каталог /var - Эта файловая система содержит файлы, изменяемые при нормально работающей системе. Она специфична для каждого компьютера и не может быть разделена в сети между несколькими машинами.  
  
**/var/man/cat*** - Временный каталог для форматируемых страниц руководств. Источником этих страниц является каталог /usr/man/man*. Некоторые руководства поставляются в отформатированном виде. Они располагаются в /usr/man/cat*. Остальные руководства перед просмотром должны быть отформатированы. Затем они помещаются в каталог /var/man и при повторном просмотре в форматировании не нуждаются. Каталог /var/man/cat часто очищается, таким же образом, как и прочие временные каталоги.  
  
**/var/lib** - Файлы, изменяемые при нормальном функционировании системы.  
  
**/var/local** - Изменяемые данные для программ, установленных в /usr/local (то есть, программы которые были установлены администратором системы). Обратите внимание, что даже в местном масштабе установленные программы должны использовать другие /var каталоги, например, /var/lock.  
  
**/var/lock** - Файлы-защелки. Многие программы при обращении к какому-либо файлу устройства создают здесь файл-защелку. Другие программы при обращении к какому-либо устройству сначала проверяют наличие файла-защелки в этом каталоге, а затем уже производят доступ к этому устройству.  
  
**/var/log** - Журнальные файлы различных программ, в особенности login (/var/log/wtmр, куда записываются все подключения и выходы из системы) и syslog (/var/log/messages, где обычно хранятся все сообщения ядра и системных программ). Файлы из /var/log необходимо регулярно удалять, иначе разрастутся сверх всякой меры.  
  
**/var/run** - Файлы, информация в которых соответствует действительности только до очередной перезагрузки системы. Например, файл /var/run/utmp содержит информацию о пользователях, подключенных к системе в данный момент.  
  
**/var/spool** - Каталоги, используемые для хранения почты, новостей, очереди для принтера, а также для других задач. Для каждой задачи существует отдельный каталог в /var/spool, например, почтовые ящики пользователей хранятся в /var/spool/mail.  
  
**/var/tmp** - Каталог для временных файлов, размер которых достаточно велик или время существования которых больше, чем в /tmp. Хотя администратор системы не должен бы держать очень уж старые файлы в /var/tmp.  
  
Каталог /proc - Файловая система /proc является виртуальной и в действительности она не существует на диске. Ядро создает ее в памяти компьютера. Система /proc предоставляет информацию о системе (изначально только о процессах — отсюда ее название). Некоторые наиболее важные файлы и каталоги рассмотрены ниже. Более подробную информацию о структуре и содержании файловой системы /proc можно найти в man-руководстве к proc.  
  
**/proc/1** - Каталог, содержащий информацию о процессе номер 1. Для каждого процесса существует отдельный каталог в /proc, именем которого является его числовой идентификатор.  
  
**/proc/cpuinfo** - Информация о процессоре, такая как тип процессора, его модель, производительность и др.  
  
**/proc/devices** - Список драйверов устройств, встроенных в действующее ядро.  
  
**/proc/dma** - Задействованные в данный момент каналы DMA.  
  
**/proc/filesystems** - Файловые системы, встроенные в ядро.  
  
**/proc/interruрts** - Задействованные в данный момент прерывания.  
  
**/proc/ioports** - Задействованные в данный момент порты ввода/вывода.  
  
**/proc/kcore** - Отображение физической памяти системы в данный момент. Размер этого файла точно такой же, как и у памяти компьютера, только он не занимает места в самой памяти, а генерируется на лету при доступе к нему программ. Однако при копировании этого файла куда-либо, он не займет места на диске.  
  
**/proc/kmsg** - Сообщения, выдаваемые ядром. Они также перенаправляются в syslog.  
  
**/proc/ksyms** - Таблица символов ядра.  
  
**/proc/loadavg** - Ориентировочная загруженность системы.  
  
**/proc/meminfo** - Информация об использовании памяти, как физической, так и swap-области.  
  
**/proc/modules** - Список модулей ядра, загруженных в данный момент.  
  
**/proc/ne**t - Информация о сетевых протоколах.  
  
**/proc/self** - Символическая ссылка к каталогу процесса, пытающегося получить информацию из /proc. При попытке двух различных процессов получить какую-либо информацию в /proc, они получают ссылки на различные каталоги. Это облегчает доступ программ к собственному каталогу процесса.  
  
**/proc/stat** - Различная статистическая информация о работе системы.  
  
**/proc/uptime** - Время, в течение которого система находится в рабочем состоянии.  
  
**/proc/version** - Версия ядра.

#структура #каталоги