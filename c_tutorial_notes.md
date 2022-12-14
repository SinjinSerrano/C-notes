# C Tutorial

## [hello.c](https://github.com/MarcAtenCSUCI/c-tutorial-SinjinSerrano/#hello-world-in-c-and-the-the-git-workflow)
### Goals
* run program
* run program (debug)
* run program (Valgrind)

All run options are available from the IDE's `Run` menu.

## [types.c](https://github.com/MarcAtenCSUCI/c-tutorial-SinjinSerrano/#task-2)

### Format Specifiers
Specifier | Data Type
--------- | ---------
%i | int
%d | int
%c | char
%f | float
%s | string

### `printf`
Outputs text to the screen.
Variables from memory can be displayed by passing additional arguments.

```
printf("CSUCI has a ratio of %.1f", ratio);
```
* `%.1f`: Prints a float with a precision of 1 digit following the decimal point.

### `scanf`
Reads input into a specified variable.

```
int myInt;
scanf("%d", &myInt);
```
* `%d`: Reads a decimal interger value, stores its value in the `myInt` variable's memory address.
* `&`: Address operator. Used here to get the memory address of `myInt`.

### Goals
* What do the `"%d"` format strings in the `scanf` and `printf` calls mean?
* What does the `&` operator in the `scanf` call do?
* What format string would you use to print a double with 1 decimal point precision? How about a float?

## [if-then-else.c](https://github.com/MarcAtenCSUCI/c-tutorial-SinjinSerrano/#task-3)

### Preprocessors
Includes the following libraries, `<stdio.h>` being an important one found in many programs involving any sort of input or output.
```
#include <stdio.h>
#include "ctype.h"
```
We can also define constants during preprocessing, very useful for defining magic numbers.
```
#define DANGER_LEVEL 5
```
> This substitutes all apperances of `DANGER_LEVEL` in the program with `5` during compilation.

### `puts(const char *s)`
Writes the string `s` AND a terminating newline character to the stream `stdout`.
> What is the difference between `puts` and `printf`?
* `printf` outputs a *formatted* string with a variable number of potential arguments. Specifically, it formats and prints the arguments given.

### `getchar()`
Gets the next character or word from the input string, and returns it.
```
char level;
level = getchar(); // notice we do not use the address operator here.
```
> What is the difference between `scanf("%c", level)` and `getchar()`?
* `scanf` takes two arguments, the format specifier and memory addrses, while `getchar` returns the first character or word.

### `tolower(int c)`
Converts an upper-case letter to the corresponding lower-case letter.

## [array.c](https://github.com/MarcAtenCSUCI/c-tutorial-SinjinSerrano/#task-4)
This program will take an input of a variable number of `double` data types and compute the average.

```
double number[MAX_NUMBER_OF_CELLS];
int userDefinedSize = 0;
double sum = 0.0;
```
> We statically allocate enough memory for the `number` array to support up to the maximum number of cells.

```
 // Get number of doubles from user
printf("Input desired number of doubles: ");
scanf("%d", &userDefinedSize);
if (userDefinedSize < DEFAULT_NUM_OF_CELLS) {
    userDefinedSize = DEFAULT_NUM_OF_CELLS;
    printf("! Number of inputs below 1; defaulted to 1.");
} else if (userDefinedSize > MAX_NUM_OF_CELLS) {
    printf("! Number of inputs too large, defaulted to 128.");
}
```
> Basic data validation is performed here, informing the user of the error and compensation.

```
// Get doubles from user
printf("Input doubles:\n");
int index = 0;
while (index < userDefinedSize) {
    scanf("%.2f", number + index);
    index += 1;
}
```
Recall that `scanf` takes a _memory address_ as its second positional argument. Arrays by default return a **memory address** when called. Thus, by adding the memory size of our index (offset starting at zero, then increasing to one int, then two ints...), we are able to update the array at the proper address.

```
// Print data output and average
printf("Data: ");
for (int index = 0; index < userDefinedSize; index += 1) {
    printf(" %.2f", number[index]);
    sum += number[index];
}
```
Basic for loop to sum and average the data.

```
printf("\nAverage: %.2f", sum / userDefinedSize);
```

## [strings.c, pointer.c, mem-alloc.c](https://github.com/MarcAtenCSUCI/c-tutorial-SinjinSerrano/#task-5)

### `strings.c`
```
char msg[10];          /* array of 10 chars */
char *p;               /* pointer to a char */
char msg2[] = "Hello"; /* msg2 = 'H''e''l''l''o''\0' */
```

Breakpoint A:
```
p = "Bonjour"; /* address of "Bonjour" goes into p */
```
* `msg` is filled with null characters.
* `p` takes the memory address of "Bonjour".
* `msg2` is a six character array containing "Hello" and a null terminator.

Breakpoint B:
```
p = msg;
```
* `p` takes the memory address of array `msg`; both are pointing at the same array.

Breakpoint C:
```
p[0] = 'H', p[1] = 'i', p[2] = '\0';
```
* **Both** `p` and `msg` read "Hi", since they share the same memory address.

### `pointer.c`

Breakpoint A:
```
int j = 1;
```
* `j` is an interger with value `1`.

Breakpoint B:
```
int *ptr = &j;
```
* `ptr` is a pointer that contains the address of `j`.

Breakpoint C:
```
*ptr = 4;
```
* `ptr` points to a memory address, which as a result of the previous line, now contains the value `4`. The unary operator, `*`, is used here to **dereference** the pointer's value. Because `ptr` and `j` share the same address, `j`'s value also becomes `4`.

Breakpoint D:
```
j = *ptr;
```
* No changes to addresses or values are made. The unary operator dereferences `ptr` to read as `4`, and j's value is already equal to `4`.
 

### `mem-alloc.c`
`malloc(size_t size)`
* Allocates `size` bytes memory to be used with any data type and returns a pointer to the newly allocated memory.

`free(void *ptr)`
* Frees allocations created with allocation functions.

```
int *ptr;
```
* Initializes a pointer to an integer called `ptr`.

```
ptr = (int *) malloc(sizeof(int));
```
* Makes `ptr` store the address of a newly allocated integer.
* `(int *)` casts the pointer returned by `malloc()` to type int, matching it to the previous declaration above.

```
*ptr = 4;
```
* Assigns the memory at the address stored in `ptr` to be value `4`.

```
free(ptr);
```
* Frees the allocated memory. The memory at the address still stores the value `4`.


### `words.c and CMakeLists.txt`
When creating a new file, we must add it to the make list in order to execute it.

> `CMakeLists.txt`
```
add_executable(words src/words.c)
```

```
char* msg[MAXIMUM_SIZE];   // maximum array size
char buffer[BUFFER_SIZE]; // maximum word size
int characters;
int word_count = 0;
int sentinel_reached = 0;
```
* Variable declarations. `buffer` is an array of characters, and is thus `char*`. `msg` is an array of strings, and is thus type `char**`.

```
// Part 1: Get words
printf("Enter words: (enter \"END\" to stop):\n");
while ( (word_count < MAXIMUM_SIZE) && (sentinel_reached == 0)) {
    scanf("%s", buffer);
    characters = strlen(buffer);
    if (characters >= BUFFER_SIZE) {
        // ensure last character is a null terminator
        buffer[BUFFER_SIZE - 1] = 0;
    }
    if (strcmp("END", buffer) == 0) {
        sentinel_reached = 1;
    } else {
        msg[word_count] = malloc(sizeof(char) * (characters + 1));
        strcpy(msg[word_count], buffer);

        word_count++;
    }
}
```
* We loop word input until the `msg` array fills up or the terminating `sentinel` is reached.
* `%s` format string: reads series of characters until whitespace is found, inserting `\0` null terminator after the string.
* Since we are setting a buffer size, we must manually insert a null terminator if the word is over capacity.
* `strcmp` to check if it is the sentinel, flagging the loop to end if so.
* Remember that `msg` is an array of pointers, thus we can allocate memory the size of the word, and then pass the pointer to the array.
* We finally copy the word into the array.

```
// Part 2: Print words; free words
printf("The following %d words have been read:\n", word_count);
for (int i = 0; i < word_count; i++) {
    printf("%s", msg[i]);
    printf("\n");
    free(msg[i]);
}
```
* We remember to free the memory that we have previously allocated.

## file.c

```
FILE *input = NULL;
FILE *output = NULL;
char filename[99];
```
* `FILE` streams are pointers to a file, which we initialize as null pointers.
* `filename` is a string up to 99 characters long.

```
printf("Input file name (will overwrite): ");
scanf("%s", filename);
```
* Prompts user to input a filename.

```
input = fopen("data.txt", "r");
output = fpoen(filename, "w");
```
* `r`: Opens a file in read-only mode, with the cursor at beginning of the file.
* `w`: Opens a file in write mode, with the cursor at the beginning of the file.
* `a`: Opens a file in write mode, with the cursor at the end of thee file.

```
if(output == NULL)
    return(1);
if(input == NULL)
    return(1);
```
* checks to make sure the files were opened successfully

```
int pos;
while (!feof(input)) {
    pos = fgetc(input);
    fputc(pos, output);
}
```
* Initialize `pos` to hold the read character.
* `feof(input)` returns `0` while the file's cursor is not at the end of the file.
* `fgetc(input)` copies a character from the file stream, and advances the cursor.
* `fputc(pos, output)` copies the character `pos` and writes it into the `output` file stream.

```
fclose(input);
fclose(output);
```
* Closes these file streams.

## ptr-arg-2.c
```
void swapIntegers(int *n1, int *n2) {
    int temp;

    temp = *n1;
    *n1 = *n2;
    *n2 = temp;
}
```
* Implementation of a swap function using pointers.
* Function accepts two `int *` data types, being pointers (memory addresses).
* The function dereferences those addressees to get their value.

```
int num1 = 5, num2 = 10;
swapIntegers(&num1, &num2);
```
* Use case.
* `&` is used to get the memory address of these variables.

```
void swapStrings(char *s1, char *s2) {
    char* temp = *s1;

    *s1 = *s2;
    *s2 = temp;
}
```
* Function takes arguments type `char *`, or, character pointers.
* `*s1` and `*s2` in the function body dereference character pointers, giving the memory address of the strings.

```
char* word1 = "Hello";
char* word2 = "World";
swapStrings(&word1, &word2);
```
* We get the address of the `char*` words, and pass them to the function.

## ptr-func.c

```
void myCaller(void (*f)(int), int param) {
    (*f)(param);
}
```
* Simply calls function `*f` with the provided `param`.

```
void myProc(int d) {
    printf("Hello world\n");
}
```
* Basic function.

```
int main(void) {
    myCaller(myProc, 10);
    return 0;
}
```
* As shown here, it is actually incredibly simple to supply function pointers as arguments.

## calc.c
### An important tidbit about format strings:
> The format string may also contain other characters. White space (such as blanks, tabs, or newlinese) in the format string match any amount of white space, including **none**, in the input. Everything else mathches only itself.s

```
int add(int, int);
int sub(int, int);
int mult(int, int);
int divi(int, int);;

int calc(int (*)(int, int), int, int);
```
* We will skip the implementations of the operator functions.
* `calc` is a function which:
    * takes a pointer to a `int` type function with `int` and `int` as parameters.

```
int op1;
int op2;
char operator;
int result;

int (*op_ptr)(int, int) = NULL;

while(1) {
    printf("calc > ");
    scanf("%d %c %d", &op1, &operator, &op2);s

    switch(operator) {
        case '+':
            op_ptr = &add;
            break;
        case '-':
            op_ptr = &sub;
            break;
        case '*':
            op_ptr = &mult;
            break;
        case '/':
            op_ptr = &divi;
            break;
        default:
            op_ptr = NULL;
            printf("Invalid operator.\n");
    }

    if (op_ptr != NULL) {
        result = calc(op_ptr, op1, op2);
        printf("%d\n", result);
        op_ptr = NULL;
    }
}
return 0;
```

`scanf("%d %c %d", &op1, &operator, &op2);`
* This elegant format string is white space agnostic!s

## person.h
```
struct person {
    char name[41];
    int age;
    float height;
    struct {
        int month;
        int day;
        int year;
    } birth;
};
```
* Creates fields for `name`, `age`, `height`, and a struct for their `birth`date.

```
typedef struct person PERSON;
```
* Compiler will substitutee `PERSON` with `struct person` in our program.

## person.c
* Not much to cover here.

