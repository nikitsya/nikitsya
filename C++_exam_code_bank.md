# C++ Exam Code Bank

This file is a separate code bank for the written exam.

It focuses on repeated code patterns from your course materials:

- functions
- recursion
- file I/O
- vectors, lists, iterators, STL algorithms
- structs
- pointers
- dynamic memory

Use it as a supplement to the main cheat sheet.

## 1. Basic Template

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <list>
#include <string>
#include <algorithm>
#include <limits>

using namespace std;

int main() {
    return 0;
}
```

## 2. Function Templates

### Simple function returning a value

```cpp
int add(int a, int b) {
    return a + b;
}
```

### Void function

```cpp
void greet() {
    cout << "Hello" << endl;
}
```

### Pass-by-reference

```cpp
void increment(int& x) {
    x++;
}
```

### Swap by reference

```cpp
void swapValues(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

### Const reference parameter

```cpp
void printName(const string& name) {
    cout << name << endl;
}
```

### Default parameter

```cpp
int divide(int x, int d = 2) {
    return x / d;
}
```

### Function declaration before main

```cpp
int add(int, int);

int main() {
    cout << add(2, 3) << endl;
    return 0;
}

int add(int a, int b) {
    return a + b;
}
```

## 3. Basic Exam-Style Functions

### Density

```cpp
double density(double width, double height, double depth, double mass) {
    return mass / (width * height * depth);
}
```

### Char to ASCII decimal

```cpp
int charToDecimal(char c) {
    return static_cast<int>(c);
}
```

### Sum of 100 random values in range

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

### Sum random values and also store them in a vector

```cpp
int sumRandom(int start, int end, vector<int>& values) {
    int total = 0;
    int range = end - start + 1;

    for (int i = 0; i < 10; i++) {
        int value = start + rand() % range;
        values.push_back(value);
        total += value;
    }

    return total;
}
```

## 4. Recursion

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

### Print first 10 Fibonacci numbers

```cpp
for (int i = 1; i <= 10; i++) {
    cout << fibonacci(i) << endl;
}
```

### Divide by 2 recursively

```cpp
void divideRecursive(int x) {
    if (x < 1) {
        return;
    }
    cout << x << endl;
    divideRecursive(x / 2);
}
```

### Reverse string recursively

```cpp
string reverseString(const string& s) {
    if (s.empty()) {
        return "";
    }
    return reverseString(s.substr(1)) + s[0];
}
```

## 5. Menu and Switch

```cpp
int choice;

cout << "1. Option One" << endl;
cout << "2. Option Two" << endl;
cout << "Enter choice: ";
cin >> choice;

switch (choice) {
    case 1:
        cout << "Option One selected" << endl;
        break;
    case 2:
        cout << "Option Two selected" << endl;
        break;
    default:
        cout << "Invalid choice" << endl;
}
```

## 6. Loops and Small Patterns

### Multiplication / square pattern

```cpp
for (int i = 1; i <= 10; i++) {
    cout << i << " x " << i << " = " << i * i << endl;
}
```

### Rectangle

```cpp
for (int i = 0; i < 5; i++) {
    for (int j = 0; j < 10; j++) {
        cout << "* ";
    }
    cout << endl;
}
```

### Square

```cpp
for (int i = 0; i < 5; i++) {
    for (int j = 0; j < 5; j++) {
        cout << "* ";
    }
    cout << endl;
}
```

### Pyramid

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

### Border around text

```cpp
string text = "Hello";

for (int i = 0; i < text.length() + 4; i++) {
    cout << "*";
}
cout << endl;

cout << "* " << text << " *" << endl;

for (int i = 0; i < text.length() + 4; i++) {
    cout << "*";
}
cout << endl;
```

## 7. File I/O

### Write a name to file

```cpp
string name;
cout << "Enter name: ";
cin >> name;

ofstream fout("name.txt");
if (fout) {
    fout << name;
    fout.close();
}
```

### Read a name from file

```cpp
ifstream fin("name.txt");
string name;

if (fin) {
    fin >> name;
    fin.close();
    cout << name << endl;
}
```

### Read integers until end of file

```cpp
ifstream fin("nums.txt");
int x;

while (fin >> x) {
    cout << x << endl;
}

fin.close();
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

### Read student results and write final grades

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

### Read menu item line + price

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

## 8. Vector Basics

### Declaration and push_back

```cpp
vector<int> nums;
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
cout << nums.size() << endl;
```

### Loop by index

```cpp
for (int i = 0; i < nums.size(); i++) {
    cout << nums[i] << " ";
}
cout << endl;
```

### Range-based loop

```cpp
for (int x : nums) {
    cout << x << " ";
}
cout << endl;
```

### Read file into vector

```cpp
ifstream fin("nums.txt");
vector<int> nums;
int x;

while (fin >> x) {
    nums.push_back(x);
}

fin.close();
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

### Print vector of strings

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

## 9. List Basics

### Declaration and push_back

```cpp
list<int> nums;
nums.push_back(10);
nums.push_back(20);
nums.push_back(30);
```

### Read file into list

```cpp
ifstream fin("nums.txt");
list<int> nums;
int x;

while (fin >> x) {
    nums.push_back(x);
}

fin.close();
```

### Loop through list

```cpp
for (auto it = nums.begin(); it != nums.end(); ++it) {
    cout << *it << " ";
}
cout << endl;
```

## 10. Iterators

### Basic iterator use

```cpp
vector<int> nums = {10, 20, 30};
auto it = nums.begin();

cout << *it << endl;
++it;
cout << *it << endl;
```

### Iterator loop

```cpp
for (auto it = nums.begin(); it != nums.end(); ++it) {
    cout << *it << " ";
}
cout << endl;
```

### Constant iterator

```cpp
for (auto it = nums.cbegin(); it != nums.cend(); ++it) {
    cout << *it << " ";
}
```

### Reverse iterator

```cpp
for (auto it = nums.rbegin(); it != nums.rend(); ++it) {
    cout << *it << " ";
}
cout << endl;
```

### Insert using iterator

```cpp
vector<string> months = {"Jan", "Apr", "May"};
auto it = months.begin();
++it;
months.insert(it, "Feb");
```

### Advance iterator

```cpp
auto it = nums.begin();
advance(it, 2);
cout << *it << endl;
```

## 11. STL Algorithms

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

### Count exact value

```cpp
int result = count(nums.begin(), nums.end(), 18);
```

### Count with condition

```cpp
int result = count_if(nums.begin(), nums.end(), [](int x) {
    return x < 18;
});
```

### Find value

```cpp
auto it = find(nums.begin(), nums.end(), 17);

if (it != nums.end()) {
    cout << "Found" << endl;
}
```

### Find first even value

```cpp
auto it = find_if(nums.begin(), nums.end(), [](int x) {
    return x % 2 == 0;
});
```

### all_of

```cpp
bool ok = all_of(nums.begin(), nums.end(), [](int x) {
    return x > 0;
});
```

### none_of

```cpp
bool ok = none_of(nums.begin(), nums.end(), [](int x) {
    return x < 0;
});
```

### Remove even values from vector while iterating

```cpp
for (auto it = nums.begin(); it != nums.end(); ) {
    if (*it % 2 == 0) {
        it = nums.erase(it);
    } else {
        ++it;
    }
}
```

### Compare two vectors

```cpp
vector<int> a = {1, 2, 3};
vector<int> b = {1, 2, 3};

if (a == b) {
    cout << "Same" << endl;
}
```

## 12. Repeated Vector Tasks from Exercises

### areSameUntil

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

### areSameReverse

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

### interleave

Assume both input vectors have the same size.

```cpp
void interleave(const vector<int>& v1, const vector<int>& v2, vector<int>& out) {
    out.clear();

    for (size_t i = 0; i < v1.size(); i++) {
        out.push_back(v1[i]);
        out.push_back(v2[i]);
    }
}
```

### Insert missing months

```cpp
vector<string> months = {"Jan", "Apr", "May", "Sep", "Nov", "Dec"};

auto it = months.begin();
++it;
it = months.insert(it, "Mar");
it = months.insert(it, "Feb");
it = it + 4;
it = months.insert(it, "Aug");
it = months.insert(it, "Jul");
it = months.insert(it, "Jun");
it = it + 4;
months.insert(it, "Oct");
```

## 13. Struct Basics

### Simple struct

```cpp
struct Movie {
    string title;
    int year;
};
```

### Declare and use object

```cpp
Movie movie1;
movie1.title = "Jaws";
movie1.year = 1975;
```

### Pass struct by value

```cpp
void displayMovie(Movie movie) {
    cout << movie.title << " " << movie.year << endl;
}
```

### Pass struct by const reference

```cpp
void displayMovie(const Movie& movie) {
    cout << movie.title << " " << movie.year << endl;
}
```

## 14. Tweet Struct Pattern

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

### Populate one Tweet

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
    cout << "ID: " << tweet.id << endl;
    cout << "Author: " << tweet.author << endl;
    cout << "Message: " << tweet.message << endl;
    cout << "Timestamp: " << tweet.timestamp << endl;
    cout << "Location: (" << tweet.loc.lat << ", " << tweet.loc.lng << ")" << endl;
}
```

### Display by const reference

```cpp
void displayByReference(const Tweet& tweet) {
    cout << "ID: " << tweet.id << endl;
    cout << "Author: " << tweet.author << endl;
    cout << "Message: " << tweet.message << endl;
    cout << "Timestamp: " << tweet.timestamp << endl;
    cout << "Location: (" << tweet.loc.lat << ", " << tweet.loc.lng << ")" << endl;
}
```

### Array of Tweet structs

```cpp
Tweet tweets[3];

tweets[0].author = "A";
tweets[0].id = 1;

tweets[1].author = "B";
tweets[1].id = 2;

tweets[2].author = "C";
tweets[2].id = 3;
```

### Display array of structs

```cpp
void displayArray(Tweet arr[], int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i].id << " " << arr[i].author << endl;
    }
}
```

## 15. Rgb Struct Pattern

```cpp
struct Rgb {
    float r, g, b;

    Rgb() : r(0), g(0), b(0) {}
    Rgb(float rr, float gg, float bb) : r(rr), g(gg), b(bb) {}
};
```

### Display pixel

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

## 16. Pointer Basics

### Pointer declaration

```cpp
int x = 25;
int* p = &x;
```

### Print address and value

```cpp
cout << x << endl;
cout << &x << endl;
cout << p << endl;
cout << *p << endl;
```

### Modify through pointer

```cpp
*p = 99;
cout << x << endl;
```

### Good practice

```cpp
int* p = nullptr;
```

### Pointer parameter increment

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

### Divide using pointer parameters

```cpp
double divide(const int* p1, const int* p2) {
    return *p1 / static_cast<double>(*p2);
}
```

Call:

```cpp
int a = 8;
int b = 3;
cout << divide(&a, &b) << endl;
```

## 17. Arrays and Pointers

### Sum using array notation

```cpp
double sumArray(const double arr[], int length) {
    double total = 0;
    for (int i = 0; i < length; i++) {
        total += arr[i];
    }
    return total;
}
```

### Sum using pointer notation

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

### Display using pointer notation

```cpp
void displayPointer(const double* ptr, int length) {
    for (int i = 0; i < length; i++) {
        if (i != 0) {
            cout << ", ";
        }
        cout << *ptr;
        ptr++;
    }
    cout << endl;
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

### Copy one array into another using pointers

```cpp
void copyArray(int* target, int* source, int length) {
    for (int i = 0; i < length; i++) {
        *target = *source;
        target++;
        source++;
    }
}
```

## 18. strcmp-Style C String Comparison

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

Example:

```cpp
char str1[] = "Are these the same?";
char str2[] = "Are these the sane?";

cout << strcmp2(str1, str2) << endl;
```

## 19. Dynamic Memory

### Allocate one int

```cpp
int* p = new int;
*p = 25;
cout << *p << endl;

delete p;
p = nullptr;
```

### Allocate one char

```cpp
char* c = new char;
*c = 'F';
cout << *c << endl;

delete c;
c = nullptr;
```

### Allocate dynamic array

```cpp
int size = 5;
int* arr = new int[size];

for (int i = 0; i < size; i++) {
    arr[i] = i * 10;
}

for (int i = 0; i < size; i++) {
    cout << arr[i] << endl;
}

delete[] arr;
arr = nullptr;
```

### Display dynamic array using pointer notation

```cpp
void displayPointer(int* ptr, int size) {
    for (int i = 0; i < size; i++) {
        cout << *ptr << endl;
        ptr++;
    }
}
```

## 20. Dynamic Memory for Structs

### Dynamically allocate one struct

```cpp
struct Movie {
    string title;
    int year;
};

Movie* moviePtr = new Movie;
moviePtr->title = "Alien";
moviePtr->year = 1979;

cout << moviePtr->title << endl;
cout << moviePtr->year << endl;

delete moviePtr;
moviePtr = nullptr;
```

### Dynamically allocate array of structs

```cpp
int size = 3;
Movie* movies = new Movie[size];

movies[0].title = "Jaws";
movies[0].year = 1975;

movies[1].title = "Alien";
movies[1].year = 1979;

movies[2].title = "Arrival";
movies[2].year = 2016;

for (int i = 0; i < size; i++) {
    cout << movies[i].title << " " << movies[i].year << endl;
}

delete[] movies;
movies = nullptr;
```

### Pointer loop through struct array

```cpp
Movie* start = new Movie[3];

start[0].title = "A";
start[1].title = "B";
start[2].title = "C";

Movie* ptr = start;

for (int i = 0; i < 3; i++) {
    cout << ptr->title << endl;
    ptr++;
}

delete[] start;
```

## 21. Combined Harder Patterns

### Read integers into vector, sort, print

```cpp
ifstream fin("nums.txt");
vector<int> nums;
int x;

while (fin >> x) {
    nums.push_back(x);
}

fin.close();

sort(nums.begin(), nums.end());

for (int value : nums) {
    cout << value << " ";
}
cout << endl;
```

### Read strings, sort by length, print

```cpp
vector<string> words = {"this", "is", "a", "long", "line", "of", "strings"};

sort(words.begin(), words.end(), [](const string& a, const string& b) {
    return a.size() < b.size();
});

for (const string& word : words) {
    cout << word << " ";
}
cout << endl;
```

### Remove all failing marks from vector

```cpp
vector<int> grades = {10, 90, 30, 60, 35, 50};

for (auto it = grades.begin(); it != grades.end(); ) {
    if (*it < 40) {
        it = grades.erase(it);
    } else {
        ++it;
    }
}
```

### Count values matching rule

```cpp
vector<int> ages = {18, 17, 21, 18, 21, 16};

int result = count_if(ages.begin(), ages.end(), [](int x) {
    return x < 18;
});

cout << result << endl;
```

### Find first even value

```cpp
vector<int> nums = {1, 5, 9, 8, 7};

auto it = find_if(nums.begin(), nums.end(), [](int x) {
    return x % 2 == 0;
});

if (it != nums.end()) {
    cout << *it << endl;
}
```

## 22. Fast Exam Reminders

- use `&` for reference parameters
- use `const` when function should not modify argument
- recursion needs base case
- `ifstream` reads files
- `ofstream` writes files
- `while (fin >> x)` is the safest input loop
- `vector` supports `[]`
- `list` does not support `[]`
- iterators use `begin()` and `end()`
- `end()` is one past the last element
- use `->` with pointer to struct
- use `.` with object
- `new` pairs with `delete`
- `new[]` pairs with `delete[]`
- set pointer to `nullptr` after delete
