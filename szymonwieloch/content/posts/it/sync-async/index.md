+++
date = '2026-05-18T19:11:34+02:00'
title = 'Synchronous vs Asynchronous IO Operations'
image = './featured.json'
+++


Due to a growing complexity of software systems, there is a natural tendency towards simpler methods that achieve similar (although not identical) results. Examples include Java or C# language. This process highly relies on removing the need for understanding complex operations that take place inside your operating system kernel. A programmer is only supposed to combine prebuilt components into his application. These "idiot-friendly" solutions often lead to dramatic decrease in your application performance.

## What can threads do to your application?

A good example is an application written by two colleagues of mine from one of my past employers. In order to handle database connections they created a new thread with a new socket per each connection. This approach is very simple and very common for example in Java (and this was the language used for the database). Unfortunately it turned out that the overhead of creating a new thread and maintaining multiple connections was so huge that the application was not working effectively at all. To prevent creating a new thread whenever they wanted to put something into the database (usually hundreds times per second) they eventually created a pool of threads. But with a growing number of parallel requests, the database slowed down and each thread had to wait a long time before it could finish its work.  It turned out that this application required **3000** threads in the pool to be able to queue all requests. This is hardly a typical solution. The application could not run on Windows platform because this operating system could not handle such an uncommon number of threads. Finally, the application was run on Solaris platform because only this operating system was capable of running such a poorly written code.

I suspect that in this situation most programmers  would blame Windows for such a low capacity for maintaining threads but, frankly speaking, this is not Bill G.’s fault. It is not normal to create such a huge amount of threads in one application. The reason for it is the cost of the so-called **context switching**. Each operating system assigns each thread a time slice when the given thread is running using 100% of one CPU core, then the processing power is passed to another thread. Although it seems that two applications are running in parallel on your computer, in fact only one of them is running at a time but your operating system is switching them so often, that you do not notice (this gets a little more complex in case of modern multicore processors but it’s outside of scope of this article). In order to switch between threads, your operating system needs to replace values of multiple processor registers and part of its memory. Context switching is normally not very costly but it needs to take place so frequently that a user experiences the illusion of parallel processing. If you create too many threads, this forces the operating system to switch context frequently and therefore to waste a lot of its processing power.

I used to work with phone cards from Dialogic company. Their documentation recommends to use their API in asynchronous mode because creation of for example 120 threads needed for the synchronous mode is way too much. Further recommendations state that the synchronous mode was introduced only as a framework for easy tests (for example as a demo for clients). In this situation you may wonder – what the consequences are of using **3000** threads? Nothing good can come out of that.

## Two approaches to IO operations

If threads and synchronous mode are not good for handling multiple parallel IO operations, then what is? It turns out that much more effective is using asynchronous operations in one single thread. Let’s just look at some examples.

## Synchronous operations

Synchronous operations are very easy to understand. They are used by 95% of all “new-age” programmers (this often explains why modern applications run so slowly and why you need to buy new hardware once in a while). Synchronous IO operations are called blocking because the thread gets stopped until new data arrives. I have decided to use C/C++ language to give you some examples.

```c++
//This example uses standard C++ library.   
#include <fstream>   
#include <string>   
using namespace std;   
int main()   {   
    ifstream file("filename.txt"); // Here a file stream is created.   
    string line;   getline(file, line); // Here a synchronous function gets called and it blocks the thread.   
     
    /*You can reach this point only when the previous operation finishes.   You need to keep in mind that IO operations are relatively slow compared to the speed of microprocessors.   This means that your CPU processing power will get wasted until new data arrives.  */   
    
    file.close(); // The file gets closed and operating system resources get freed.   
}
```

In the example of an application with 3000 threads my colleagues of course used a blocking synchronous function to perform operation on the database. The function was finishing only when data was exchanged between the application and the database and the database reported end of its work. Because it was quite a time consuming operation, they had to have 3000 threads and the overhead of such a huge number of threads was killing performance of their application.

## Asynchronous operations

Asynchronous operations work in a different way. The thread gets blocked too but contrary to synchronous operations you can wait for multiple sources of data just in one thread. Control comes back to your thread as soon as one of the sources reports arrival of data. Instead of 3000 threads you can create only one thread and wait inside of it for 3000 sources of data. Once the data arrives, you can process received data and continue waiting for signals. This approach removes most overhead of the previous operations, can run on greater number of platforms and highly improves performance. Asynchronous operations are operating-system-specific and, depending on a platform, they need to be handled in a different way. But in reality most of programming languages provide their platform independent wrappers around OS native API. For example in Java you can use the java.nio.channels package. But since such libraries only wrap the internal OS functionality, let’s concentrate on what is actually going on below the surface.

On Windows platform the function to wait for multiple sources of IO data is WaitForMultipleObjects(). On platforms based on the POSIX standard its equivalent is the select() function (modern platforms also support epoll()). All those functions work in a similar way. First you need to open streams that represent the given socket or file (depending on platform other IO types may be available, for example unix sockets are popular on unix-like operating systems including Linux). When opening the given stream you receive a unique (within the process) ID of the stream. On Windows it’s a HANDLE structure (that is internally implemented as a pointer). POSIX-compatible platforms use common integer number (int). Then all those IDs are collected into a set and are passed to the previously mentioned function. Call to this function always blocks the thread. When the call finishes it returns identification of the source that does have data to be handled. Then it’s up to the programmer to handle this call and perform operations that perform business logic of his application.

The following example shows a use of two COM ports on Linux:

```c++
#include <sys/time.h>   
#include <sys/types.h>   
#include <unistd.h>   
int main()   {   
    int fd1, fd2; /* Two new IDs are allocated. */   
    fd_set readfs; /* A collection of IDs is allocated. */   
    int maxfd; /* Maximum number of used IDs. */   
    int loop=1; /* loop runs in a cycle till its value equals 0. */   
    
    /* open_input_source opens device, configures port and returns ID of the stream. */   
    fd1 = open_input_source("/dev/ttyS1"); /* COM1 */   if (fd1<0) exit(0);   
    fd2 = open_input_source("/dev/ttyS2"); /* COM2 */   if (fd2<0) exit(0);   
    maxfd = MAX (fd1, fd2)+1; /*Maximum value of ID that needs to be checked by select() function. */   
    
    /* The main loop. */   
    while (loop) {   
        FD_SET(fd1, &readfs); /* Add fd1 to the collection. */   
        FD_SET(fd2, &readfs); /* Add fd2 to the collection. */   
        
        /* Block the thread till new data is available. */   
        select(maxfd, &readfs, NULL, NULL, NULL);   
        if (FD_ISSET(fd1, &readfs)) /* If there is data available on port COM1, handle it somehow. */   
            handle_input_from_source1();   
        if (FD_ISSET(fd2, &readfs)) /* if there is data available on port COM2, handle it somehow */   
            handle_input_from_source2();   
    }   
}
```    

## Limitations of asynchronous operations

It is obvious that creators of operating systems like to make our lives miserable by adding unnecessary complexity. Some operating systems have certain limitations on asynchronous operations. The most important factor is the highest possible number of data sources. In case of POSIX-compatible systems this value highly depends on the concrete OS but usually it is huge, something between hundreds and thousands. Real issues programmers have with Windows, which limits the number of asynchronous data sources to only 64. And 64 includes also timers, waiting for other threads to finish and synchronization primitives. It’s because of those limitations that writing multithreaded applications is more common on Windows and writing asynchronous applications is more common among Linux programmers.

Additionally, writing asynchronous applications tends to be more complex and requires greater programming skills. It’s even more difficult when you want to create a multiplatform application because APIs of operating systems differ. To handle the last problem it’s best to use one of existing C++ multiplatform libraries such as ACE (Adaptive Communication Environment) or Boost.Asio. But it remains a topic for yet another article.