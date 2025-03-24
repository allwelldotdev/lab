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

## More Helpful Resources
- looking at Python GIL (Global Interpreter Lock)
	- https://wiki.python.org/moin/GlobalInterpreterLock
	- https://python.land/python-concurrency/the-python-gil (this one is quite helpful)
	- David Beazley's "Understanding the Python GIL" Pycon 2010 talk: https://www.youtube.com/watch?v=Obt-vMVdM8s&ab_channel=DavidBeazley
		- David Beazley's website for more helpful resources: https://www.dabeaz.com/courses.html
	- https://realpython.com/python-gil/
	- python GIL developments in 2021
		- someone revived the discussion by offering a promising proof-of-concept CPython version with the GIL removed
			- source code on [Github](https://github.com/colesbury/nogil)
			- comprehensive [Google docs](https://docs.google.com/document/d/18CXhDb1ygxg-YXNBJNzfzZsDFosB5e6BfnXLlejd9l0/edit?tab=t.0) explaining his efforts
- understanding Python concurrency with `asyncio` library, and `async`/`await` 
	- https://medium.com/@danielwume/an-in-depth-guide-to-asyncio-and-await-in-python-059c3ecc9d96#:~:text=The%20await%20keyword%20is%20used,awaited%20coroutine%20is%20in%20progress
	- https://realpython.com/async-io-python/
	- Video: https://realpython.com/courses/understanding-global-interpreter-lock-gil/
	- https://tenthousandmeters.com/blog/python-behind-the-scenes-12-how-asyncawait-works-in-python/
	- Python docs: https://docs.python.org/3/library/asyncio.html
	- https://www.reddit.com/r/Python/comments/pdtmtw/how_asyncawait_works_in_python/
	- https://realpython.com/courses/python-3-concurrency-asyncio-module/
	- Speed Up Your Python Program with Concurrency (https://realpython.com/python-concurrency/)
- other Python learning resources
	- https://tenthousandmeters.com/tag/python-behind-the-scenes/

## Excerpts from the book: Python Concurrency with asyncio
The book is Python Concurrency with asyncio. Written by Matthew Fowler. Hosted on orielly.com

> When we refer to an operation as I/O-bound or CPU-bound we are referring to the limiting factor that prevents that operation from running faster.

New on concurrency...