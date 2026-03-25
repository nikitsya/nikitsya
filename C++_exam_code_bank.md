# C++ Exam Extra Code Only

This file contains only extra snippets that are useful to add after the main cheat sheet.

It avoids the basic repeated patterns and keeps only additional code ideas.

## 1. Function Declaration Before `main()`

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

## 2. Insert Missing Months with Iterators

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

## 3. Copy One Array into Another Using Pointers

```cpp
void copyArray(int* target, int* source, int length) {
    for (int i = 0; i < length; i++) {
        *target = *source;
        target++;
        source++;
    }
}
```

## 4. Dynamic Allocation of a Single `char`

```cpp
char* c = new char;
*c = 'F';

cout << *c << endl;

delete c;
c = nullptr;
```

## 5. Pointer Walk Through Array of Structs

```cpp
struct Movie {
    string title;
    int year;
};

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

## 6. Compare Two Vectors Directly

```cpp
vector<int> a = {1, 2, 3};
vector<int> b = {1, 2, 3};

if (a == b) {
    cout << "Same" << endl;
}
```

## 7. Sort File Data Immediately After Reading

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

## 8. Sort Strings by Length and Print

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

## 9. Find First Even Value

```cpp
vector<int> nums = {1, 5, 9, 8, 7};

auto it = find_if(nums.begin(), nums.end(), [](int x) {
    return x % 2 == 0;
});

if (it != nums.end()) {
    cout << *it << endl;
}
```

## 10. Remove Failing Values from a Vector

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
