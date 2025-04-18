---
title: Difference between '[[]] * 10' and '[[] for _ in range(10)]' in Python.
date: 2025-04-13
categories: [Technical]
tags: [python]
permalink: /:year/:month/:title.html
description: "What does this two python list creation method do at the fundamental level? we will explore this in this article by deep diving into cpython codebase!"
---

## Context
I have a habit of avoiding syntactic sugars of python unless absolutely necessary or if I am feeling lazy!
It's not like I have a bad opinion of them. I think some of them are highly optimized which could help you leverage the unique features and efficiency of Python where it matters.

Recently I was writing a script where I decided to use this syntactic sugar of python which I assumed would create a list of lists. 
Well to be fair it does.

```python
list_of_lists = [[]] * 10
list_of_lists[0].append(1)
print(list_of_lists)
```

### Expected
I assumed that the above code would result into the first sublist having a single element appended to it like
```python
[[10], [], [], [], [], [], [], [], [], []]
```

### Actual Result
But to my surpirise it resulted in this
```python
[[10], [10], [10], [10], [10], [10], [10], [10], [10], [10]]
```

Well it was into a subroutine which was doing many other things, so it took me a while but it was not long before I figured out that all of this lists pointed to a single memory address. As verified by the below routine.


```python
>>> register = [[]] * 10
>>> for i in range(10):
...     print(id(register[i]))
...
1728395547200
1728395547200
1728395547200
1728395547200
1728395547200
1728395547200
1728395547200
1728395547200
1728395547200
1728395547200
```

It all made sense, But I had this itch to see how it was really implemented under the hood. 
So I did what any productivity guru and pragmatic person would tell you do,<br/> 
Checkout the Python Interpreter Source Code 😤

## Implementation
I head over to the [cpython repo](https://github.com/python/cpython) on GitHub

In CPython the below two method in `listobject.c` file that are responsible for creating the list when you write the list multiplication syntax like `[[]] * 10`

```c
static PyObject *
list_repeat_lock_held(PyListObject *a, Py_ssize_t n)
{
    const Py_ssize_t input_size = Py_SIZE(a);
    if (input_size == 0 || n <= 0)
        return PyList_New(0);
    assert(n > 0);

    if (input_size > PY_SSIZE_T_MAX / n)
        return PyErr_NoMemory();
    Py_ssize_t output_size = input_size * n;

    PyListObject *np = (PyListObject *) list_new_prealloc(output_size);
    if (np == NULL)
        return NULL;

    PyObject **dest = np->ob_item;
    if (input_size == 1) {
        PyObject *elem = a->ob_item[0];
        _Py_RefcntAdd(elem, n);
        PyObject **dest_end = dest + output_size;
        while (dest < dest_end) {
            *dest++ = elem;
        }
    }
    else {
        PyObject **src = a->ob_item;
        PyObject **src_end = src + input_size;
        while (src < src_end) {
            _Py_RefcntAdd(*src, n);
            *dest++ = *src++;
        }
        // TODO: _Py_memory_repeat calls are not safe for shared lists in
        // GIL_DISABLED builds. (See issue #129069)
        _Py_memory_repeat((char *)np->ob_item, sizeof(PyObject *)*output_size,
                                        sizeof(PyObject *)*input_size);
    }

    Py_SET_SIZE(np, output_size);
    return (PyObject *) np;
}

static PyObject *
list_repeat(PyObject *aa, Py_ssize_t n)
{
    PyObject *ret;
    PyListObject *a = (PyListObject *)aa;
    Py_BEGIN_CRITICAL_SECTION(a);
    ret = list_repeat_lock_held(a, n);
    Py_END_CRITICAL_SECTION();
    return ret;
}
```

Okay, enough of Egyptian Hyerogliphics, let's come to the point.
What is it actually doing?

When evaluating `[[]] * 10` *list_repeat* function is called.
Here,
*PyObject \*aa* = `[]` 
*Py_ssize_t n* = 10

```c
PyObject *ret;
PyListObject *a = (PyListObject *)aa;
```
- This declares a new `PyObject` for returning
- Creates a `PyListObject` and typecasts the existing `*aa` parameter to `PyListObject` that is our `[]` in this case

Now we come to the part where it calls the `list_repeat_lock_held` function for creating the list by passing these two parameters

- our object reference `a` which ultimately is a pointer to `aa`
- the multiplier number (In our case it's 10)

```c
ret = list_repeat_lock_held(a, n);
```

---

```c
    const Py_ssize_t input_size = Py_SIZE(a);
    if (input_size == 0 || n <= 0)
        return PyList_New(0);
    assert(n > 0);
```

In this we assess the size of current list object that we created. 
If the size of our list is 0 or the multiplier is less than equal to zero then we just return an empty list

Which basically means,
If you had for example performed `[] * 10` or `[[]] * 0` or `[[]] * -29` the result would be `[]`

Don't belive me? Let's put it to test!

```python
>>> [] * 10
[]
>>> [[]] * 0
[]
>>> [[]] * -29
[]
>>> [] * 0
[]
>>> [] * -9888
[]
```

As you can see, as expected we got a single newly created python list of length 0

Next, It needs to allocate memory for our new list<br/>
But before it does that<br/>
we have a defensive check to prevent integer overflow
```c
    if (input_size > PY_SSIZE_T_MAX / n)
        return PyErr_NoMemory();
    Py_ssize_t output_size = input_size * n;
```

Here, `PY_SSIZE_T_MAX` is the max value of a `long int` 

You can verify it in C through `limits.h` using below snippet
```c
#include <stdio.h>
#include <limits.h>

int main() 
{
    printf("The max size of a long is : %ld\n", LONG_MAX); 
}

// outputs
// The max size of a long is : 9223372036854775807
```

OR in Python using `sys.maxsize` from `sys` module 

```python
>>> import sys
>>> sys.maxsize
9223372036854775807
```

This should mean if I tring 

Now the actual size might differ based on your CPU architecture, if you are using a 64-bit machine then it would likely be the same as I have shown.

For more details, you can also check it's C implementation in [sysmodule.c](https://github.com/python/cpython/blob/9e52c907b5511393ab7e44321e9521fe0967e34d/Python/sysmodule.c#L1985-L1986)

Trying, not to get too nerdy here...<br/>
Let's stick to our original topic, I might create a seperate article on how python's `sys` module is written in C.

After our defensive check, now it finally allocates the memory for our new list object

```c
    Py_ssize_t output_size = input_size * n;

    PyListObject *np = (PyListObject *) list_new_prealloc(output_size);
    if (np == NULL)
        return NULL;
```

Here, we observe something interesting, there is a check for null value, in what would this `list_new_prealloc` function return a **NULL** value??
As we can see in the below function snippets if our `output_size` is negative then it results in returning of NULL.

```c
static PyObject *
list_new_prealloc(Py_ssize_t size)
{
    assert(size > 0);
    PyListObject *op = (PyListObject *) PyList_New(0);
    if (op == NULL) {
        return NULL;
    }
    ...
}

PyObject *
PyList_New(Py_ssize_t size)
{
    if (size < 0) {
        PyErr_BadInternalCall();
        return NULL;
    }
    ...
}
```

Now we come to the main part where the list is actually made and returned to the user

```c
    PyObject **dest = np->ob_item;
    if (input_size == 1) {

        PyObject *elem = a->ob_item[0];
        _Py_RefcntAdd(elem, n);
        PyObject **dest_end = dest + output_size;
        while (dest < dest_end) {
            *dest++ = elem;
        }
    }
    else {
        PyObject **src = a->ob_item;
        PyObject **src_end = src + input_size;
        while (src < src_end) {
            _Py_RefcntAdd(*src, n);
            *dest++ = *src++;
        }
        // TODO: _Py_memory_repeat calls are not safe for shared lists in
        // GIL_DISABLED builds. (See issue #129069)
        _Py_memory_repeat((char *)np->ob_item, sizeof(PyObject *)*output_size,
                                        sizeof(PyObject *)*input_size);
    }

    Py_SET_SIZE(np, output_size);
    return (PyObject *) np;
```
### Single element list
Here we see if the list size is one element 
Then it explictly  only gets the first element inside of it and then keeps adding it into the array until it reach the output size.

(**Remember** the list size is the size of list object *PyListObject *a* which we passed into the parameter.)

Let's see how this is being done.
```c
        PyObject *elem = a->ob_item[0];
        _Py_RefcntAdd(elem, n);
        PyObject **dest_end = dest + output_size;
        while (dest < dest_end) {
            *dest++ = elem;
        }
```
1. We select the first element of the list (remember it's a one element list), We can also say that `*elem* is the pointer to the 1st element of the list.
2. We use `_Py_RefcntAdd(elem, n);` to increase the number of references associated with that object, 
Now this is an interesting one, In Python every object (PyObject) internally has an unsigned 32-bit integer property `ob_refcnt` to track how many references are tied to this particular object, 
for example, I have an object `snow_bell` of `Cat` class instance, Now `snow_bell` can be a inside a list with his friendly neighbourhood cats, a dictionary, another class. So each time a new data structure points to it, Python internally increases this count. If ever the count reaches to `0` then the object is marked for garbage collection so that Python can free up the memory. In Python we also have something called `Immortal Objects` that are never garbage collected like `None`, `True` and a seemingly normal object can rise to the level of `Immortal` under some conditions. But that's the topic for another article, You can read the source code of `_Py_RefcntAdd` to dig deeper.

You can also check it using below code in Python shell
```python
>>> class Cat:
...     def __init__(self, name):
...         self.name = name
...
>>> snow_bell = Cat("Snow Bell")

>>> import sys
>>> sys.getrefcount(snow_bell)
2
```
`sys.getrefcount` functions gives us the internal reference count of an object. The reason why it says `2` count because firstly we have a reference `snow_bell` variable which itself is also just a pointer to that memory address and secondly to get the count, `getrefcount` function itself creates a temporary pointer to the object, this results in one additional pointer. 

3. Now we are essentially calculating the last memory address of the list. We already had the start pointer of the list when we created a pointer to the first element of the list.
```c
    PyObject **dest = np->ob_item[0]; 
```

Now when we have to calculate the supposed last memory address of the array after we finish up operation we can simply add that size into our existing address.
```c
        PyObject **dest_end = dest + output_size;
```
So essentially when we are doing `dest + output_size` the compiler is essentially calculating the last element memory address, this might look sort of deceiving because we aren't explictly specifying the size of Python object, in explicit terms we should be adding `output_size * sizeof(PyObject)` to the `dest`. But the compiler takes care of that and does the appropirate arithmetic for calculating.

4. Now we run a damn simple while loop to copy the same memory reference on our entire array until we reach our new memory location `dest_end`
```c
        while (dest < dest_end) {
            *dest++ = elem;
        }
```

### Multi element list
To recap, earlier before entering this conditional. 

We had already calcualted `output_size` that is the size of output list we will create and also preallocated the required memory in for our `PyListObject` that we will be sending back.

```c
    Py_ssize_t output_size = input_size * n;
    PyListObject *np = (PyListObject *) list_new_prealloc(output_size);
    if (np == NULL)
        return NULL;
```

this was our `dest`, which is essentially the pointer to our python list
```c
    PyObject **dest = np->ob_item;
```

Now we come to the part where where our source list has multiple elements

```c
        PyObject **src = a->ob_item;
        PyObject **src_end = src + input_size;
        while (src < src_end) {
            _Py_RefcntAdd(*src, n);
            *dest++ = *src++;
        }
        // TODO: _Py_memory_repeat calls are not safe for shared lists in
        // GIL_DISABLED builds. (See issue #129069)
        _Py_memory_repeat((char *)np->ob_item, sizeof(PyObject *)*output_size, sizeof(PyObject *)*input_size);
```

Let's take the below example
The line to interpret is `[[10], [20]] * 10`

The input list is [[10], [20]], and the multiplier is 10. This means:
•	Input size (input_size): 2 (the number of elements in the input list).
•	Output size (output_size): input_size * n = 2 * 10 = 20.
The function list_repeat_lock_held is called with:
•	a: The input list [[10], [20]].
•	n: The multiplier 10.

•	src: Points to the start of the input list ([[10], [20]]).
•	src_end: Points to the end of the input list (src + input_size).

**Iteration 1**:
•	*src points to [10].
•	_Py_RefcntAdd(*src, n) increments the reference count of [10] by 10.
•	*dest = *src copies the reference to [10] into the output list.
•	src and dest are incremented.

**Iteration 2**:
•	*src points to [20].
•	_Py_RefcntAdd(*src, n) increments the reference count of [20] by 10.
•	*dest = *src copies the reference to [20] into the output list.
•	src and dest are incremented.

At the end of this loop:
•	The first 2 slots of np->ob_item contain references to [10] and [20].
•	The remaining 18 slots are still NULL.

**Repeat References Using _Py_memory_repeat**
```c
_Py_memory_repeat((char *)np->ob_item, sizeof(PyObject *) * output_size, sizeof(PyObject *) * input_size);
```

•	dest: The memory region starting at np->ob_item.
•	len_dest: sizeof(PyObject *) * output_size = sizeof(PyObject *) * 20.
•	len_src: sizeof(PyObject *) * input_size = sizeof(PyObject *) * 2.

```c
// Repeat the bytes of a buffer in place
static inline void
_Py_memory_repeat(char* dest, Py_ssize_t len_dest, Py_ssize_t len_src)
{
    assert(len_src > 0);
    Py_ssize_t copied = len_src;
    while (copied < len_dest) {
        Py_ssize_t bytes_to_copy = Py_MIN(copied, len_dest - copied);
        memcpy(dest + copied, dest, (size_t)bytes_to_copy);
        copied += bytes_to_copy;
    }
}
```

_Py_memory_repeat efficiently repeats a block of memory (like references to list elements) to fill a larger memory region. Here's what it does:
1.	It starts with a memory region (dest) that already contains the first len_src bytes (example: references to the input list elements).
2.	Doubling Strategy: In each iteration, it doubles the size of the copied region by copying the already-copied portion of dest to the next available position.
3.	It ensures that no more than len_dest bytes are written by calculating the smaller of the remaining space (len_dest - copied) and the size of the already-copied region (copied).
4.	Logarithmic Efficiency: The function repeats the memory in logarithmic steps, minimizing the number of memcpy calls.
This is used in list repetition (e.g., [[10], [20]] * 10) to efficiently duplicate references to the input list elements in the output list.
•	_Py_memory_repeat doubles the copied region in each iteration, making it logarithmically efficient.


So here my friends, how a simple `[[10],[20]] * 10` does behind the hood in a single functions, This is apart from the things that numerous other modules of Python Interpreter take care of that even I haven't fully explored. 

Enjoy your Coffee ☕