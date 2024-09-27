+++
title = "Unboxing Integers"
date = 2024-05-05
+++

# Unboxing Integers

Acton's new integer unboxing feature can speed up number-heavy applications by 100x or more. In this post, we'll break down what integer unboxing is, how it works, and why it matters for your performance-critical code.

**Boxed (yellow) vs Unboxed (green) vs Python (Yellow)**
[![](/blog/boxed-vs-unboxed.png)](/blog/boxed-vs-unboxed.png)

## Boxed values

In many programming languages, values like integers or strings are stored in a boxed fashion. The box is a structure in memory that holds the type of the value, like "signed 64 bit integer", as well as the actual value, like `42`. This is different than the most basic representation in computer memory, which is really just the value itself, like a series of 64 bits, without the meta-data of what type the value is of. We can think of this basic representation as "unboxed", there is no box to tell us what type of value is stored, it's just some stored bits. The downside of such unboxed values is that you have to keep track of the type of a value by some other means. When writing C programs, this is natural and just the way things works, like we know that variable `a` is of type `int` in the program code, so no need to also repeat that in memory but it does require the programmer to bear this burden. With higher level languages and in particular those that support generic types, it becomes necessary to keep track of value types and using boxed types is the simplest means to achieving this. Acton uses boxed values in general. 

There's nothing wrong with boxed values, it's just that it comes with some performance overhead, so for compute intensive code, this can add up to make quite a difference. We might end up spending more time looking at the box and following pointers than doing the actual computation.

For a contemporary 64 bit CPU, a *machine word*, the normal sized chunk that the CPU deals with, is 64 bits and so any type that is 64 bits or smaller can be passed around directly. C ABI calling convention typically passes a 64 bit integer argument value to a function using a CPU register, which is incredibly fast. In contrast, a boxed value is instead sent as a pointer to the box and then the receiving function has to fetch the box and then the value from memory, which can be considerably slower.


## Unboxing boxed integers

Unboxing is the process of removing the box around a value, allowing the value to be passed and operated on directly without any extra layers of indirection. In Acton, integers were traditionally stored as boxed values—meaning that instead of simply passing the raw number around, we were also managing metadata (such as the type of value) along with the value itself. This made certain operations, especially mathematical ones, slower than necessary.

By unboxing these integers, we can bypass this overhead. When an integer is unboxed, it’s stored and passed around as a raw, untagged machine word. This is similar to how a language like C would handle primitive types such as int. The result? Faster memory access and improved computational performance, especially for tight loops and arithmetic-heavy code.

The Acton compiler now has support for automatically detecting when an integer can be unboxed, and it will generate optimized C code accordingly. This allows you to write clean, expressive code while still benefiting from low-level performance optimizations under the hood.

In this first iteration, unboxing is supported for local variables and variables passed as arguments between functions. Object attributes are always boxed.


## Optimal code with unboxed integers - Discrete Cosine Transform

The discrete cosine transform is a very popular and widely used algorithm used for image compression. And here it is, in an Acton version:

```python
import math

def dct(k,n,l):
    return math.cos(math.pi/l * (n + 0.5) * k)

def dct_sum(l):
    s = 0
    k = 0
    while k < l:
        n = 0
        while n < l:
            s += dct(k,n,l)
            n += 1
        k += 1
    return s
```

The values operated on here are i64 and with the new support in the Acton compiler for unboxed values, the generated C code is the following:

```c
double dctQ_U_dct (double U_1k, double U_2n, double U_3l) {
    double U_4N_tmp = cos((((mathQ_pi->val / U_3l) * (U_2n + 0.5)) * U_1k));
    return U_4N_tmp;
}
double dctQ_U_5dct_sum (double U_6l) {
    double U_7s = 0;
    double U_8k = 0;
    while ((U_8k < U_6l)) {
        double U_9n = 0;
        while ((U_9n < U_6l)) {
            U_7s += dctQ_U_dct(U_8k, U_9n, U_6l);
            U_9n += 1;
        }
        U_8k += 1;
    }
    return U_7s;
}
```

Which is pretty much as simple and optimal as one would write by hand, bar the naming convention. In other words, using Acton, which can certainly be considered a higher level language than C, comes with essentially no overhead for this program. Boxed values are doubly bad because they also exert a massive pressure on the memory subsystem, both when allocating on the heap and when the GC subsystem is collecting garbage. This makes a tremendous difference in performance, especially for larger values where memory consumption grows. With unboxing, we avoid the extra memory allocation and instead store values on the stack and pass them to functions via CPU registers.


## Doubly good for the GC and memory subsystem

By not allocating memory for boxed values, we avoid the need for the garbage collector to clean up after them. This is a double win for performance, as the garbage collector can be a significant bottleneck in many applications. Acton currently uses the Boehm GC (but not for long!), which is a mark-sweep collector with a stop-the-world. All Acton Run Time System worker threads are stopped while running the GC which means that otherwise parallelizable code might not end up being very fast because we become concurrency constrained around GC. Using unboxed values avoids malloc & GC altogether and so our application can easily run concurrently.

## Performance comparison

In this graph we can see the performance difference between the boxed and unboxed versions of the DCT program. Depending on the input size, the unboxed version can be 100x or even faster. Python is also included in this comparison where it scores better than the boxed version but still significantly slower than the unboxed version.

**Boxed (red) vs Unboxed (green) vs Python (Yellow)**
[![](/blog/boxed-vs-unboxed-vs-python.png)](/blog/boxed-vs-unboxed-vs-python.png)

If we remove Acton's boxed version, we can zoom in on the unboxed and Python versions. Acton's unboxed version is, by quite a fair margin, consistently faster than Python, for all input sizes. The difference becomes more pronounced as the input size grows and at 10000, Acton is ~15x faster than Python.

**Unboxed (green) vs Python (yellow)**
[![](/blog/unboxed-vs-python.png)](/blog/unboxed-vs-python.png)

Many computationally intensive applications are written in C and receive a thin wrapper in Python or some other high level language. Now you can write the entire application in Acton and still get the performance of C!

## Future work

The current implementation of unboxing in Acton supports local variables and function arguments. In the future, we plan to extend this support to object and actor attributes as well. This will further improve the performance of Acton programs, especially those that rely heavily on object-oriented programming.

Further, monomorphization is a technique that can be used to specialize generic functions for specific types. This can be used to optimize the performance of functions that are called with a small number of different types. We plan to explore monomorphization in Acton to further improve the performance of generic functions.

Until then, enjoy the speed boost that unboxed integers bring to your Acton programs!
