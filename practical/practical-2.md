# Практическая работа № 2
ФИО: Сидоренко Алеся 
Курс: 1 
Группа: ПОО
## Цель работы
Научиться организовывать проекты на языке C, разбивая их на несколько исходных файлов и используя GNU Make для автоматизации сборки. Задания должны помочь будущим учителям информатики: - Освоить принципы модульного программирования. - Научиться составлять Makefile для сборки многофайловых проектов. - Разработать оригинальные учебные примеры, которые можно адаптировать для школьников.
## Задача 1. Система управления студентами
Создайте многофайловый проект, реализующий простейшую систему управления записями студентов. Проект должен состоять как минимум из трёх файлов: - student.h – содержит определение структуры Student и прототипы функций для работы с записями. - student.c – реализует функции для добавления студента, вывода списка студентов и (опционально) сохранения данных в файл. - main.c – содержит функцию main(), где происходит взаимодействие с пользователем (например, добавление и вывод студентов). Также необходимо написать Makefile для автоматической сборки проекта.
### Код программы
##### Файл: student.h
```c
#ifndef STUDENT_H
#define STUDENT_H

// Определение структуры Student
struct Student {
    char name[50]; // Имя студента
    int age;       // Возраст студента
    float grade;   // Средний балл
};

// Прототипы функций для работы со студентом
void addStudent(struct Student *s, const char *name, int age, float grade);
void printStudent(const struct Student *s);

#endif // STUDENT_H
```
##### Файл: student.c
```c
#include <stdio.h>
#include <string.h>
#include "student.h"

// Функция для заполнения данных студента
void addStudent(struct Student *s, const char *name, int age, float grade) {
    // Копируем имя студента в поле name
    strcpy(s->name, name);
    s->age = age;
    s->grade = grade;
}

// Функция для вывода информации о студенте
void printStudent(const struct Student *s) {
    printf("Имя: %s\n", s->name);
    printf("Возраст: %d\n", s->age);
    printf("Средний балл: %.2f\n", s->grade);
}
```
##### Файл: main.c
```c
#include <stdio.h>
#include "student.h"

int main(void) {
    // Создание массива студентов
    struct Student students[3];

    // Заполнение данных студентов
    addStudent(&students[0], "ЧокоПай", 20, 4.44);
    addStudent(&students[1], "ПанкиХой", 22, 4.92);
    addStudent(&students[2], "Мемас", 21, 4.15);

    // Вывод информации о студентах
    for (int i = 0; i < 3; i++) {
        printf("Студент %d:\n", i + 1);
        printStudent(&students[i]);
        printf("\n");
    }

    return 0;
}
```
##### Makefile
```c
CC = gcc
CFLAGS = -Wall -O2
SRC = main.c student.c
OBJ = $(SRC:.c=.o)
TARGET = student_manager

all: $(TARGET)

$(TARGET): $(OBJ)
	$(CC) -o $@ $^ $(CFLAGS)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJ) $(TARGET)

.PHONY: all clean
```
### Пример входных/выходных данных
```
Входные данные (задаются в коде):
Студент 1: "ЧокоПай", 20, 4.44
Студент 2: "ПанкиХой", 22, 4.92
Студент 3: "Мемас", 21, 4.15

Выходные данные (результат выполнения программы):
Студент 1:
Имя: ЧокоПай
Возраст: 20
Средний балл: 4.44

Студент 2:
Имя: ПанкиХой
Возраст: 22
Средний балл: 4.92

Студент 3:
Имя: Мемас
Возраст: 21
Средний балл: 4.15
```
### Инструкция по сборке и запуску
1. Сохраните все файлы в одной директории:
* student.h
* student.c
* main.c
* Makefile
2. Откройте терминал в этой директории и выполните команды:
```
make
./student_manager
```
3. Для очистки скомпилированных файлов выполните:
```
make clean
```
## Задача 2. Игра “Угадай число”
Создайте многофайловый проект, реализующий консольную игру “Угадай число”. Проект должен состоять из следующих файлов: - game.h – содержит прототипы функций игрового модуля. - game.c – реализует функции для генерации случайного числа, проверки догадок и выдачи подсказок. - utils.h и utils.c – (опционально) содержат вспомогательные функции (например, для чтения ввода). - main.c – содержит функцию main(), где происходит взаимодействие с пользователем: генерируется случайное число, пользователь вводит догадки, программа сообщает, что догадка слишком высокая или слишком низкая. Также создайте Makefile для автоматизации сборки проекта.
### Код программы
##### Файл: game.h
```c
#ifndef GAME_H
#define GAME_H

// Генерация случайного числа в диапазоне [lower, upper]
int generateNumber(int lower, int upper);

// Проверка догадки: возвращает -1, если guess меньше target, 0, если равны, 1, если guess больше target
int checkGuess(int target, int guess);

#endif // GAME_H
```
##### Файл: game.c
```c
#include <stdlib.h>
#include <time.h>
#include "game.h"

// Генерация случайного числа в диапазоне [lower, upper]
int generateNumber(int lower, int upper) {
    return lower + rand() % (upper - lower + 1);
}

// Сравнение target и guess
int checkGuess(int target, int guess) {
    if (guess == target) {
        return 0;
    } else if (guess < target) {
        return -1;
    } else {
        return 1;
    }
}
```
##### Файл: main.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include "game.h"

int main(void) {
    int lower = 1, upper = 100;
    int target, guess, result;

    // Инициализация генератора случайных чисел
    srand(time(NULL));
    target = generateNumber(lower, upper);

    printf("Угадайте число от %d до %d:\n", lower, upper);
    
    do {
        printf("Введите вашу догадку: ");
        scanf("%d", &guess);
        result = checkGuess(target, guess);
        
        if (result == -1) {
            printf("Слишком мало!\n");
        } else if (result == 1) {
            printf("Слишком много!\n");
        }
    } while (result != 0);

    printf("Поздравляем! Вы угадали число %d.\n", target);
    return 0;
}
```
##### Makefile
```c
# Makefile для проекта "Игра Угадай число"

CC = gcc
CFLAGS = -Wall -O2
SRC = main.c game.c
OBJ = $(SRC:.c=.o)
TARGET = guess_game

all: $(TARGET)

$(TARGET): $(OBJ)
	$(CC) -o $@ $^ $(CFLAGS)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJ) $(TARGET)

.PHONY: all clean
```
### Пример входных/выходных данных
```
Угадайте число от 1 до 100:
Введите вашу догадку: 50
Слишком мало!
Введите вашу догадку: 75
Слишком много!
Введите вашу догадку: 63
Слишком мало!
Введите вашу догадку: 69
Слишком много!
Введите вашу догадку: 66
Слишком много!
Введите вашу догадку: 64
Поздравляем! Вы угадали число 64.
```
### Инструкция по сборке и запуску
1. Сохраните все файлы в одной директории:
* game.h
* game.c
* main.c
* Makefile
2. Откройте терминал в этой директории и выполните команды:
```
make
./guess_game
```
3. Для очистки скомпилированных файлов выполните:
```
make clean
```