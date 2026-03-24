# C++ Written Exam Cheat Sheet

## Exam Topics

- Functions
- Recursion
- Pass-by-value
- Pass-by-reference
- File I/O
- Containers
- Iterators
- STL algorithms
- Structs
- Pointers

This sheet is intentionally dense and redundant in useful ways. It is designed for print and fast lookup during the exam.

## 1. Basic Program Skeleton

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <list>
#include <string>
#include <algorithm>
#include <limits>

using namespace std;

// function declarations
int add(int, int);
void print(const vector<int>&);

int main() {
    // code
    return 0;
}

int add(int a, int b) {
    return a + b;
}

void print(const vector<int>& v) {
    for (int x : v) {
        cout << x << " ";
    }
    cout << endl;
}
```

## 2. Functions

### Why functions are used

- code reuse
- better structure
- easier testing
- easier maintenance
- less duplication

### General form

```cpp
return_type function_name(parameter_list) {
    statements
}
```

Examples:

```cpp
int add(int a, int b) {
    return a + b;
}

void greet() {
    cout << "Hello" << endl;
}
```

### Function declaration and definition

Declaration:

```cpp
int add(int, int);
```

Definition:

```cpp
int add(int a, int b) {
    return a + b;
}
```

Important:

- A function must be declared before it is called.
- If full definition appears later, put a declaration near the top.

### Return types

- `int`
- `double`
- `char`
- `bool`
- `string`
- `void`

### Parameters

- parameters are local to the function
- arguments are the actual values passed in

```cpp
int multiply(int x, int y) {   // x and y are parameters
    return x * y;
}

int result = multiply(3, 4);   // 3 and 4 are arguments
```

### Pass-by-value

```cpp
void increment(int x) {
    x++;
}
```

- `x` is a copy
- original variable is unchanged

```cpp
int a = 5;
increment(a);
cout << a;   // still 5
```

### Pass-by-reference

```cpp
void increment(int& x) {
    x++;
}
```

- `x` refers to the original variable
- changes affect the caller

```cpp
int a = 5;
increment(a);
cout << a;   // 6
```

### Const reference

Use for efficiency when:

- object is large
- no copy wanted
- no modification allowed

```cpp
void printName(const string& name) {
    cout << name << endl;
}
```

### When to use which

- `int x`:
  - small type
  - copy is fine
- `int& x`:
  - must modify original
- `const int& x`:
  - often unnecessary for small primitive types, but valid
- `const string& s`:
  - good for large objects
- `vector<int>& v`:
  - modify vector
- `const vector<int>& v`:
  - read vector only

### Function overloading

Same function name, different parameter lists.

```cpp
void print(int x) {
    cout << x << endl;
}

void print(const string& s) {
    cout << s << endl;
}
```

### Default parameters

```cpp
int divide(int x, int d = 2) {
    return x / d;
}
```

Calls:

```cpp
divide(10);      // 5
divide(10, 5);   // 2
```

### Inline functions

```cpp
inline int square(int x) {
    return x * x;
}
```

- compiler may expand the function inline
- usually only useful for small functions

### Useful small function templates

```cpp
int maxValue(int a, int b) {
    return (a > b) ? a : b;
}

bool isEven(int x) {
    return x % 2 == 0;
}

double average(int total, int count) {
    return static_cast<double>(total) / count;
}
```

### Swap

```cpp
void swapValues(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

Need reference because original values must change.

### Typical theory explanation

Pass-by-value:

- function receives a copy
- original is unchanged
- safer
- may be less efficient for large objects

Pass-by-reference:

- function receives alias to original
- original may change
- more efficient for large objects
- risk of accidental modification

Const reference:

- no copy
- original cannot be changed through that parameter
- often best for large read-only objects

### Common mistakes with functions

- forgetting `return`
- wrong return type
- missing function declaration
- using pass-by-value when you meant pass-by-reference
- changing a value when it should be `const`

## 3. Recursion

### Definition

A recursive function is a function that calls itself.

### Every recursive function needs

- base case
- recursive case

### Factorial

```cpp
int factorial(int n) {
    if (n == 0) {
        return 1;
    }
    return n * factorial(n - 1);
}
```

### Fibonacci

```cpp
int fibonacci(int n) {
    if (n <= 2) {
        return n - 1;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

First values:

- `F(1) = 0`
- `F(2) = 1`
- `F(3) = 1`
- `F(4) = 2`
- `F(5) = 3`

### Divide by 2 recursively

```cpp
void divide(int x) {
    if (x < 1) {
        return;
    }
    cout << x << endl;
    divide(x / 2);
}
```

### Reverse a string recursively

```cpp
string reverseString(const string& s) {
    if (s.empty()) {
        return "";
    }
    return reverseString(s.substr(1)) + s[0];
}
```

### Call stack idea

Each call gets:

- its own parameters
- its own local variables
- its own stack frame

When base case is reached:

- no more recursive calls
- returns begin
- execution unwinds back up the stack

### If asked to draw call stack for `factorial(4)`

Calls:

- `factorial(4)`
- `factorial(3)`
- `factorial(2)`
- `factorial(1)`
- `factorial(0)` base case

Then returns:

- `1`
- `1 * 1 = 1`
- `2 * 1 = 2`
- `3 * 2 = 6`
- `4 * 6 = 24`

### Common recursion mistakes

- no base case
- wrong base case
- recursive call does not move toward base case
- infinite recursion

## 4. File I/O

### Libraries

```cpp
#include <fstream>
```

### File stream types

- `ifstream` = input file stream
- `ofstream` = output file stream

### Open-read-close

```cpp
ifstream fin("data.txt");
if (fin) {
    int x;
    fin >> x;
    fin.close();
} else {
    cout << "Error opening file" << endl;
}
```

### Open-write-close

```cpp
ofstream fout("out.txt");
if (fout) {
    fout << "Hello" << endl;
    fout.close();
}
```

### Alternative open syntax

```cpp
ifstream fin;
fin.open("data.txt");
```

### Reading until end of file

Best pattern:

```cpp
int x;
while (fin >> x) {
    cout << x << endl;
}
```

For strings:

```cpp
string word;
while (fin >> word) {
    cout << word << endl;
}
```

For whole lines:

```cpp
string line;
while (getline(fin, line)) {
    cout << line << endl;
}
```

### Why `while (fin >> x)` is better than `while (!fin.eof())`

- checks that the read succeeded
- avoids processing invalid data after failure

### Write user name to file

```cpp
string name;
cin >> name;

ofstream fout("name.txt");
fout << name;
fout.close();
```

### Read name and print star border

```cpp
ifstream fin("name.txt");
string name;
fin >> name;
fin.close();

for (int i = 0; i < name.length() + 4; i++) {
    cout << "*";
}
cout << endl;
cout << "* " << name << " *" << endl;
for (int i = 0; i < name.length() + 4; i++) {
    cout << "*";
}
cout << endl;
```

### Average of integers from file

```cpp
ifstream fin("num.txt");
int number;
int sum = 0;
int count = 0;

while (fin >> number) {
    sum += number;
    count++;
}
fin.close();

if (count > 0) {
    cout << static_cast<double>(sum) / count << endl;
}
```

### Weighted grade from file

Input format example:

```text
John 70 80
Mary 50 90
```

Code:

```cpp
ifstream fin("results.txt");
ofstream fout("final_results.txt");

string name;
int examMark;
int caMark;

while (fin >> name >> examMark >> caMark) {
    int finalGrade = (examMark * 60 + caMark * 40) / 100;
    fout << name << " " << finalGrade << endl;
}

fin.close();
fout.close();
```

### Reading description line + price

```cpp
ifstream fin("menu.txt");
ofstream fout("new_menu.txt");

string item;
double price;

while (getline(fin, item) && fin >> price) {
    fin.ignore(numeric_limits<streamsize>::max(), '\n');

    if (price > 5.0) {
        price += 1.5;
    } else {
        price += 0.5;
    }

    fout << item << " " << price << endl;
}

fin.close();
fout.close();
```

### Common file I/O theory points

- streams are like pipes of data
- `>>` extracts formatted input
- `<<` inserts output
- always check file opened successfully
- close files when finished

### Common mistakes

- forgetting `#include <fstream>`
- forgetting open check
- wrong input format assumptions
- division by zero in average
- mixing `getline()` and `>>` incorrectly

## 5. STL Overview

Three main STL parts:

- containers
- iterators
- algorithms

## 6. Vector

### What it is

`vector<T>` is a dynamic array.

### Main properties

- stores elements of one type
- grows and shrinks dynamically
- supports indexing
- efficient random access

### Declaration

```cpp
vector<int> nums;
vector<string> words;
vector<double> grades;
```

### Add values

```cpp
nums.push_back(10);
nums.push_back(20);
nums.push_back(30);
```

### Access values

```cpp
cout << nums[0] << endl;
cout << nums.at(1) << endl;
cout << nums.front() << endl;
cout << nums.back() << endl;
```

### Size

```cpp
cout << nums.size() << endl;
```

### Empty

```cpp
if (nums.empty()) {
    cout << "Empty" << endl;
}
```

### Loop by index

```cpp
for (int i = 0; i < nums.size(); i++) {
    cout << nums[i] << " ";
}
```

### Range-based loop

```cpp
for (int x : nums) {
    cout << x << " ";
}
```

### Pass vector to function

Read-only:

```cpp
void print(const vector<int>& v) {
    for (int x : v) {
        cout << x << " ";
    }
}
```

Modify:

```cpp
void addOne(vector<int>& v) {
    for (int& x : v) {
        x++;
    }
}
```

### `at()` vs `[]`

- `at()` checks bounds
- `[]` does not check bounds
- `at()` is safer

## 7. List

### What it is

`list<T>` is a linked list.

### Main properties

- good for insertion and deletion
- no indexing with `[]`
- use iterators to access elements

### Declaration

```cpp
list<int> nums;
nums.push_back(10);
nums.push_back(20);
nums.push_back(30);
```

### Iterator loop

```cpp
for (auto it = nums.begin(); it != nums.end(); ++it) {
    cout << *it << " ";
}
```

### Vector vs list

Vector:

- fast random access
- supports `[]`
- usually best default choice

List:

- better for insertion/deletion in the middle
- no random access by index

## 8. Iterators

### Definition

An iterator identifies a position in a container.

Think of it like a smart pointer for a container.

### Main methods

- `begin()`
- `end()`
- `cbegin()`
- `cend()`
- `rbegin()`
- `rend()`

### Meaning of `end()`

- points one past the last element
- do not dereference `end()`

### Basic use

```cpp
vector<int> nums = {10, 20, 30};
auto it = nums.begin();

cout << *it << endl;   // 10
++it;
cout << *it << endl;   // 20
```

### Standard iterator loop

```cpp
for (auto it = nums.begin(); it != nums.end(); ++it) {
    cout << *it << " ";
}
```

### Constant iterators

```cpp
for (auto it = nums.cbegin(); it != nums.cend(); ++it) {
    cout << *it << " ";
}
```

- safer when no modification intended

### Reverse iterators

```cpp
for (auto it = nums.rbegin(); it != nums.rend(); ++it) {
    cout << *it << " ";
}
```

### Insert using iterator

```cpp
auto it = nums.begin();
++it;
nums.insert(it, 99);
```

### Erase using iterator

```cpp
auto it = nums.begin();
++it;
nums.erase(it);
```

### Important erase rule

After erasing from a vector:

- current iterator may become invalid
- use the iterator returned by `erase()`

```cpp
for (auto it = nums.begin(); it != nums.end(); ) {
    if (*it % 2 == 0) {
        it = nums.erase(it);
    } else {
        ++it;
    }
}
```

### `advance()`

```cpp
auto it = nums.begin();
advance(it, 3);
```

- moves iterator forward by 3 positions

## 9. STL Algorithms

### Include

```cpp
#include <algorithm>
```

### Sort ascending

```cpp
sort(nums.begin(), nums.end());
```

### Sort descending

```cpp
sort(nums.begin(), nums.end(), [](int a, int b) {
    return a > b;
});
```

### Sort strings by length

```cpp
sort(words.begin(), words.end(), [](const string& a, const string& b) {
    return a.size() < b.size();
});
```

### Count

```cpp
int n = count(nums.begin(), nums.end(), 18);
```

### Count with condition

```cpp
int under18 = count_if(nums.begin(), nums.end(), [](int x) {
    return x < 18;
});
```

### Find exact value

```cpp
auto it = find(nums.begin(), nums.end(), 17);
if (it != nums.end()) {
    cout << "Found" << endl;
}
```

### Find by condition

```cpp
auto it = find_if(nums.begin(), nums.end(), [](int x) {
    return x % 2 == 0;
});
```

### all_of

```cpp
bool result = all_of(nums.begin(), nums.end(), [](int x) {
    return x > 0;
});
```

### none_of

```cpp
bool result = none_of(nums.begin(), nums.end(), [](int x) {
    return x < 0;
});
```

### Lambdas

General form:

```cpp
[](parameter_list) {
    statements
}
```

Examples:

```cpp
[](int x) { return x % 2 == 0; }
[](const string& a, const string& b) { return a.size() < b.size(); }
```

### Important note

- `sort()` works with `vector`
- `std::sort()` does not work directly with `list`
- use `list.sort()` for lists

## 10. Useful Vector/List Practice Patterns

### Read integers into vector

```cpp
ifstream fin("nums.txt");
vector<int> nums;
int x;

while (fin >> x) {
    nums.push_back(x);
}
```

### Read integers into list

```cpp
ifstream fin("nums.txt");
list<int> nums;
int x;

while (fin >> x) {
    nums.push_back(x);
}
```

### Print vector

```cpp
void print(const vector<int>& v, const string& msg = "Values: ") {
    cout << msg << "[ ";
    for (auto it = v.begin(); it != v.end(); ++it) {
        if (it != v.begin()) {
            cout << ", ";
        }
        cout << *it;
    }
    cout << " ]" << endl;
}
```

### Overload print for strings

```cpp
void print(const vector<string>& v, const string& msg = "Values: ") {
    cout << msg << "[ ";
    for (auto it = v.begin(); it != v.end(); ++it) {
        if (it != v.begin()) {
            cout << ", ";
        }
        cout << *it;
    }
    cout << " ]" << endl;
}
```

### `areSameUntil()`

```cpp
int areSameUntil(const vector<int>& v1, const vector<int>& v2) {
    size_t minSize = min(v1.size(), v2.size());

    for (size_t i = 0; i < minSize; i++) {
        if (v1[i] != v2[i]) {
            return static_cast<int>(i);
        }
    }

    if (v1.size() != v2.size()) {
        return static_cast<int>(minSize);
    }

    return -1;
}
```

Meaning:

- returns first position where they differ
- returns `-1` if completely same

### `areSameReverse()`

```cpp
bool areSameReverse(const vector<int>& v1, const vector<int>& v2) {
    if (v1.size() != v2.size()) {
        return false;
    }

    for (size_t i = 0; i < v1.size(); i++) {
        if (v1[i] != v2[v2.size() - 1 - i]) {
            return false;
        }
    }

    return true;
}
```

### `interleave()`

Assumption:

- vectors have same size

```cpp
void interleave(const vector<int>& v1, const vector<int>& v2, vector<int>& out) {
    out.clear();

    for (size_t i = 0; i < v1.size(); i++) {
        out.push_back(v1[i]);
        out.push_back(v2[i]);
    }
}
```

## 11. Structs

### Definition

A struct groups related data under one name.

```cpp
struct Movie {
    string title;
    int year;
};
```

### Important fact

- struct members are public by default

### Declare a struct variable

```cpp
Movie m;
```

### Access members

```cpp
m.title = "Jaws";
m.year = 1975;
```

Use `.` when you have an object.

### Struct example from exam-style tasks

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

### Populate a struct

```cpp
Tweet tweet1;
tweet1.author = "Nikita";
tweet1.message = "My first tweet";
tweet1.timestamp = 1773233433;
tweet1.id = 1;
tweet1.loc.lng = 43.654;
tweet1.loc.lat = -21.343;
```

### Display by value

```cpp
void displayByValue(Tweet tweet) {
    cout << tweet.id << endl;
    cout << tweet.author << endl;
    cout << tweet.message << endl;
}
```

- whole struct is copied
- changes only affect copy

### Display by const reference

```cpp
void displayByReference(const Tweet& tweet) {
    cout << tweet.id << endl;
    cout << tweet.author << endl;
    cout << tweet.message << endl;
}
```

- more efficient
- no modification

### Array of structs

```cpp
Tweet tweets[3];
```

Populate:

```cpp
tweets[0].author = "A";
tweets[1].author = "B";
tweets[2].author = "C";
```

Loop:

```cpp
for (int i = 0; i < 3; i++) {
    cout << tweets[i].author << endl;
}
```

### Display array of structs

```cpp
void displayArray(Tweet arr[], int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i].id << " " << arr[i].author << endl;
    }
}
```

### Rgb struct from exercises

```cpp
struct Rgb {
    float r, g, b;
    Rgb() : r(0), g(0), b(0) {}
    Rgb(float rr, float gg, float bb) : r(rr), g(gg), b(bb) {}
};
```

### Display pixel channels

```cpp
void displayPixel(const Rgb& pixel) {
    cout << pixel.r << " " << pixel.g << " " << pixel.b << endl;
}
```

### Red filter

```cpp
void redFilter(Rgb& pixel) {
    pixel.g = 0;
    pixel.b = 0;
}
```

### If asked “struct vs class”

Good short answer:

- both can store data and functions
- main default difference:
  - `struct` members are public by default
  - `class` members are private by default
- in this course, structs are mainly used for simple data records

## 12. Pointers

### Definition

A pointer is a variable that stores the address of another object.

```cpp
int x = 25;
int* p = &x;
```

### Symbols

- `&x` = address of `x`
- `*p` = value stored at the address in `p`

### Example

```cpp
int x = 25;
int* p = &x;

cout << x << endl;    // 25
cout << &x << endl;   // address of x
cout << p << endl;    // same address
cout << *p << endl;   // 25
```

### Modify through pointer

```cpp
*p = 99;
cout << x << endl;   // 99
```

### Good practice

```cpp
int* p = nullptr;
```

`nullptr` means the pointer points nowhere valid.

### Pointer parameter

```cpp
void increment(double* d) {
    *d += 0.01;
}

double value = 3.14;
increment(&value);
```

### Divide using pointers

```cpp
double divide(const int* p1, const int* p2) {
    return *p1 / static_cast<double>(*p2);
}
```

Why `const`:

- prevents modification of original integers

### Pointers and arrays

Array name acts like address of first element.

```cpp
int arr[4] = {10, 20, 30, 40};
```

Pass to function:

```cpp
void display(int arr[], int length);
void display(int* arr, int length);
```

Both are valid.

### Array notation

```cpp
double sum(const double arr[], int length) {
    double total = 0;
    for (int i = 0; i < length; i++) {
        total += arr[i];
    }
    return total;
}
```

### Pointer notation

```cpp
double sumPointer(const double* ptr, int length) {
    double total = 0;
    for (int i = 0; i < length; i++) {
        total += *ptr;
        ptr++;
    }
    return total;
}
```

### Modify array through pointer

```cpp
void doubleElements(double* ptr, int length) {
    for (int i = 0; i < length; i++) {
        *ptr = *ptr * 2;
        ptr++;
    }
}
```

Do not use `const` here because function must modify elements.

### Pointer arithmetic

```cpp
ptr++;
ptr--;
```

For arrays:

- moves by one element, not one byte
- type matters

### C-style strings

Character arrays end with null terminator:

```cpp
'\0'
```

Example strcmp-style function:

```cpp
int strcmp2(const char* s1, const char* s2) {
    while (*s1 != '\0' && *s2 != '\0' && *s1 == *s2) {
        s1++;
        s2++;
    }

    if (*s1 < *s2) {
        return -1;
    }
    if (*s1 > *s2) {
        return 1;
    }
    return 0;
}
```

### Important pointer theory

- pointer stores address
- dereference gets value at address
- pointer type must match pointed object type
- pointer parameters receive addresses by value
- dereferencing invalid pointers causes undefined behavior

## 13. Dynamic Memory

### Stack vs heap

Stack:

- automatic local variables
- memory managed automatically
- life usually ends when function returns

Heap:

- dynamic memory
- requested at runtime with `new`
- must be freed manually with `delete`

### Allocate one object

```cpp
int* p = new int;
*p = 25;
```

### Free one object

```cpp
delete p;
p = nullptr;
```

### Allocate array dynamically

```cpp
int size = 5;
int* arr = new int[size];
```

### Free dynamic array

```cpp
delete[] arr;
arr = nullptr;
```

### Important rule

- `new` pairs with `delete`
- `new[]` pairs with `delete[]`

### Dynamic memory for struct

```cpp
Movie* moviePtr = new Movie;
moviePtr->title = "Alien";
moviePtr->year = 1979;

delete moviePtr;
moviePtr = nullptr;
```

### Dynamic array of structs

```cpp
int size = 3;
Movie* movies = new Movie[size];

movies[0].title = "Jaws";
movies[1].title = "Alien";
movies[2].title = "Arrival";

delete[] movies;
movies = nullptr;
```

### Member access through pointer to struct

```cpp
moviePtr->title = "Jaws";
```

Equivalent but uglier:

```cpp
(*moviePtr).title = "Jaws";
```

### Memory leak

Definition:

- memory allocated with `new`
- never freed with `delete`

Example:

```cpp
int* p = new int;
*p = 5;
// forgot delete
```

### Dangling pointer

Definition:

- pointer still stores an old address
- but memory at that address has already been freed

Example:

```cpp
int* p = new int;
delete p;
// p is now dangling
```

Safer:

```cpp
delete p;
p = nullptr;
```

### Why set pointer to `nullptr` after delete

- helps avoid accidental use
- easier to detect bugs

## 14. Struct Pointers and Arrays of Structs

### Pointer to struct

```cpp
Movie m;
Movie* ptr = &m;
ptr->year = 2000;
```

### Array of structs with pointer

```cpp
Movie movies[3];
Movie* ptr = movies;

for (int i = 0; i < 3; i++) {
    cout << ptr->title << endl;
    ptr++;
}
```

### Dynamic array of structs with pointer loop

```cpp
Movie* movies = new Movie[3];

movies[0].title = "A";
movies[1].title = "B";
movies[2].title = "C";

Movie* ptr = movies;

for (int i = 0; i < 3; i++) {
    cout << ptr->title << endl;
    ptr++;
}

delete[] movies;
```

### Important caution

If you increment a pointer through an array:

- keep original start address somewhere if you need `delete[]`

Good:

```cpp
Movie* start = new Movie[3];
Movie* ptr = start;

// use ptr

delete[] start;
```

## 15. Common Theory Questions and Short Answers

### What is a function?

A named block of reusable code that may take parameters and may return a value.

### What is pass-by-value?

The function receives a copy of the argument, so changes do not affect the original variable.

### What is pass-by-reference?

The function receives an alias to the original variable, so changes affect the original.

### Why use `const` in parameters?

To prevent accidental modification and often to avoid unnecessary copying.

### What is recursion?

A technique where a function calls itself, usually with a smaller or simpler problem, until a base case is reached.

### What is a pointer?

A variable that stores the memory address of another object.

### What is dereferencing?

Using `*` on a pointer to access the value at the address it stores.

### What is a struct?

A user-defined type that groups related data members under one name.

### Difference between `.` and `->`

- `.` accesses a member of an object
- `->` accesses a member through a pointer to an object

### What is dynamic memory allocation?

Requesting memory at runtime using `new`, usually from the heap.

### What is a memory leak?

Allocated memory that is never freed.

### What is a dangling pointer?

A pointer that still points to memory that has already been deleted.

### What is an iterator?

An object used to move through elements of a container and identify positions in that container.

### What is STL?

The Standard Template Library: a library of containers, iterators, and algorithms.

### Difference between vector and list

- vector: dynamic array, fast random access
- list: linked list, efficient insertion/removal, no indexing

## 16. Common Practical Questions and Fast Templates

### Increment original integer

```cpp
void increment(int& x) {
    x++;
}
```

### Add two ints

```cpp
int add(int a, int b) {
    return a + b;
}
```

### Density

```cpp
double density(double width, double height, double depth, double mass) {
    return mass / (width * height * depth);
}
```

### Char to decimal ASCII

```cpp
int charToDecimal(char c) {
    return static_cast<int>(c);
}
```

### Random sum in range

```cpp
int sumRandom(int start, int end) {
    int total = 0;
    int range = end - start + 1;

    for (int i = 0; i < 100; i++) {
        int value = start + rand() % range;
        total += value;
    }

    return total;
}
```

### Sum array

```cpp
double sum(const double arr[], int length) {
    double total = 0;
    for (int i = 0; i < length; i++) {
        total += arr[i];
    }
    return total;
}
```

### Print first 10 Fibonacci numbers

```cpp
for (int i = 1; i <= 10; i++) {
    cout << fibonacci(i) << endl;
}
```

### Menu with switch

```cpp
int choice;
cin >> choice;

switch (choice) {
    case 1:
        cout << "Option 1" << endl;
        break;
    case 2:
        cout << "Option 2" << endl;
        break;
    default:
        cout << "Invalid choice" << endl;
}
```

### Draw rectangle

```cpp
for (int i = 0; i < 5; i++) {
    for (int j = 0; j < 10; j++) {
        cout << "* ";
    }
    cout << endl;
}
```

### Draw square

```cpp
for (int i = 0; i < 5; i++) {
    for (int j = 0; j < 5; j++) {
        cout << "* ";
    }
    cout << endl;
}
```

### Draw pyramid

```cpp
for (int i = 1; i <= 5; i++) {
    for (int s = 0; s < 5 - i; s++) {
        cout << " ";
    }
    for (int j = 0; j < i; j++) {
        cout << "* ";
    }
    cout << endl;
}
```

## 17. Paper-Writing Reminders

When writing by hand:

- write function declaration clearly
- write return type
- mark reference parameters with `&`
- mark read-only references with `const`
- write base case first for recursion
- when using files, show:
  - stream declaration
  - open/check
  - loop
  - close
- when using vectors, include:
  - declaration
  - `push_back`
  - loop
- when using pointers, show:
  - variable
  - pointer declaration
  - address operator `&`
  - dereference operator `*`
- when using dynamic memory, always show matching `delete` or `delete[]`

## 18. Easy-to-Forget Details

- array parameter needs separate length parameter
- `end()` is one past the last element
- do not dereference `end()`
- `[]` works on `vector`, not on `list`
- `std::sort()` works on `vector`, not on `list`
- use `->` with pointer to struct
- use `.` with actual struct object
- use `delete[]` for arrays allocated with `new[]`
- set pointer to `nullptr` after delete
- recursion needs base case
- pass-by-reference changes original
- pass-by-value does not
- `const` means “do not modify through this name”
- `getline()` reads spaces, `>>` usually stops at whitespace

## 19. Fast Compare Table

### Value vs reference vs pointer

Pass-by-value:

- copy
- original unchanged
- simple and safe

Pass-by-reference:

- alias to original
- original may change
- no null references in normal use

Pointer:

- stores address
- can be dereferenced
- can be `nullptr`
- explicit address passing

### Stack vs heap

Stack:

- automatic
- fast
- managed by language/runtime

Heap:

- dynamic
- manual management in this material
- flexible size
- risk of leaks

### Vector vs list

Vector:

- dynamic array
- random access
- supports `[]`
- great general choice

List:

- linked list
- no random access
- good for insertion/removal

## 20. Final Mini Checklist Before Exam

Be able to write from memory:

- a normal function
- a function with reference parameter
- a function with const reference parameter
- one recursive function
- one file reading loop
- one file writing example
- vector declaration and traversal
- iterator loop
- sort with lambda
- struct declaration and usage
- pointer basics
- dynamic memory with `new` and `delete`
- dynamic array with `new[]` and `delete[]`

If you can explain each section here and reproduce the short templates quickly, you are covering the main repeated material in your folder.
