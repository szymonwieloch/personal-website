+++
date = '2026-05-18T19:03:46+02:00'
draft = true
title = 'Object Oriented Approach to Asynchronous Operations'
image = './featured.json'
+++

# Object Oriented Approach to Asynchronous Operations

In one of my previous articles, I described the idea of asynchronous operations. Apart from many advantages, asynchronous operations do have some disadvantages as well – they are complicated. It is relatively complicated to implement a big, multiplatform application. Of course, it is possible to wrap native functions from your operating system with classes that will greatly simplify the use of this complicated approach, and they can also hide the inter-platform differences. Correct encapsulation and abstraction can result in a much more user-friendly interface.

## Basic Object-Oriented Framework

Let’s start by thinking about what the core of such wrappers would look like. We definitely need to wrap `select()` or `WaitForMultipleObjects()` (depending on the platform). This class should contain a collection of IDs of the previously created I/O streams. When data arrives, it should automatically invoke a callback and then return to waiting for other events. Let’s call this class Reactor because it reacts to events reported by the operating system.

Certainly, the Reactor class doesn’t “know” what to do with the data received. The decision belongs to business logic that should be placed outside the wrapper. As I have already mentioned, we need a callback. Since there are multiple signals that can be received from a stream (data, error, notification about a closed socket, etc.), it’s best to create a generic interface — or rather, because C++ does not support the interface concept directly, an abstract class with several purely virtual methods. Implementations of this abstract class can then be registered as callbacks for handling I/O events. Let’s call this abstract class EventHandler because it handles the events.

Let’s try to write a simple piece of code that shows what we are aiming at:

```c++
class EventHandler {
    friend class Reactor;
    private:
    virtual void handleInput() = 0;
    virtual void handleOutput() = 0;

    virtual void handleClose() = 0;
    virtual void handleError(int err) = 0;
};
```

The Reactor class cooperates with the EventHandler class. It needs to contain a collection of registered pairs: a stream ID and a callback for handling events. It also needs methods for adding, removing, and manipulating callbacks and handlers — and, of course, for starting and stopping the asynchronous loop. An exemplary Reactor interface is shown below. `Handle` represents an OS-specific stream ID — `int` on POSIX platforms and `HANDLE` on Windows (such types are usually defined as typedefs in platform-specific C++ headers):

```c++
// Brief overview of the Reactor class
class Reactor {
public:
    void registerHandler(Handle handle, EventHandler* eventHandler); // adds new pair
    void removeHandler(Handle handle);                               // removes pair
    void startEventLoop();                                           // blocks the thread and waits for asynchronous events
    void stopEventLoop();                                            // ends the loop and unblocks the thread
};
```

Now we can create, for example, a class that handles data coming from a socket. It should inherit from the EventHandler class and provide implementations of its purely virtual methods. Once we have this class, we can create several instances and open several sockets. Then, pairs consisting of a socket and a handler can be registered in Reactor to handle several connections within a single thread.

Usually you create only one Reactor instance per thread. There can be several threads, each running its own internal event loop using Reactor. A common pattern is, therefore, to use thread-specific storage. Instead of passing a Reactor reference to each object that uses it, it is much simpler to create a function like `Reactor& giveMeThisThreadReactor()`.

## More Advanced Functionality

We already have everything we need to handle I/O operations correctly, but it turns out there's more that can be achieved with Reactor. Let's look at cross-thread notifications, timers, and OS signals.

## Cross-Thread Notification System

Let's imagine that you have several threads in your application. Each of those threads has its own Reactor, and each thread maintains several sockets, files, and other asynchronous I/O sources. Let's say that one of your threads needs to send a message to another thread. How would you do that? Of course, the solution is to use synchronized message queues. It's a very elegant solution commonly used in the majority of mature systems. The problem, though, is that the thread handling I/O operations gets blocked and won't check the queue until some I/O data arrives. To avoid that, we not only need a queue but also a way to wake up the receiving thread.

## Timers

Another feature commonly used by applications is timers. For example, when you perform an HTTP GET request, you always want to make sure that your application won't hang forever if the other party stops responding. To achieve that, you have to set a time limit and simply end the request after the given time. Since the receiving thread is blocked on I/O operations, it can't check the time. It's a very similar situation to the one with a synchronized queue – your thread is blocked, unresponsive, and waiting for some I/O data. Fortunately, both `select()` and `WaitForMultipleObjects()` allow you to pass a timeout value. If you wrap this functionality correctly, you can have a solid timer framework with the ability to add and cancel asynchronous timers.

## Signals

Signals are very short messages passed from your operating system to the application you are running. For example, if you press Ctrl+C, your operating system sends `SIGINT` to your application. If the application does not provide a handler for this signal, the operating system will kill it. If it does, it handles the signal in the way implemented by the programmer. Of course, signals are asynchronous in their nature, so they fit naturally into our Reactor/EventHandler framework.

## Complexity Problem

After this analysis, you have surely noticed that our framework becomes a little complex. It gets even more complex if you want to create a multiplatform solution, because then you need to hide all the platform-specific details inside the Reactor, write duplicated platform-specific code, use a lot of C++ `#ifdef` directives, and test your code on all platforms. This framework grows even further when you notice that signals, asynchronous notifications, and timers are implemented differently in each operating system, and your code will differ significantly for each platform. It would probably take months to implement it on your own. Fortunately, there are ready-to-use libraries where all that work has already been done.

## Libraries for Handling Asynchronous I/O

### Asio from Boost

There are actually two main libraries commonly used in the C++ world to handle asynchronous I/O operations. The first one is **Asio**, which comes from **Boost** ([https://boost.org](https://boost.org/)). Boost is one of the most solid and widely-used libraries for system programming in C++. Unfortunately, this library does not support signals or cross-thread notifications. You could probably implement them on your own, but what's the point of using a library then?

### Adaptive Communication Environment

A very solid alternative is the **Adaptive Communication Environment** ([http://www.cs.wustl.edu/~schmidt/ACE.html](http://www.cs.wustl.edu/~schmidt/ACE.html)). Contrary to Boost, it provides all the mentioned functionality. Its API consists of the **ACE_Reactor** and **ACE_Event_Handler** classes. As you can guess, they almost precisely match what I have described earlier as Reactor and EventHandler. Additionally, ACE allows you to use operating system handles directly; it has its own wrappers around sockets, file servers, etc. that make I/O much simpler, and it is suitable for real-time solutions.

## A Real-Life Example

While working for Wind Mobile, I had the opportunity to maintain the NaviVoice application. NaviVoice is a layer of a phone card management stack, and it is purely asynchronous due to speed requirements. Since it was supposed to be migrated from Windows to Linux (but was using direct operating system calls), ACE and the Reactor framework were a perfect solution for that. Not only did this framework prove to be useful, but it also saved me months of slow implementation and writing multiplatform code. A huge advantage of using an external library is that you don’t have to write more code. In fact, you need to delete much of your own code and therefore make your product much easier to maintain. You can also limit the number of tests, because such libraries tend to be heavily covered with tests and are already used by thousands of companies around the world. You can skip part of the documentation too – ACE is, of course, very well documented.

The final result was an application that had less code, could work on several platforms, and was much easier to maintain. Simply – an almost perfect success story.