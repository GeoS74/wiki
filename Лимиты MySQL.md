# Лимиты MySQL

Настройки для изменения лимитов при работе с [[MySQL]]

### php.ini
- upload_max_filesize — максимальный размер загружаемого файла
- post_max_size — максимальный размер сообщения методом POST

### для XAMPP
в файле: xampp\phpMyAdmin\libraries\config.default.php
- `$cfg['ExecTimeLimit'] = 10000;` - максимальное время для импорта

### Ubuntu  
найти директорию с phpmyadmin в [[Ubuntu]]: `find / -iname "phpmyadmin*" -type d`