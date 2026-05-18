+++
date = '2026-05-18T19:25:34+02:00'
draft = false
title = 'What Programming Language Should You Choose?'
image = './featured.jpg'
+++

Modern IT world has created hundreds of programming languages. Each of them has its own pros and cons. If you have a job to do – what language should you choose?

The short answer to this question is: it depends, because depending on the kind of functionality that you need, a different language may be optimal. In this article I’ll try to describe the most popular kinds of languages, their advantages and disadvantages as well as the kinds of applications that you should use them for.

## High or low level?

A useful distinction for us will be whether the language is high-level or low-level. Low-level languages directly describe operations that need to be performed by the underlying hardware. The higher-level the language is, the more abstract it gets (for example, it can be run on a variety of processors) and the more functionality is automated (for example, memory management). Unfortunately, high abstraction and automation come with a price. The biggest problem is the overhead of the layer that translates high-level commands into hardware operations. In the case of scripting languages, the overhead makes the application run around 100 times slower than a similar application written in a low-level language. Somebody could now think that high-level languages do not make sense if they slow down your system to this level. But it’s a mistake. In the modern world, the cost of a processor is relatively low compared to the cost of writing software. High-level languages can greatly decrease the time needed to finish your projects and limit potential problems. This is why the modern world generally tends to move in the direction of high-level languages.

If you want to write an application, you can usually choose to either use a low-level language and slowly build fast-running software, or use a high-level language and quickly build a slow-running solution.

Different tasks may require different choices. For example, websites are usually built using high-level scripting languages. But embedded devices (that are mass-produced in as many as millions of pieces) are commonly built using low-level C, and this makes it possible to use cheaper processors and lower the unit cost.

## Assembler

Assembler is a class of languages that represents the lowest possible level. A characteristic feature of those languages is that each existing processor has its own assembler language, and an application written for one processor cannot be run on another processor.

Currently, assembler is relatively rarely used. It is sometimes a language of choice for programming microcontrollers and – of course – compilers. Its organization makes it difficult to write any large software because the code quickly becomes hard to maintain.

## C

C is currently the most popular procedural language. This means that its syntax is based on procedures and data structures. A disadvantage of C is its lack of full portability between platforms. Although code written for one processor can be compiled for another, in practice there are differences in details between microprocessors and C does not always hide the complexity. For example, an integer variable (int) has different sizes depending on the microprocessor used. A similar problem is caused by the so-called endianness – the order of bytes in larger structures. Different processors organize their memory in different ways, and this sometimes causes problems when code gets migrated from one platform to another. To write a multiplatform application in C, you need to hire really good programmers who are aware of the challenges of running C code on multiple processors and know how to handle them.

Currently C language is mostly used to write embedded software and operating systems.

## C++

C++ is a younger cousin of C. Actually, it is an object-oriented extension to C. C++ is replacing C because of its ability to create object-oriented design that makes building complex applications much easier. At the same time, object-oriented design does not introduce significant overhead. If your platform supports a C++ compiler, you should probably use it instead of C.

C and C++ (and a recent, not yet fully developed, Rust language) are the only languages that can be used to build real-time solutions, for example audio streaming in Skype. For a long time, C++ was also used in desktop applications and, although it has been superseded by C# and Java, it is still quite popular because of market inertia. Another area of popularity is game development, where C++ has definitely been the most popular choice for many years.

## Java/C#

Java and C# belong to the first generation of languages that are 100% independent of the processors they run on. Thanks to this, each application written in Java should (at least in theory) work on every operating system and on every microprocessor. Practice sometimes shows that there are small differences between platforms that do cause some problems, but contrary to C and C++, those problems are rather uncommon.

Java and C# introduced one more very important mechanism that solved an age-old programming problem – memory leaks. A C or C++ programmer needs to remember to free previously allocated memory when it is no longer needed. C# and Java detect when a given object is no longer referenced and free it for you. They eliminate hundreds of difficult problems and reduce programmers’ work. A disadvantage of garbage collection is its unpredictability, which makes Java and C# unsuitable for writing real-time solutions.

Java and C# are frequently used to create office software (for example, Word) and business web services. There was research conducted on productivity in C++ and Java, and according to it, you need only half as much time to write the same application in Java compared to C++. Unfortunately, it then runs between 1.4 and 5 times slower.

## Scripting languages

The last category of programming languages is scripting languages. Frequently, they are also called dynamically typed (formally this is not the same, but most often scripting languages are also dynamically typed). What is characteristic of those languages is that objects do not have predefined types, and only when you perform a given operation on them is it checked whether this operation is possible. To state this in a more metaphorical way, scripting languages allow you to add five carrots to a car and, during this operation, raise an exception. Statically typed languages such as C++ or Java do not let you add incompatible types at all, and they report an error during compilation.

There are many scripting languages, with JavaScript, Python, Perl, and PHP being among the most popular ones. Because of the lack of static code checking, dynamic languages require a huge number of automatic tests, especially unit tests. They are not the best choice for writing one big application, but a great choice for writing complex microservice architectures that consist of multiple small and independent services.

Scripting languages are commonly used in a web context for generating and maintaining websites, applications running in the cloud, and for automation and writing short programs on desktop computers. Scripting languages are also frequently used to perform integration tests of applications written in other languages.