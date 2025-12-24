# Windows


##### Для запуска photorec из под linux (флешка Kaspersky Recovery Disk)

```bash
sudo apt-get update && sudo apt-get install testdisk -y
sudo photorec
```

##### Для работы с дисками через стороннюю утилиту

Minitool partition wizard - прога для работы с дисками из под винды


##### Для работы с дисками через командную строку

```cmd
diskpart
list disk
select disk 0        (убедитесь, что это SSD!)

clean                (1. Полная очистка)

convert gpt          (2. Конвертация в GPT/MBR)

create partition primary  (3. Создание одного раздела на весь диск)
format fs=ntfs quick label="Windows"
assign letter=C

list volume          (4. Проверить результат)
exit
```

##### Для работы с дисками стандартную утилиту

запустить CMD от админа

запустить diskmgmt.msc

#windows