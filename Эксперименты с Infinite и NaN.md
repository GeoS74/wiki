
# Эксперименты с Infinite и NaN

```c
#include<stdio.h>
#include <math.h>

int main(void) {
	double x = 1.0/0.0;
	double y = 0.0/0.0;

	if (isinf(x)) {
		printf("x is infinite\n");
	}
	if (isnan(y)) {
		printf("y is NaN\n");
	}
	
	// не делай так!!!
	if (x) { // выполнится, эквивалентно (infinite != 0.0)
		printf("x is infinite\n");
	}
	if (y) { // выполнится, эквивалентно (NaN != 0.0)
		printf("y is NaN\n");
	}
	
	printf("%f\n", x); //inf
	printf("%f\n", x*0); // -nan
	printf("%f\n", x+1); // inf
	printf("%f\n", y); // -nan
	printf("%f\n", y*0); // -nan
	printf("%f\n", y+1); // -nan
	
	printf("------------\n");
	
	printf("%d\n", x < y); // false
	printf("%d\n", x > y); // false
	printf("%d\n", x <= y); // false
	printf("%d\n", x >= y); // false
	printf("%d\n", x == y); // false
	printf("%d\n", x != y); // true
	
	printf("------------\n");
	
	printf("%d\n", x < x); // false
	printf("%d\n", x > x); // false
	printf("%d\n", x <= x); // true
	printf("%d\n", x >= x); // true
	printf("%d\n", x == x); // true
	printf("%d\n", x != x); // false
	
	printf("------------\n");
	
	printf("%d\n", y < y); // false
	printf("%d\n", y > y); // false
	printf("%d\n", y <= y); // false
	printf("%d\n", y >= y); // false
	printf("%d\n", y == y); // false
	printf("%d\n", y != y); // true
	
	return 0;
}
```