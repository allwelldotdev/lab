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
- An example I/O bound task would be a computer program that reads from a file because the speed of the hard drive limits the speed of the program.
- Multithreading attempts to get around the limitations of I/O bound tasks by running multiple threads of a program concurrently, allowing each thread to make progress independently.

> In Python, a major characteristic is its Global Interpreter Lock (GIL) which allows only one thread to hold control of the Python interpreter at once — preventing multiple threads from executing in parallel.

> For more on Multithreading, read geekforgeeks: https://www.geeksforgeeks.org/multithreading-python-set-1/

## Parallelism and Concurrency: What's the connection?

The dictionary definitions of “concurrent” and “parallel” are quite similar — referring to things happening at the same time — but the computer science definitions are more nuanced. To make this distinction even more clear, imagine a group of ten people standing in two lines at a grocery store checkout, with five people in each checkout line. If only one cashier is servicing both checkout lines, alternating which checkout line they are assisting, this scenario would be **concurrent execution**. However, if another cashier showed up and each of the two cashiers checked out one line of customer search, this scenario would be **parallel execution**.

Multiprocessing more closely resembles parallelism, whereas multithreading more closely resembles concurrency. Multiprocessing achieves parallelism by running tasks on separate cores, while multithreading achieves concurrency by running tasks in separate threads within a single core.

![concurrency diagram](../assets/Pasted%20image%2020250312175929.png)

![parallelism diagram](../assets/Pasted%20image%2020250312175953.png)

> Watch this YouTube video by freeCodeCamp on the section on Multithreading and Multiprocessing: https://youtu.be/HGOBQPFzWKo?si=n4MkmL2qN__rqbDO

