# AIC303 C Coding Style Guide

## 1. Purpose

This document defines the preferred C coding style for AIC303 programming assignments and projects.

The style is based primarily on the conventions used by the Linux kernel, adapted for the course. The goal is to make code:

- easy to read and review;
- consistent across assignments and projects;
- straightforward to debug and maintain;
- clear enough that the structure of a program can be understood quickly.

These rules are guidelines for writing professional-quality C code. Unless an assignment explicitly says otherwise, code submitted for AIC303 should follow this guide.

## 2. Indentation

### 2.1 Use tabs for indentation

When writing user applications use **4-character** tabs. If you write kernel code, then use **8-character** tabs.

Do not use spaces for indentation.

Example:

```c
if (condition) {
    do_something();
}
```

The example above represents one tab of indentation, displayed as four spaces.

Configure your editor so that:

- indentation uses tabs;
- a tab is displayed as 4 columns (or 8 columns when writing kernel code);
- indentation is not automatically converted to spaces.

### 2.2 Do not mix tabs and spaces for indentation

Avoid code such as:

```text
<TAB><spaces>statement;
```

Use tabs for structural indentation and spaces only where they are part of normal syntax or alignment.

### 2.3 Keep nesting under control

Deeply nested code is difficult to read. Prefer early returns, helper functions, or a restructuring of the algorithm when a function develops many levels of nesting.

For example:

```c
if (!valid) {
    return -1;
}

if (ready) {
    process();
}
```

is generally preferable to repeatedly nesting additional blocks.


## 3. Line Length

The preferred maximum line length is **80 columns**.

Lines longer than 80 columns should normally be broken into sensible pieces.

For example:

```c
result = calculate_result(first_value, second_value,
                          third_value, fourth_value);
```

When breaking a long expression, continuation lines should be clearly separated from the surrounding code.

Do not break strings merely to satisfy the line-length limit. When doing so would make a user-visible message difficult to search for or understand.


## 4. Braces

AIC303 follows the Linux kernel / K&R brace style.

### 4.1 Control statements

Opening braces go on the same line as the statement. For example:

```c
if (condition) {
    do_something();
}
```

The same convention applies to:

```c
if (condition) {
    ...
} else {
    ...
}
```

```c
while (condition) {
    ...
}
```

```c
for (i = 0; i < count; i++) {
    ...
}
```

```c
switch (value) {
case 1:
    do_one();
    break;
case 2:
    do_two();
    break;
default:
    break;
}
```

### 4.2 Function definitions

Functions are the important exception: the opening brace goes on the **next line**.

Write:

```c
int calculate_sum(int a, int b)
{
    return a + b;
}
```

Do not write:

```c
int calculate_sum(int a, int b) {
    return a + b;
}
```

### 4.3 Single statements

Braces are not required for a simple single statement:

```c
if (error)
    return -1;
```

However, braces should be used when they improve clarity or when both branches of a conditional contain blocks:

```c
if (condition) {
    do_this();
    do_that();
} else {
    do_something_else();
}
```

When adding a second statement would otherwise make the meaning ambiguous, use braces.

### 4.4 `do` / `while`

Use the standard form:

```c
do {
    process_item();
} while (condition);
```


## 5. Spaces

Use spaces consistently around binary and ternary operators.

Write this:

```c
x = a + b;
result = value * 2;
if (x == y)
    return 1;
```

not this:

```c
x=a+b;
result = value*2;
if (x==y)
    return 1;
```

Do not put unnecessary spaces inside parentheses.

Write:

```c
if (x == y)
    do_something();
```

Do not write:

```c
if ( x == y )
    do_something();
```

Use a space after C keywords such as:

```c
if (condition)
switch (value)
for (i = 0; i < count; i++)
while (condition)
```

Function calls do not have a space before the opening parenthesis.

Write:

```c
printf("Hello\n");
calculate_result();
```

Do not write:

```c
printf ("Hello\n");
calculate_result ();
```

### 5.1 Pointer declarations

Place `*` next to the variable name:

```c
char *name;
int *values;
struct student *student;
```

For multiple pointer levels:

```c
char **argv;
```

Do not write:

```c
char* name;
int* values;
```

### 5.2 Unary operators

Do not put spaces between unary operators and their operands:

```c
i++;
--count;
!valid;
*ptr;
&value;
```

Likewise, do not put spaces around `.` and `->`:

```c
student.name
student->name
```

## 6. Naming

Use descriptive names that make the purpose of a variable or function clear.

### 6.1 Variables and functions

Use `snake_case`:

```c
int student_count;
int calculate_average(void);
void print_student_info(void);
```

Avoid mixed-case names:

```c
int studentCount;
int calculateAverage(void);
```

### 6.2 Local variables

Local variables may be short when their meaning is obvious:

```c
int i;
int j;
int n;
char *ptr;
```

Use descriptive names when a short name would be unclear:

```c
int student_count;
double average_grade;
```

### 6.3 Functions

Function names should describe what the function does and always start with 
a lower-case letter:

```c
int calculate_average(void);
void print_student(void);
bool is_valid_student(const struct student *student);
```

Avoid vague names such as:

```c
void do_it(void);
int process(void);
```

unless their meaning is genuinely obvious from context.

### 6.4 Constants and macros

Use uppercase `SNAKE_CASE` for constants and preprocessor macros:

```c
#define MAX_STUDENTS 100
#define BUFFER_SIZE 256
```

Prefer `const` variables or enums where they provide a better representation than a macro.

### 6.5 Boolean values

Use `bool` for Boolean values:

```c
bool valid;
bool found;
bool is_registered;
```

Include `<stdbool.h>` when required by the language standard/compiler being used.

Prefer descriptive Boolean names such as:

```c
bool is_valid;
bool has_error;
bool found;
```

## 7. Types and `typedef`

Prefer standard C types and explicit structures.

For example:

```c
struct student {
    char name[64];
    int id;
    double average;
};
```

Use:

```c
struct student student;
```

rather than unnecessarily hiding the structure behind a typedef:

```c
typedef struct student student_t;
```

A `typedef` should have a clear purpose. Do not create typedefs merely to make ordinary C types look shorter.

Standard integer types such as `uint32_t` and `int64_t` may be used when the exact integer width matters:

```c
uint32_t flags;
int64_t offset;
```

Include the appropriate standard header, such as `<stdint.h>`.

## 8. Variable Declarations

Declare local variables at the top of a block, **before any statements**.
Declarations may appear in any block, not just at function scope.

Sort declarations **by size** (largest to smallest), then **alphabetically**.
Several variables of the same type may share a line. If the line overflows,
repeat the type keyword on the next line.

Variables may be initialized where declared, especially when constant for the
rest of the scope. Do not call complicated functions in initializers.

**One blank line** between the declarations and the first statement. If there
are no declarations, the first line is a statement. Do not declare functions 
inside other functions.

Use one declaration per line:

```c
char *buffer;  // sizeof(char *) > sizeof(int)
int count;
int total;
```

rather than:

```c
int count, total;
char *buffer, *name;
```

Initialize variables when practical:

```c
int count = 0;
bool found = false;
```

Avoid declaring variables far away from where they are first used.

Keep declarations easy to associate with their purpose.


## 9. Functions

Functions should be relatively short and should generally perform **one logical task**.

If a function becomes complicated, consider extracting part of its work into a helper function.

For example, instead of one large function that:

1. reads input;
2. validates input;
3. calculates a result;
4. formats output; and
5. writes a file;

consider separate functions:

```c
int read_input(...);
bool validate_input(...);
int calculate_result(...);
void format_result(...);
int write_output(...);
```

This makes each part easier to test and understand.

### 9.1 Function prototypes

Include parameter names in function prototypes:

```c
int calculate_average(const int *grades, size_t count);
```

rather than:

```c
int calculate_average(const int *, size_t);
```

Parameter names provide useful information to readers.

### 9.2 Function definitions

Use the following form:

```c
int calculate_average(const int *grades, size_t count)
{
    ...
}
```

Every function is prototyped somewhere. Prototypes for private functions go
at the top of the source file, and those functions are declared static.
Prototypes include parameter names. Do not use extern on function
declarations.

Preferred prototype order: storage class, return type, name, parameters.

For example:

```c
static inline int clamp_int(int value, int low, int high);

size_t read_samples(FILE *fp, float *buf, size_t max_samples);
```

Variable-argument functions:

```c
#include <stdarg.h>

void vaf(const char *fmt, ...)
{
	va_list ap;

	va_start(ap, fmt);
	/* use ap */
	va_end(ap);
}
```

Usage messages mirror the manual-page synopsis, in this order: options without
operands (alphabetical, in one set of brackets), options with operands (each in
its own brackets), required arguments, then optional arguments. `|` separates
either-or choices.

```c
(void)fprintf(stderr, "usage: f [-aDde] [-b b_arg] req1 req2 [opt1 [opt2]]\n");
(void)fprintf(stderr, "usage: f [-a | -b] [-c [-dEe] [-n number]]\n");
exit(1);
```

Run code through a static checker and compile with `-Wall -Wextra`; code should
build with minimal (ideally zero) warnings.

Separate functions with one blank line.

## 10. Comments and Documentation

Comments should explain **what the code means or why a non-obvious decision was made**, rather than restating obvious C syntax.

Prefer clear code over comments that explain complicated code unnecessarily.

For example, this comment adds little value:

```c
/* Increment i by one. */
i++;
```

A useful comment explains a non-obvious reason:

```c
/*
 * Skip the first entry because it contains the header rather than
 * a student record.
 */
i++;
```

### 10.1 Multi-line comments

Use Linux-style C comments:

```c
/*
 * This is a multi-line comment.
 * Each line starts with an asterisk.
 */
```

Avoid large blocks of decorative comment characters.

### 10.2 Function documentation

Public functions and functions whose interface or behavior is important should use **Doxygen-style documentation**.

Use `@brief`, `@param`, `@return`, and other appropriate Doxygen tags.

Example:

```c
/**
 * @brief Calculate the arithmetic mean of an array of grades.
 *
 * @param grades Array containing the grades.
 * @param count Number of grades in the array.
 *
 * @return The calculated average, or -1.0 if count is zero.
 */
double calculate_average(const int *grades, size_t count)
{
    ...
}
```

For a function that does not return a value:

```c
/**
 * @name print_student
 * @brief Print information about a student.
 *
 * @param student Student whose information should be printed.
 */
void print_student(const struct student *student)
{
    ...
}
```

For a function with no parameters:

```c
/**
 * @name print_menu
 * @brief Display the program's main menu.
 */
void print_menu(void)
{
    ...
}
```

You may also add more details about a function, and document every parameter, in signature order, 
with `[in]`, `[out]`, or `[in,out]`. `[in]` means that the parameter is used for input, `[out]` means that
a parameter is used for output, `[in,out]` means that the parameter is used for both input and
output.

```c
/**
 * @brief Computes the mean value.
 *
 * @details
 * Samples that are NaN are skipped and not counted. Runs in _O(count)_ time
 * and does not allocate memory.
 *
 * @param[in] samples Array of values. Must not be `NULL`.
 * @param[in] count Number of elements in `samples`.
 * @param[out] mean  Receives the mean. Unchanged on error.
 * @retval 0 Success.
 * @retval -EINVAL Invalid arguments.
 */
int compute_mean(const float *samples, size_t count, float *mean)
{
	float sum = 0.0f;
	size_t i, valid = 0;

	if (samples == NULL || mean == NULL || count == 0)
		return (-EINVAL);

	for (i = 0; i < count; i++) {
		if (isnan(samples[i]) != 0)
			continue;
		sum += samples[i];
		valid++;
	}

	*mean = sum / valid;
	return (0);
}
```

### 10.3 Document parameters accurately

The documentation should describe what each parameter means, not merely repeat its type.

Good:

```c
/**
 * @param count Number of elements in the grades array.
 */
```

Less useful:

```c
/**
 * @param count An integer count.
 */
```

### 10.4 Document return values

Use `@return` to describe the meaning of the returned value:

```c
/**
 * @return 0 on success, or a negative value on error.
 */
```

If a function has multiple meaningful return values, document their meanings clearly.

### 10.5 Do not over-document

Do not add documentation that simply repeats an obvious function signature.

The purpose of Doxygen comments is to communicate the function's **contract, behavior, assumptions, and important effects**.



## 11. `if`, `else`, and Comparisons

Write conditions clearly:

```c
if (count == 0)
    return -1;
```

Avoid unnecessarily complicated expressions:

```c
if ((count == 0 && !initialized) || (count > 100 && !forced))
    ...
```

If a condition becomes difficult to understand, introduce a well-named Boolean variable or helper function:

```c
bool invalid_count = count == 0 || count > MAX_COUNT;

if (invalid_count)
    return -1;
```

Do not compare Boolean values unnecessarily:

```c
if (valid)
    ...
```

is preferable to:

```c
if (valid == true)
    ...
```

unless the explicit comparison improves clarity in a particular context.


## 12. `switch` Statements

Do not unnecessarily indent `case` labels.

Use:

```c
switch (command) {
case COMMAND_START:
    start();
    break;
case COMMAND_STOP:
    stop();
    break;
case COMMAND_EXIT:
    return 0;
default:
    report_invalid_command();
    break;
}
```

Keep each case easy to identify.

If a case contains multiple statements, indentation makes those statements subordinate to the case:

```c
switch (value) {
case 1:
    initialize();
    process();
    break;
case 2:
    reset();
    break;
default:
    break;
}
```

## 13. Loops

Use straightforward loop structures:

```c
for (int i = 0; i < count; i++) {
    process(items[i]);
}
```

For loops that contain only one simple statement, braces may be omitted:

```c
for (int i = 0; i < count; i++)
    process(items[i]);
```

Use braces when the loop body contains multiple statements:

```c
for (int i = 0; i < count; i++) {
    process(items[i]);
    update_statistics();
}
```

Avoid clever loop expressions that make the control flow difficult to understand.

## 14. Error Handling

Check errors as close as practical to the operation that can produce them.

Prefer:

```c
result = read_file(file);

if (result < 0)
    return result;
```

over allowing an error to propagate through several unrelated operations.

Use early returns when they make error handling clearer:

```c
if (file == NULL)
    return -EINVAL;

if (!valid_file(file))
    return -EINVAL;

process_file(file);

return 0;
```

When several error paths require the same cleanup, a centralized cleanup path may be appropriate:

```c
int process_data(void)
{
    int ret;
    char *buffer;

    buffer = allocate_buffer();
    if (buffer == NULL)
        return -ENOMEM;

    ret = process_buffer(buffer);
    if (ret != 0)
        goto out_free_buffer;

    ret = 0;

out_free_buffer:
    free(buffer);

    return ret;
}
```

Use meaningful labels such as:

```c
out_free_buffer:
err_close_file:
out_cleanup:
```

Avoid meaningless labels such as:

```c
err1:
err2:
```

The cleanup mechanism should make ownership and resource lifetime obvious.

## 15. Memory Management

Every dynamically allocated resource must have a clearly identifiable owner and corresponding cleanup.

For example:

```c
char *buffer = (char *) malloc(size);
if (buffer == NULL)
    return -ENOMEM;

process(buffer);

if (buffer != NULL)
    free(buffer);
```

Do not lose the original pointer returned by an allocation.

Check allocation results before dereferencing the pointer:

```c
char *data = (char *) malloc(sizeof(*data));
if (data == NULL)
    return -ENOMEM;
```

Prefer:

```c
data = malloc(sizeof(*data));
```

over:

```c
data = malloc(sizeof(struct data));
```

when allocating an object through its pointer variable. This keeps the allocation tied to the variable's type.


## 16. `sizeof`

Prefer `sizeof` on the object being allocated:

```c
struct student *student;

student = (struct student *) malloc(sizeof(*student));
```

rather than repeating the type:

```c
student = (struct student *) malloc(sizeof(struct student));
```

For arrays, use the array itself when appropriate:

```c
size_t count = sizeof(values) / sizeof(values[0]);
```

Do not hard-code an array's size when the compiler can determine it.


## 17. Preprocessor Directives

Preprocessor directives should begin at the start of the line:

```c
#include <stdio.h>
#include <stdlib.h>

#define BUFFER_SIZE 256
```

Conditional compilation should remain readable:

```c
#ifdef DEBUG
    print_debug_information();
#endif
```

Use include guards in headers:

```c
#ifndef STUDENT_H
#define STUDENT_H

/* declarations */

#endif /* STUDENT_H */
```

For projects using a compiler/environment that supports it, `#pragma once` may be used if permitted by the course.


## 18. Header Files

Header files should contain declarations and interfaces rather than implementation details whenever possible.

A typical header might look like:

```c
#ifndef STUDENT_H
#define STUDENT_H

#include <stdbool.h>
#include <stddef.h>

struct student {
    int id;
    char name[64];
    double average;
};

int student_is_valid(const struct student *student);
double calculate_average(const int *grades, size_t count);

#endif /* STUDENT_H */
```

Include only the headers that are actually required.


## 19. Source File Organization

A typical `.c` file should have a consistent organization.

For example:

```c
/*
 * Module description.
 */

#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

#include "student.h"

static int helper_function(int value);

/**
 * @name calculate_average
 * @brief Calculate the average of an array of grades.
 *
 * @param grades Array of grades.
 * @param count Number of grades.
 *
 * @return The average, or -1.0 if count is zero.
 */
double calculate_average(const int *grades, size_t count)
{
    ...
}

static int helper_function(int value)
{
    ...
}

```

A reasonable order is:

1. file/module comment, when useful;
2. system/library includes;
3. project includes;
4. macros and constants;
5. type definitions;
6. static/global declarations;
7. function prototypes;
8. function definitions.

Keep the organization consistent within a project.


## 20. Global Variables

Avoid global variables, unless there is a clear reason to use them.

Prefer passing data explicitly to functions:

```c
int calculate_total(const int *values, size_t count);
```

rather than relying on global state:

```c
int values[MAX_VALUES];
int value_count;
```

If a global variable is necessary, give it a descriptive name and restrict its visibility with `static`, when it is only used within one source file:

```c
static int student_count;
```

## 21. `const`

Use `const` when a function does not modify data supplied by the caller:

```c
int calculate_sum(const int *values, size_t count);
```

This communicates the function's contract and allows the compiler to enforce it.

Use `const` for values that should not change:

```c
const int max_students = 100;
```

Do not remove `const` merely to make an interface easier to use.


## 22. Magic Numbers

Avoid unexplained numeric constants:

```c
if (count > 100)
    ...
```

Prefer a named constant:

```c
#define MAX_STUDENTS 100

if (count > MAX_STUDENTS)
    ...
```

The name should communicate why the value exists.

Not every number requires a constant. Simple values such as `0`, `1`, and `-1` are often self-explanatory.

## 23. Avoid Clever C

C provides many compact constructs. Compact does not necessarily mean readable.

Prefer:

```c
if (count > 0)
    count--;
```

over overly compressed expressions such as:

```c
count -= count > 0;
```

Similarly, avoid using comma operators, deeply nested ternary expressions, or multiple unrelated operations on one line merely to save space.

The code should make the programmer's intent obvious.


## 24. One Statement Per Line

Put one statement on each line.

Good:

```c
count++;
total += value;
print_result(total);
```

Avoid:

```c
count++; total += value; print_result(total);
```

Do not use commas to combine operations that should be separate statements.


## 25. Whitespace

Do not leave trailing whitespace at the end of lines.

Blank lines should be genuinely blank.

Use blank lines to separate logical sections, but do not insert blank lines between every line of code.

For example:

```c
read_input();

if (validate_input()) {
    process_input();
    write_output();
}
```

is easier to read than a function filled with unnecessary blank lines.


## 26. Example: Complete Function

The following example demonstrates the main conventions in this guide:

```c
/**
 * @brief Calculate the average of an array of integer values.
 *
 * @param values Array containing the values.
 * @param count Number of values in the array.
 *
 * @return The average value, or -1.0 if count is zero.
 */
double calculate_average(const int *values, size_t count)
{
    long total = 0;

    if (values == NULL || count == 0)
        return -1.0;

    for (size_t i = 0; i < count; i++)
        total += values[i];

    return (double)total / count;
}
```

## 27. Example: Header file

```c
#ifndef STUDENT_H
#define STUDENT_H

#include <stdbool.h>
#include <stddef.h>

struct student {
    int id;
    char name[64];
    double average;
};

/**
 * @brief Check whether a student record contains valid data.
 *
 * @param student Student record to validate.
 *
 * @return true if the student is valid, otherwise false.
 */
bool student_is_valid(const struct student *student);

/**
 * @brief Calculate the average of an array of grades.
 *
 * @param grades Array containing the grades.
 * @param count Number of grades.
 *
 * @return The average grade, or -1.0 if count is zero.
 */
double calculate_average(const int *grades, size_t count);

#endif /* STUDENT_H */
```

## 28. Submission checklist

Before submitting C code for AIC303, check the following:

- [ ] Indentation uses **tabs displayed as 4 characters for user-level code or 5 characters for kernel code**.
- [ ] Spaces are not used for structural indentation.
- [ ] Lines are preferably no longer than 80 columns.
- [ ] Control-statement braces follow K&R style.
- [ ] Function opening braces are on the next line.
- [ ] Pointer declarations use `type *name`.
- [ ] Variables and functions use `snake_case`.
- [ ] Constants and macros use `UPPER_CASE`.
- [ ] Functions have a clear and focused purpose.
- [ ] Function prototypes include parameter names.
- [ ] Important functions have Doxygen documentation.
- [ ] Doxygen comments use appropriate tags such as `@brief`, `@param`, and `@return`.
- [ ] Each variable declaration is on its own line.
- [ ] Errors are checked appropriately.
- [ ] Dynamically allocated memory is released.
- [ ] `const` is used where appropriate.
- [ ] There is no unnecessary global state.
- [ ] There is no trailing whitespace.
- [ ] There are no unnecessarily clever expressions.
- [ ] Comments explain intent or non-obvious decisions rather than obvious syntax.
- [ ] The code is consistent with the surrounding project.


## 28. Summary

Following are the key rules for styling C source code:

1. Use **4-character or 8-character tabs** for indentation.
2. Use **K&R braces** for control structures.
3. Put function opening braces on the **next line**.
4. Prefer an **80-column** line length.
5. Use descriptive `snake_case` names.
6. Keep functions focused and reasonably short.
7. Avoid unnecessary typedefs and global variables.
8. Check errors explicitly.
9. Avoid clever or compressed C when straightforward C is clearer.
10. Document important functions with **Doxygen** using tags such as `@name`, `@brief`, `@param`, and `@return`.
11. Write comments that explain **intent and rationale**, not obvious syntax.
12. Keep the code consistently formatted and free of trailing whitespace.

The objective is not to make code look identical for its own sake. The objective is to establish a common 
visual and structural language so that your code can be read, reviewed, debugged, and maintained efficiently.
