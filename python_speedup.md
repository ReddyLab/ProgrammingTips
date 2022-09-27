# Speeding up Python

First -- profile for speed! Figure out where your code is slow/taking a lot of time so you can make informed choices on what areas to target for any optimization work.

* [profile](https://docs.python.org/3/library/profile.html)
* [pyinstrument](https://github.com/joerick/pyinstrument/)

Profile memory usage too. We in CS used to talk about tradeoffs between space (memory) and time in CS. You could reduce time by using more space, and visa versa. In modern computer architectures this is often no longer true and improving (reducing) memory usage can also improve time.

* Some information about profiling memory usage: https://pythonspeed.com/datascience/#measuring-memory-usage

## Use better Data Structures
* [Data classes](https://docs.python.org/3/library/dataclasses.html)
* [Tuples](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences)
* [Named tuples](https://docs.python.org/3/library/collections.html#collections.namedtuple)
* [__slots__](https://wiki.python.org/moin/UsingSlots)

These all have the advantage of being smaller than standard classes, reducing memory usage and potentially improving speed.

## Use a Faster Algorithm
If profiling indicates that your algorithm is slow, there are a few things you can do.

* Do a web search to see if anyone has solved your problem before, and see if they did it differently
* Rethink your algorithm, making sure it's not [accidentally quadratic](https://accidentallyquadratic.tumblr.com)
* Use a simpler but "worse" algorithm

Regarding that last tip, surprisingly, a "worse" algorithm is sometimes faster for small input sizes.

## (Indirectly) Use a Faster Language
* [Numpy](https://numpy.org)
* [Scipy](https://numpy.org)

Nice python interfaces for a mix of C, C++, Fortran

## Work Concurrently/in Parallel
* [async/await](https://docs.python.org/3/library/asyncio.html) and [asyncio coroutines and tasks](https://docs.python.org/3/library/asyncio-task.html)
* [threading](https://docs.python.org/3/library/threading.html)

    Useful if app is I/O-bound -- data input and output is a major cause of poor performance

* [multiprocessing](https://docs.python.org/3/library/multiprocessing.html)

    Spin up multiple python processes, avoiding the GIL

* [concurrent.futures](https://docs.python.org/3/library/concurrent.futures.html)

    Offers nice wrapper around multiprocessing and threading

These different ways of performing multiple tasks at once each have their place. Split your data up properly to get the most out of multithreading and multiprocessing.

* CRADLE anecdote about splitting multiprocessing data into fewer big chunks

## Use PyPy
* [PyPy](https://www.pypy.org) JITs python, plus lots of other optimizations.

PyPy won't always be a win, it depends a lot on the situation. See https://www.pypy.org/features.html for more information about appropriate use cases. It also only supports python up through 3.7.

## Compile Python
* [Cython](https://cython.org) Compiles python, or a python-like language to C, then to machine code
    
* [Numba](https://numba.pydata.org) JIT python to machine code
* [Taichi](https://www.taichi-lang.org) JIT python to machine code

A lot of these improvements are based on speeding up loops of code, numeric operations, and operations
on arrays of numbers. They can perform transformations on code to make use of GPUs and CPU-specific features like SIMD.

Incidentally while neither HARDAC nor DASH/SOM-HPC support GPUs, DCC does! https://dcc.duke.edu/dcc/slurm/#gpu-jobs -- All the GPUs are Nvidia, so they support CUDA and OpenCL programming interfaces. It might be interesting to try out numba/taichi on DCC on a GPU node.

## Use a Faster Language
Using a new language means there are even more ways to speed up code. 

* [Julia](https://julialang.org)
* [Go](https://golang.org)
* [Rust](https://rust-lang.org/) (integrate with python using [PyO3](https://pyo3.rs/) and [maturin](https://maturin.rs))
  * I did this for data filtering in the portal
* C/C++ (https://docs.python.org/3/extending/index.html#extending-index, https://docs.python.org/3/c-api/index.html)

## More Resources
* https://pythonspeed.com
