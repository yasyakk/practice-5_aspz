# Практична робота №5

## Дослідження впливу ASLR та PIE на розміщення памʼяті процесу в Linux

## Мета роботи
Дослідити, як механізм ASLR (Address Space Layout Randomization) та режим компіляції PIE (Position Independent Executable) впливають на розміщення стеку, купи, коду програми та бібліотек у памʼяті процесу.


### Створення:

nano aslr_test.c

### Код:

#include <stdio.h>

#include <stdlib.h>

void print_addresses() {

int stack_var = 42;

void *heap_var = malloc(16);

printf("=== Memory addresses ===\n");

printf("Stack address: %p\n", (void*)&stack_var);

printf("Heap address:  %p\n", heap_var);

printf("Function addr: %p\n", (void*)&print_addresses);

printf("Libc printf:   %p\n", (void*)&printf);

free(heap_var);

}

int main() {

print_addresses();

return 0;

}

### Запуск програми:

#### Перевірка стану ASLR:

cat /proc/sys/kernel/randomize_va_space

#### Компіляція з PIE:

gcc aslr_test.c -o aslr_pie

file aslr_pie

#### Запуск при увімкненому ASLR:

./aslr_pie
./aslr_pie
./aslr_pie

#### Вимкнення ASLR:

sudo sysctl -w kernel.randomize_va_space=0

cat /proc/sys/kernel/randomize_va_space

#### Повторний запуск:

./aslr_pie

./aslr_pie

./aslr_pie

#### Компіляція без PIE:

gcc -no-pie aslr_test.c -o aslr_nopie

file aslr_nopie

#### Запуск без PIE при вимкненому ASLR:

./aslr_nopie

./aslr_nopie

./aslr_nopie

#### Повернення ASLR:

sudo sysctl -w kernel.randomize_va_space=2

## Висновки

1. При увімкненому ASLR і PIE всі області памʼяті мають випадкові адреси при кожному запуску.
2. При вимкненому ASLR, але з PIE, стек і купа стають фіксованими, проте код програми та бібліотеки залишаються випадковими.
3. При вимкненому ASLR і без PIE всі адреси памʼяті є фіксованими.
4. PIE є ключовим механізмом, який дозволяє випадкову адресацію коду навіть без ASLR.


## Скріншоти виконання

Усі скріншоти виконання практичної роботи знаходяться в папці:

[screenshots](./screenshots)
