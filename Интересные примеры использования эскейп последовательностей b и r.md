# Интересные примеры использования эскейп последовательностей b и r

Скомпилируй, запусти и наслаждайся!

### Листинг 1

```c
#include <stdio.h
#include <unistd.h> // для sleep

int main() {
	printf("=== ДЕМО \\b (backspace) ===\n\n");
	// Пример 1: Простое затирание
	printf("1. Стирание: ABC\\bD → ");
	printf("ABC");
	fflush(stdout);
	sleep(1);
	printf("\b"); // курсор возвращается на 'C'
	fflush(stdout);
	sleep(1);
	printf("D");
	fflush(stdout);
	sleep(1);
	printf("\n Результат: ABD (C заменена на D)\n\n");
	
	// Пример 2: Анимация
	printf("2. Анимация загрузки: ");
	fflush(stdout);
	char spinner[] = "|/-\\";
	for (int i = 0; i < 10; i++) {
		printf("\b%c", spinner[i % 4]);
		fflush(stdout);
		usleep(200000); // 0.2 сек
	}
	printf("\b \n\n");
	
	// Пример 3: Прогресс в одной строке
	printf("3. Счётчик: ");
	for (int i = 1; i <= 5; i++) {
		printf("\b%d", i);
		fflush(stdout);
		sleep(1);
	}
	
	printf("\n\n");
	return 0;
}
```

### Листинг 2

```c
#include <stdio.h>
#include <unistd.h>

int main() {
	printf("=== ДЕМО \\r (carriage return) ===\n\n");
	
	// Пример 1: Перезапись строки
	printf("1. Перезапись: Первая строка\\rВторая →\n ");
	printf("Первая строка");
	fflush(stdout);
	sleep(2);
	printf("\rВторая строка \n");
	printf(" (Первая строка полностью заменена)\n\n");
	
	// Пример 2: Прогресс-бар
	printf("2. Прогресс-бар:\n");
	printf(" [ ] 0%%");
	fflush(stdout);
	for (int i = 0; i <= 100; i += 5) {
		usleep(100000); // 0.1 сек
		printf("\r [");
		int bars = i / 10;
		for (int j = 0; j < bars; j++) printf("#");
		for (int j = bars; j < 10; j++) printf(" ");
		printf("] %3d%%", i);
		fflush(stdout);
	}
	printf("\n\n");
	
	// Пример 3: Часы
	printf("3. Часы (5 секунд):\n");
	for (int sec = 0; sec <= 5; sec++) {
		printf(" Время: %02d:%02d", sec / 60, sec % 60);
		fflush(stdout);
		if (sec < 5) {
			sleep(1);
			printf("\r");
		}
	}
	printf("\n\n");
	return 0;
}
```

### Листинг 3

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
	printf("=== АНИМАЦИЯ С \\r и \\b ===\n\n");
	
	// Бегущая строка
	char message[] = "Hello, World! ";
	int len = strlen(message);
	printf(" Бегущая строка: ");
	fflush(stdout);
	for (int i = 0; i < 20; i++) {
		for (int j = 0; j < len; j++) {
			printf("%c", message[(i + j) % len]);
		}
		fflush(stdout);
		usleep(200000); // 0.2 сек
		printf("\r Бегущая строка: ");
		for (int j = 0; j < len; j++) {
			printf(" ");
		}
		printf("\r Бегущая строка: ");
		fflush(stdout);
	}
	printf("%s\n\n", message);
	return 0;
}
```

### Листинг 4

```c
#include <stdio.h>
#include <unistd.h>

int main() {
	printf("=== ИНТЕРАКТИВНОЕ МЕНЮ ===\n\n");
	int choice = 1;
	char *options[] = {"Выйти", "Создать", "Открыть", "Сохранить"};
	int num_options = 4;
	printf("Используйте стрелки вверх/вниз, Enter для выбора\n");
	printf("(Здесь стрелки эмулируются клавишами +/-)\n\n");
	while (1) {
	
		// Печать всех опций
		for (int i = 0; i < num_options; i++) {
			if (i + 1 == choice) {
				printf(" -> %s\n", options[i]);
			} else {
				printf(" %s\n", options[i]);
			}
		}
		
		// Эмуляция ввода (в реальной программе был бы getch())
		printf("\nНажмите +/- для выбора, Enter для подтверждения: ");
		fflush(stdout);
		
		// Вместо реального ввода - демо
		sleep(1);
		if (choice < num_options) {
			choice++;
		} else {
			choice = 1;
		}
		
		// Возврат к началу меню
		printf("\r\033[%dA", num_options + 2); // ANSI escape - вверх на N строк
	}
	return 0;
}
```


### Листинг 5

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <time.h>

int main() {
	printf("=== ПРИБЛИЖАЮЩИЙСЯ ПРИШЕЛЕЦ ===\n\n");
	srand(time(NULL));
	int alien_pos = 20; // стартовая позиция
	int shot_pos = -1; // позиция выстрела (-1 = нет выстрела)
	printf("Цельтесь пробелом, стреляйте Enter!\n");
	printf("(Здесь автоматическая демонстрация)\n\n");
	printf("Космический корабль: V\n");
	printf("Пришелец: @\n\n");
	for (int frame = 0; frame < 30; frame++) {
		// Очистка экрана (упрощённо)
		printf("\033[4A\r"); // вверх на 4 строки
		// Движение пришельца
		alien_pos += (rand() % 3) - 1; // -1, 0, или 1
		if (alien_pos < 0) alien_pos = 0;
		if (alien_pos > 40) alien_pos = 40;
		// Движение выстрела
		if (shot_pos >= 0) {
			shot_pos++;
			if (shot_pos > 40) shot_pos = -1;
		}
		// Случайный выстрел для демо
		if (frame == 10) shot_pos = 0;
		// Отрисовка
		printf("Космический корабль: V\n");
		printf("Пришелец: @\n\n");
		// Поле боя
		printf(" ");
		for (int i = 0; i <= 40; i++) {
			if (i == 0 && shot_pos >= 0) {
			printf("*"); // выстрел
			} else if (i == alien_pos) {
			printf("@"); // пришелец
			} else if (i == shot_pos) {
			printf("*"); // выстрел в полёте
			} else {
			printf(" ");
			}
		}
		printf("\n");
		// Проверка попадания
		if (shot_pos == alien_pos) {
			printf("\n\nПОПАДАНИЕ! Пришелец уничтожен!\n");
			break;
		}
		usleep(200000); // 0.2 сек за кадр
	}
	printf("\n");
	return 0;
}
```