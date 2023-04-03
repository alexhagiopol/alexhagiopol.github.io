---
title: 'Notes on A Tour of C++ 3rd Edition'
date: 2023-04-02
permalink: /posts/2023/04/a-tour-of-cpp-notes/
tags:
  - C++
---

This (living) article contains my notes from *[A Tour of C++ 3rd Edition](https://www.pearson.com/en-us/subject-catalog/p/tour-of-c-a/P200000002116/9780136816485){:target="_blank"}* by Bjarne Stroustrup. Last updated April 2nd, 2023. 

## Chapter 18: Concurrency

### 18.1 Introduction

- Purposes of concurrency: (a) improve throughput by using several processors for a single computation, (b) improve responsiveness by allowing one part of a program to progress while another is waiting for a response.
- STL aims to provide utilities for systems-level concurrency.
- STL directly supports concurrent execution of multiple threads in a single address space i.e. intra-process concurrency. C++ provides a suitable memory model and atomic operations to enable that. Lock free programming is possible because of atomic operations.
- Features such as `thread`s, `mutex`es, `lock()` operations, `packaged_task`s, and `future`s are built directly upon what operating systems offer. *They do not incur performance penalties compared with those. Neither do they guarantee significant performance improvements.*

### 18.2 Tasks and Threads

- *Task* = computation that can potentially be executed concurrently with other computations. In code, a task can be a function or a function object.
- *Thread* = system-level representation of a task in a program. Threads of a program share a single address space. This is different from processes which generally do not directly share data.

```
#include <thread>

void function();                    // function that represents a task

struct FunctionObj {
    void operator()();              // call operator
}

void user() {
    std::thread t1{function};      // function() executes in a separate thread        
    std::thread t2{FunctionObj{}}; // FunctionObj{}() executes in a separate thread
    t1.join();                      // wait for t1 
    t2.join();                      // wait for t2
}
```

- `join()` means "wait for a thread to terminate". The STL provides `std::jthread` whose destructor calls `join()` so you don't forget.

### 18.2.1 Passing Arguments
```
#include <functional>
#include <thread>

void function(std::vector<double>& vec);

struct FunctionObj {
    std::vector<double>& vec;
    FunctionObj(std::vector<double>& vec_) : vec{_vec} {}
    void operator()();
}

int main() {
    std::vector<double> myVec1{1, 2, 3, 4};
    std::vector<double> myVec2{1, 2, 3, 4};
    std::jthread t1{function, std::ref(myVec1)};  // #include <functional> and use std::ref() to explicitly indicate pass by reference to jthread's variadic template constructor. not required for passing arguments by value.
    std::jthread t2{FunctionObj(myVec2)};  // function objects can be constructed inline, so std::ref() not needed to pass by reference here.
```

### 18.3 Sharing Data

- There is no problem with many tasks simultaneously reading immutable data.
- *`mutex`* = mutual exclusion object.

```
#include <mutex>
#include <thread>

std::mutex myMutex;
int sharedData;

void function() {
    std::scoped_lock myLock{myMutex};
    sharedData++;
    // release mutex implicitly
}
``` 

- `scoped_lock` uses RAII: constructor locks the mutex passed as an argument, any thread that attempts to construct a lock using that mutex will have to wait, upon destruction, `scoped_lock` unlocks the mutex.
- In the above case, the programmer has to remember which data is locked by which mutex. One improvement is to make a mutex a class member to imply it has to be locked before accessing the rest of the class:

```
class MyData {
public:
    std::mutex myMutex;
    // ...
}
```

- *Deadlock*= `thread1` has locked `mutex1`. `thread1` needs data guarded by `mutex2`. `thread2` has locked `mutex2`. `thread2` needs data guarded by `mutex1`. These threads will wait forever.
- To help prevent deadlocks, `scoped_lock` can acquire multiple locks simultaneously:

```
std::scoped_lock{mutex1, mutex2, mutex3};
```

- `std::scoped_lock` will proceed only after acquiring all its mutexes and will never block while holding a mutex. The destructor releases all mutexes.
- Advice: Some people are convinced that communicating through shared data is more efficient than calls and returns. Locking and unlocking are relatively expensive operations. Don't choose shared data for communication because of "efficiency" without thought and preferably measurement.
- `std::mutex` allows only one thread at a time to access data. `std::shared_mutex` enables implementing the "reader-writer lock" idiom in which there are many simultaneous readers with non-exclusive access and a single writer with exclusive access:

```
std::shared_mutex sharedMutex;

void read() {
    std::shared_lock sharedLock(sharedMutex);
    // read operations
}

void write() {
    std::unique_lock uniqueLock(sharedMutex);
    // write operations
}
```

- Use `std::shared_lock` for non-exclusive access.
- Use `std::unique_lock` for exclusive access.
- `std::lock_guard` uses RAII for locking a single mutex.
- `std::scoped_lock` uses RAII for locking one or more mutexes.

### 18.3.2 atomics

- An `std::mutex` is a fairly heavy weight mechanism involving the OS that allows arbitrary amounts of work to be doClassX // non-trivial initialization

if (!flag) {
    std::lock_guard{myMutex};
    if (!flag) {
        // do non trivial initialization of object of ClassX
        objectX();
        flag = true;
    }
}

// use objectX
```
