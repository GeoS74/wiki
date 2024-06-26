# Завершить процесс в [[Ubuntu]]

Для зависшего процесса сначала надо узнать его pid, а затем вызвать команду `kill`. Но, не всё так просто... Если завис процесс ноды (ctrl + z), то узнав его pid, завершить командой `kill` может не получиться. Т.к. команда `kill` без параметров по умолчанию посылает сигнал SIGTERM (от слова termination — завершение), который имеет номер 15. Для завершения надо передать сигнал SIGKILL, под номером 9, для закрытия:
  ```
  $ kill pid -9
  ```
   Вывести список всех сигналов:
   ```
   $ kill -l
   ```
   
   Для [[Узнать pid, запущенного процесса|поиска pid зависшего процесса]] можно использовать команду `pidof`. Если надо программа запустила несколько процессов и надо закрыть сразу все, то можно сделать так:
   ```
   $ kill -9 $(pidof firefox)
   ```
   
   Или перечислять pid процессов вручную:
   ```
   $ kill -9 6589 1254 5885 9578 5489
   ```
  
   
   [Подробнее про kill](https://pingvinus.ru/note/ps-kill-killall)
 