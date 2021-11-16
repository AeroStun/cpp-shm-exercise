# Take it to production

Can you turn this barely working prototype to a production-ready set of libraries?

## Introduction

This exercise/case/task is meant to evaluate potential team members in some recruitment process
I am probably involved in.
There is no right or wrong solution and the exercise is purposefully very open-ended.
At the same time, the domain is very relevant to what the team actually works with, i.e. interprocess communication.

Specifically, in our team we are often tasked to process data and transfer them from one process to another,
while code we write remains:

* Readable
* Maintainable
* Reusable
* Testable
* Performant

## The task

The team wants to create one or more libraries to handle the communication and necessary signaling between processes.
We have written a simple POC based on the
[Anonymous mutex example](https://www.boost.org/doc/libs/1_77_0/doc/html/interprocess/synchronization_mechanisms.html#interprocess.synchronization_mechanisms.mutexes.mutexes_anonymous_example)
found in the Boost library. It consists of the following files:
* [data_creator_writer_shm.cpp](example/data_creator_writer_shm.cpp)
  * Creates a shared memory "channel" and writes some data
  * Waits until some other process has changed the value (incremented by `1`) of the said data
* [data_reader_writer_shm.cpp](example/data_reader_writer_shm.cpp)
  * Reads an already existing shared memory "channel" and increments the value of the shared content by `1`
* [shared_data.h](example/shared_data.h)
  * The data structure to share between the two processes

It is up to you, to create a solution of high-enough quality that will handle the interprocess communication.<br>
Sounds vague? Well, it is!<br>
We would like to see how you'd tackle the problem but if you would like to discuss some details with us,
please feel free to get in touch.
