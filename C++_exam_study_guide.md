# C++ Exam Study Guide

## Exam Scope

According to the exam notice in `exam.png`, the written exam covers:

- Functions, recursion, pass-by-value, and pass-by-reference
- File I/O
- Containers, iterators, and STL algorithms
- Structs
- Pointers

The notice also says the exam includes both:

- theoretical material from lectures
- practical material from exercises and labs

This guide merges the lecture notes, exercises, labs, and sample programs into one non-repetitive summary.

## 1. Functions, Pass-by-Value, Pass-by-Reference, and Recursion

### What you must know

- A function groups reusable code.
- General form:

```cpp
return_type function_name(parameter_list) {
    // body
}
```

- A function can:
  - return a value
  - return nothing using `void`
  - receive zero or more arguments

### Function declaration vs definition

- A declaration tells the compiler that a function exists.
- A definition provides the full body.
- If a function is called before its full definition appears, a declaration must appear first.

```cpp
int add(int, int);   // declaration

int main() {
    cout << add(2, 3);
}

int add(int a, int b) {   // definition
    return a + b;
}
```

### Pass-by-value

- The function receives a copy.
- Changes inside the function do not affect the original variable.
- Safer, but potentially less efficient for large objects.

```cpp
void increment(int x) {
    x++;
}
```

### Pass-by-reference

- The function receives an alias to the original variable.
- Changes inside the function affect the caller’s variable.
- Useful when you must modify the original object.

```cpp
void increment(int& x) {
    x++;
}
```

### Const reference

- Best choice when:
  - the object may be large
  - you do not want to copy it
  - you must not modify it

```cpp
void print(const string& text) {
    cout << text;
}
```

### Pass-by-value vs pass-by-reference

- Use pass-by-value for small primitive values when a copy is fine.
- Use pass-by-reference when the function must modify the caller’s variable.
- Use `const T&` when the object should not be copied and should not be modified.

### Default parameters

- A parameter can have a default value.
- If the caller does not pass that argument, the default is used.

```cpp
int divide(int x, int d = 2) {
    return x / d;
}
```

- `divide(10)` uses `d = 2`.
- `divide(10, 5)` uses `d = 5`.

### Inline functions

- `inline` suggests that the compiler may replace a function call with the function body.
- Usually relevant only for very small functions.
- You should know the idea, not depend on it for correctness.

### Recursion

- A recursive function calls itself.
- Every recursive function needs:
  - a base case
  - a recursive step

Example:

```cpp
int factorial(int n) {
    if (n == 0) {
        return 1;
    }
    return n * factorial(n - 1);
}
```

### Recursive functions from the course materials

- `factorial(n)`
- `divide(int)` repeatedly dividing by 2 until the value becomes less than 1
- `fibonacci(n)`
- recursive string reversal

### Call stack

You should understand that:

- each function call creates a new stack frame
- parameters and local variables belong to that frame
- recursive calls create many stack frames
- execution returns back up the stack after the base case is reached

Typical theory question:

- draw the call stack for `factorial(4)` or a recursive `divide()` call

### Common exam-style function tasks

- write `increment(int&)`
- write `swap(int&, int&)`
- write a function that returns density
- write a function converting `char` to ASCII integer
- write `sumRandom(start, end)`
- explain what changes when a parameter is passed by value vs by reference

### Common mistakes

- forgetting `&` in a function that must modify the original variable
- returning the wrong type
- missing a base case in recursion
- infinite recursion
- using pass-by-value for large objects when `const&` is better

## 2. File I/O

### Libraries and stream types

- Include `<fstream>`.
- Use:
  - `ifstream` for reading from files
  - `ofstream` for writing to files

### Basic reading pattern

```cpp
#include <fstream>

ifstream fin("data.txt");
if (fin) {
    int x;
    fin >> x;
    fin.close();
}
```

### Basic writing pattern

```cpp
ofstream fout("output.txt");
if (fout) {
    fout << "Hello" << endl;
    fout.close();
}
```

### Core workflow

1. Open the file.
2. Check whether the file opened successfully.
3. Read from or write to the stream.
4. Close the file.

### Reading unknown amounts of data

The lecture notes show `while (!fin.eof())`, but the safer and more idiomatic pattern is:

```cpp
int value;
while (fin >> value) {
    // use value
}
```

This avoids processing invalid reads after the stream fails.

### Formatted extraction

- `>>` reads token by token and skips whitespace.
- `getline()` reads a whole line.
- If you mix `>>` and `getline()`, you may need `ignore()` to discard the leftover newline.

Example:

```cpp
file.ignore(numeric_limits<streamsize>::max(), '\n');
```

### Practical tasks from the exercises and labs

- ask the user for a name and write it to `name.txt`
- read a name from `name.txt` and print it inside a star border
- read integers from `num.txt` and compute an average
- read student names and marks, compute a weighted grade, and write results to a new file
- read a menu where each item has a description and a price, update prices, and write a new menu

### What to know for file-processing questions

- how to open a file with a constructor
- how to open a file with `.open()`
- how to test whether the file opened successfully
- how to read primitives and strings
- how to read until the file ends
- how to write output in the required format
- how to transform input data into output data

### Common mistakes

- forgetting to check whether the file opened
- dividing by `count` when `count == 0`
- mixing `getline()` and `>>` incorrectly
- forgetting to close the file
- reading paired values incorrectly when the input format alternates between lines and tokens

## 3. Containers, Iterators, and STL Algorithms

### Three STL parts you must know

- Containers
- Iterators
- Algorithms

### Containers covered in your materials

- `vector`
- `list`

### Vector

`vector<T>` is a dynamic array.

Main properties:

- grows and shrinks as needed
- supports indexing
- fast random access
- good when you need direct element access by position

Common operations:

```cpp
vector<int> nums;
nums.push_back(10);
nums.push_back(20);

cout << nums[0] << endl;
cout << nums.at(1) << endl;
cout << nums.front() << endl;
cout << nums.back() << endl;
cout << nums.size() << endl;
```

Important difference:

- `nums.at(i)` checks bounds and can throw an exception
- `nums[i]` does not do bounds checking

### List

`list<T>` is a doubly linked list.

Main properties:

- efficient insertion and removal in the middle
- no indexing with `[]`
- access is usually done with iterators

Use a list when insertion/removal pattern matters more than random access.

### Iterators

An iterator points to a position inside a container.

Main iterator functions:

- `begin()`
- `end()`
- `cbegin()`
- `cend()`
- `rbegin()`
- `rend()`

`end()` points one past the last element.

### Dereferencing and moving iterators

```cpp
auto it = nums.begin();
cout << *it << endl;
++it;
```

- `*it` gives the element
- `++it` moves to the next position

### Typical iterator loop

```cpp
for (auto it = nums.begin(); it != nums.end(); ++it) {
    cout << *it << " ";
}
```

### Iterator types mentioned in the lecture

- iterator
- const_iterator
- reverse_iterator
- const_reverse_iterator

### Algorithms you should know from the materials and sample code

- `sort`
- `count`
- `count_if`
- `find`
- `find_if`
- `all_of`
- `none_of`

### Sorting

Ascending sort:

```cpp
sort(nums.begin(), nums.end());
```

Descending sort:

```cpp
sort(nums.begin(), nums.end(), [](int a, int b) {
    return a > b;
});
```

Sorting strings by length:

```cpp
sort(words.begin(), words.end(), [](const string& a, const string& b) {
    return a.size() < b.size();
});
```

Important note:

- `std::sort` requires random-access iterators, so it works with `vector`
- for `list`, use `list.sort()` instead of `std::sort`

### Counting and searching

```cpp
size_t n = count(nums.begin(), nums.end(), 18);

size_t under18 = count_if(nums.begin(), nums.end(), [](int x) {
    return x < 18;
});

auto it = find(nums.begin(), nums.end(), 17);
auto it2 = find_if(nums.begin(), nums.end(), [](int x) {
    return x % 2 == 0;
});
```

### Removing elements from a vector

```cpp
for (auto it = nums.begin(); it != nums.end(); ) {
    if (*it % 2 == 0) {
        it = nums.erase(it);
    } else {
        ++it;
    }
}
```

You must know why this is necessary:

- when `erase()` removes an element from a vector, iterators to shifted elements become invalid
- `erase()` returns the next valid iterator

### Iteration styles you should know

- index-based loop
- range-based for loop
- iterator-based loop

### Practical tasks from your exercises and labs

- read integers from file into a `vector`
- repeat with `list`
- sort integers ascending
- sort integers descending
- sort strings by length
- print a vector using a `print()` function
- overload `print()` for `vector<int>` and `vector<string>`
- insert missing months using iterators
- write `areSameUntil()`
- write `areSameReverse()`
- write `interleave()`

### Vector vs list

Prefer `vector` when:

- you need indexing
- you need fast access to arbitrary positions
- the main operations are traversal and append

Prefer `list` when:

- you insert and erase in the middle often
- random access is not required

### Common mistakes

- dereferencing `end()`
- moving an iterator beyond a valid range
- using `[]` on a `list`
- forgetting that `erase()` invalidates iterators
- assuming `std::sort` works directly on `std::list`

## 4. Structs

### What a struct is

A `struct` groups related data under one name.

```cpp
struct Movie {
    string title;
    int year;
};
```

### Key fact

- Members of a `struct` are public by default.
- A `struct` is very similar to a class, but typically used for plain data records.

### Declaring and using struct objects

```cpp
Movie movie1;
movie1.title = "Jaws";
movie1.year = 1975;
```

### Member access operators

- Use `.` when you have an object.
- Use `->` when you have a pointer to an object.

```cpp
Movie movie1;
Movie* ptrMovie = &movie1;

movie1.year = 1975;
ptrMovie->year = 1975;
```

### Nested structs

Your labs use a nested structure like:

```cpp
struct Geolocation {
    double lng;
    double lat;
};

struct Tweet {
    string author;
    string message;
    unsigned long int timestamp;
    int id;
    Geolocation loc;
};
```

You should be able to access nested members:

```cpp
tweet.loc.lat = 53.3;
tweet.loc.lng = -6.2;
```

### Passing structs to functions

By value:

```cpp
void display(Movie movie);
```

- copies the whole struct
- modifications affect only the local copy

By const reference:

```cpp
void display(const Movie& movie);
```

- efficient
- does not modify the original

By pointer:

```cpp
void display(Movie* moviePtr);
```

- access members with `->`
- can modify the original object unless the pointer points to `const`

### Arrays of structs

```cpp
Movie movies[3];
movies[0].title = "Alien";
movies[1].title = "Jaws";
movies[2].title = "Arrival";
```

Iterating:

```cpp
for (int i = 0; i < 3; i++) {
    cout << movies[i].title << endl;
}
```

### Typical struct tasks from your materials

- create a `tweet_struct`
- store author, message, ID, timestamp, and geolocation
- display a struct by value
- display a struct by const reference
- declare and populate an array of structs
- write a function to display an array of structs
- declare an `Rgb` struct and apply a filter

### Common mistakes

- forgetting to initialize struct members
- using `.` when the variable is a pointer
- using `->` when the variable is not a pointer
- passing a large struct by value unnecessarily

## 5. Pointers and Dynamic Memory

### Pointer basics

A pointer stores the address of an object in memory.

```cpp
int x = 25;
int* ptr = &x;
```

- `&x` means “address of `x`”
- `ptr` stores that address

### Dereferencing

Use `*` to access the value at the pointed location.

```cpp
cout << *ptr << endl;
*ptr = 99;
```

If `ptr` points to `x`, then changing `*ptr` changes `x`.

### Pointer parameters

You can pass the address of a variable to a function:

```cpp
void increment(double* d) {
    *d += 0.01;
}
```

Call:

```cpp
double value = 3.14;
increment(&value);
```

### Pointer parameters and const

If a function must not modify the original object:

```cpp
double divide(const int* p1, const int* p2) {
    return *p1 / static_cast<double>(*p2);
}
```

### Arrays and pointers

When an array is passed to a function, the array decays to a pointer to its first element.

```cpp
void display(const int arr[], int length);
void display(const int* arr, int length);
```

These are effectively equivalent for parameter passing.

### Array notation vs pointer notation

Array notation:

```cpp
sum += arr[i];
```

Pointer notation:

```cpp
sum += *ptr;
++ptr;
```

You should be able to write both styles.

### Pointer arithmetic

- `ptr++` moves to the next element of the correct type
- with an `int*`, it moves by `sizeof(int)` bytes
- with a `double*`, it moves by `sizeof(double)` bytes

### C-style strings

The pointer exercise includes:

```cpp
int strcmp(const char* s1, const char* s2);
```

You should know that:

- C-style strings end with `'\0'`
- pointer loops typically advance one character at a time

### Dynamic memory allocation

Allocate one object:

```cpp
int* p = new int;
*p = 25;
```

Free it:

```cpp
delete p;
p = nullptr;
```

Allocate an array dynamically:

```cpp
int* arr = new int[size];
```

Free it:

```cpp
delete[] arr;
arr = nullptr;
```

### Stack vs heap

Stack:

- automatic local variables
- managed automatically

Heap:

- memory requested at runtime with `new`
- must be released with `delete` or `delete[]`

### Why use dynamic memory

- size may only be known at runtime
- you need memory beyond normal automatic object lifetime
- you need a dynamically allocated object or array

### Structs and dynamic memory

Allocate one struct:

```cpp
Movie* moviePtr = new Movie;
moviePtr->title = "Jaws";
delete moviePtr;
moviePtr = nullptr;
```

Allocate an array of structs:

```cpp
Movie* movies = new Movie[size];
movies[0].title = "Alien";
delete[] movies;
movies = nullptr;
```

### Critical memory concepts

- memory leak: allocated memory is never freed
- dangling pointer: pointer still points to memory that has already been freed
- `nullptr`: special value meaning the pointer points nowhere valid

### Practical tasks from the exercises and labs

- increment a `double` using a pointer parameter
- print a variable’s address and confirm it matches the pointer value
- divide two integers via pointer parameters
- sum array values using both array notation and pointer notation
- double array elements using a pointer
- write `strcmp(const char*, const char*)`
- dynamically allocate one value and one array
- dynamically allocate a struct and an array of structs

### Common mistakes

- dereferencing an uninitialized pointer
- forgetting to use `&` when passing an address
- forgetting `delete` or `delete[]`
- using `delete` instead of `delete[]` for an array
- using a pointer after deletion
- incrementing a pointer beyond the valid array range

## 6. What Is Most Likely to Appear on the Exam

This section is an inference from the exercise sheets, labs, sample code, and the exam notice.

### Very likely theory questions

- explain pass-by-value vs pass-by-reference
- explain a pointer and the meaning of `&` and `*`
- explain the difference between stack memory and heap memory
- explain `new`, `delete`, `new[]`, and `delete[]`
- explain how iterators work
- compare `vector` and `list`
- explain when `const` should be used in parameters
- draw a call stack for a recursive function
- explain member access with `.` and `->`

### Very likely code-writing tasks

- write a small function using reference parameters
- write a recursive function such as factorial, fibonacci, or repeated divide-by-2
- read a file and compute or transform values
- use a `vector` to store data from a file
- sort a container
- use `find`, `count_if`, or a lambda
- create a struct and display it
- iterate through an array of structs
- write a pointer-based array function

### Practical patterns that appear repeatedly in your materials

- menu and `switch`
- framed text output
- weighted grade calculation
- file transform input -> output
- `vector` population with `push_back`
- iterator loop with `begin()` and `end()`
- erase while iterating
- pointer-based modification of caller data
- struct display by value, reference, and pointer

## 7. High-Value Short Templates

### Swap by reference

```cpp
void swapValues(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

### Recursive factorial

```cpp
int factorial(int n) {
    if (n == 0) {
        return 1;
    }
    return n * factorial(n - 1);
}
```

### Read integers from a file

```cpp
ifstream fin("nums.txt");
int x;
while (fin >> x) {
    cout << x << endl;
}
fin.close();
```

### Read into a vector

```cpp
vector<int> nums;
int x;
while (fin >> x) {
    nums.push_back(x);
}
```

### Iterator loop

```cpp
for (auto it = nums.begin(); it != nums.end(); ++it) {
    cout << *it << " ";
}
```

### Sort descending

```cpp
sort(nums.begin(), nums.end(), [](int a, int b) {
    return a > b;
});
```

### Pointer increment function

```cpp
void increment(double* d) {
    *d += 0.01;
}
```

### Array sum with pointer notation

```cpp
double sum(const double* ptr, int length) {
    double total = 0;
    for (int i = 0; i < length; i++) {
        total += *ptr;
        ++ptr;
    }
    return total;
}
```

### Struct display by const reference

```cpp
void display(const Tweet& tweet) {
    cout << tweet.author << endl;
    cout << tweet.message << endl;
}
```

### Dynamic array

```cpp
int* arr = new int[size];
delete[] arr;
arr = nullptr;
```

## 8. Fast Revision Checklist

Before the exam, make sure you can do all of these without notes:

- write and call a function
- explain declaration vs definition
- explain pass-by-value vs reference vs const reference
- write one recursive function and draw its call stack
- open, read, write, and close a file
- read unknown amounts of data from a file
- use `vector`, `push_back`, `size`, `at`, `front`, `back`
- loop with iterators
- use `sort`, `count_if`, `find`, `find_if`
- explain iterator invalidation after `erase`
- declare a struct and access members
- write and use an array of structs
- explain `&`, `*`, pointer parameters, and pointer arithmetic
- write `new`/`delete` and `new[]`/`delete[]` correctly
- explain memory leaks and dangling pointers

## 9. Final Advice for the Cheat Sheet

If you are preparing the permitted cheat sheet, the highest-value items to include are:

- function syntax patterns
- pass-by-value / reference / const reference examples
- one recursion template
- one file read template and one file write template
- vector operations and iterator loop syntax
- common STL algorithms with lambda examples
- struct syntax with `.` and `->`
- pointer basics with `&`, `*`, `new`, `delete`, `new[]`, `delete[]`
- a short list of mistakes to avoid

If you can reproduce the code patterns in sections 7 and 8 quickly and explain the concepts in sections 1 to 5 clearly, you will be covering the main recurring material from this folder and the exam notice.
