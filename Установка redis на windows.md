# Установка redis на windows

[Взято отсюда](https://skillbox.ru/media/base/kak_ustanovit_redis_v_os_windows_bez_ispolzovaniya_docker/?topic=base&section=kak_ustanovit_redis_v_os_windows_bez_ispolzovaniya_docker)


Для установки [[Redis]] на Windows используется пакетный менеджер chocolatey.

Для установки `chocolatey` надо запустить powershell от администратора и выполнить:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Установка [[Redis]]

```powershell
// установка определённой версии
choco install redis-64 --version 3.0.503

// установка последней версии
choco install redis-64
```


Если всё прошло успешно, то файлы утилит [[Redis]] появятся в директории:
`C:/ProgramData/chocolatey/lib/redis-64`

В неё можно расположить файл `redis.conf` конфигурации Redis и запустить сервер БД указав вторым аргументом путь до конфигурации. В этом примере запуск выполняется из директории `C:/ProgramData/chocolatey/lib/redis-64`:

```powershell
redis-server redis.conf
```

Аналогичным образом запускается клиент:

```powershell
redis-cli -h localhost -p 6385

// после запуска клиента если требуется ввести пароль, то выполнить
AUTH {secret_pass}
// взамен {secret_pass} указать пароль
```



#redis