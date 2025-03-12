> Check out this blog: https://medium.com/capital-one-tech/python-guide-using-multiprocessing-versus-multithreading-55c4ea1788cd

## What is Multiprocessing?
- Multiprocessing is a technique that allows a program to run multiple tasks simultaneously.
- Each task is executed in its own process, which is a separate instance of the program.
- Multiprocessing takes advantage of multicore operating system architectures, in which multiple cores communicate directly through shared hardware caches.
- most useful for tasks that are CPU-bound.

> For more on Multiprocessing, read the Python docs on it: https://docs.python.org/3/library/multiprocessing.html

## What is Multithreading?
- Multithreading is a technique that allows a program to run multiple tasks concurrently within a single process.
- Each task is executed in its own thread, which is a lightweight unit of execution that shares the same memory space as other threads in the process.
- Multithreading is useful for tasks that are I/O bound.
- 

> In Python, a major characteristic is its Global Interpreter Lock (GIL) which allows only one thread to hold control of the Python interpreter at once — preventing multiple threads from executing in parallel.

