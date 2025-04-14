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


