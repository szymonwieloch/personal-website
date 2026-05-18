+++
date = '2026-05-18T18:36:12+02:00'
draft = false
title = 'Rust – Modern Alternative to C++'
image = './featured.jpg'
+++

For many years the C++ language was the only reasonable option for writing real-time or embedded software. But during recent years a new technology has emerged — a technology that has the potential to take the crown of low-level programming in several years. It's called Rust.

### Why not Java, Python, Go or D?

There are many alternative languages to C++. Somebody could wonder – what’s the point of creating a new one? Why don’t you simply use Java, C#, Go, Python or any of the currently popular languages? It turns out that for many use cases those languages are solid substitutes. Actually they can be much better than C++ (have you ever seen any website written in C++?). But there are cases where C++ did not have any real alternatives. Let’s look at them:

- Real-time software – Java, Go, C#, Python and almost all modern languages rely on a garbage collector. This memory management method is perfect for writing an average application but it's definitely not suitable for real-time systems because the garbage collector tends to stop the running application at random moments for random periods of time. Therefore you can't force any time-related guarantees using those languages. If the ABS system of your car was written in Java it could stop at some moment and refuse to work for several seconds. If this happened during a difficult situation, this could end in a serious crash.
- Operating systems – the garbage collector and other mechanisms used in modern languages require a pre-existing operating system that provides several services. It is therefore not possible to use Java or Python to write your operating system. The C language is commonly used for this purpose.
- High load systems – computer games are a good example of applications where the speed is hugely important. Games compete on having best graphics. Other languages do not get compiled to binary processor commands and therefore are much slower. For example Java is 1.4 to 5 times slower than C++ and Python is around 100 times slower. There are only two other languages that compile to binary representation without any overhead: D and Go. Unfortunately the Go language has a garbage collector and it has still some overhead compared to C++. D language will be discussed later.
- Embedded software – by embedded software I mean software created for devices that are produced in millions of copies. C++ requires much slower CPUs than other languages and therefore can highly reduce the price of the device. Just one dollar difference multiplied by 10 million devices gives you 10 million dollars in savings – money that is worth the extra effort needed to create software in C++.

### Why don’t we stick to C++?

You could wonder – why to change something that has been successfully used for decades? Because C++ is a really old language and it has many flaws. Many, many, many flaws. Let’s just look at a few of them:

- It’s not multiplatform by design – although C++ can be used for multiplatform programming, it requires extra effort and sometimes generates unpredictable behavior. For example the integer “int” type has different size depending on CPU and even operating system.
- It uses preprocessor – even Bjarne Stroustrup, the creator of C++, thinks that the preprocessor is a totally out-of-date solution and should not be used in modern languages. But C++ cannot drop backwards compatibility and still allows the use of something so ridiculously error prone.
- It is not safe – this is probably the biggest disadvantage of C++. It is so easy to shoot yourself in the foot using C++ that you may wonder how all the big applications are actually built. In C++ you can easily write out of bounds of an array and corrupt your memory. Then you get what is thought to be one of the most difficult bugs to find and fix.
- Memory leaks – C++ doesn't have any built-in mechanism to prevent memory leaks and it's a common problem that many complex applications consume more and more and more resources by never freeing part of allocated memory. While there are memory debuggers that help you with finding such problems, they often generate thousands of false positives and require a lot — and I mean — a lot of effort to find such bugs.
- Many small design flaws – C++ was one of the first object-oriented languages and many of its features were experimental. Not all of them are thought to be right choices. For example a default copy constructor very often causes unpredictable bugs. Lack of standard for naming C++ symbols makes it impossible to link pieces of code compiled by two different compilers. Automatic conversion between numeric types often removes part of the information. Lack of built-in synchronization methods makes multithreading programming a real challenge.
- There are no standard building methods and no packaging system – while all modern languages provide what you could call a meta information format for projects, C++ can be built with many tools including Make, CMake, Microsoft internal solution format and many more. Without any standard for building a project it is impossible to introduce packages – small “bundles” of code that can be downloaded from a central repository. Such packages were successfully introduced for example in Java and Python.

### What then?

After the success of C++ there were several attempts to introduce another language that could possibly replace C++. One of them was the “D” language. But it did not stick. It is thought that D language – although better than its predecessor – was simply not good enough. The value that it provided was not enough to switch from C++ to it. As a result, D quickly stopped being used and has only minimal support in the Internet.

Rust is just another approach. But contrary to previous languages it’s the one that truly has potential to become something greater than C++. Let’s look at several of its main features that make it a solid proposal as a C++ replacement.

- Rust is multiplatform. Since the beginning Rust was thought to be a platform independent language with types of constant size and the operating system API hidden after a multiplatform Rust standard library. This allows you to write your code once and run it everywhere.
- Rust is safe. By "safe" I mean that it is not possible to corrupt process memory, write code with random bugs or compile something that obviously would not work. The Rust compiler is very rigid. It strictly analyzes your code and reports errors whenever there is any potential for a mistake (sometimes it even forbids you to run perfectly valid code if it can't understand its validity). It's not possible to write outside of arrays, it's not possible to modify read-only data, and it's not possible to create any cross-thread hazards because Rust won't allow you to modify the same variable from two threads without a mutex. From the beginning to the end Rust was designed to be 100% safe.
- Rust has a unique memory management approach. What makes Rust different from both Go and C++ is that it does not allow memory leaks but at the same time it does not have any garbage collector. This memory management system is a little similar to C++ smart pointers (programmers are not allowed to allocate memory with smart pointer assigned to it).
- Rust is fast. The comparison of languages showed no reasonable differences between C++ and Rust.
- Rust has a standard build system and packages. https://crates.io is a central repository of all free Rust packages (also known as crates).
- Rust links with C. From Rust language it is possible to run C functions directly (although they need to be separately declared). It is also possible to create a Rust library with a C compatible API. This makes it easy to combine those two languages and reuse a lot of existing code.

### Are there any disadvantages?

As with all existing technologies, Rust is not perfect. But contrary to other languages most of the problems are not technical but rather caused by its relatively fresh release.

- Rust is new. And as a new language it is not yet finished. Many people in the world are working on making Rust better but at the moment the language lacks several important features (such as templates with constant numbers) and there are not as many libraries as in C++.
- Rust is difficult. While still being probably less complicated than C++, it requires quite a long time to achieve high level of competency. What makes Rust especially difficult are its two features: lifetimes and the borrow checker that work together to prevent memory corruption and memory leaks. In more complicated scenarios it is sometimes difficult to write the right combination of lifetimes to prove to your compiler that the code is safe.
- There are almost no Rust programmers. This is a huge business blocker. Companies rarely start new projects in Rust because there are no workers in the market and they need to hire C++ programmers and spend a lot of resources on teaching them Rust.

### Summary

While Rust may not be perfect, it’s the only language that differentiates itself from C++ enough to create a potential for becoming a replacement for C++. There is still a lot to do but interest in Rust seems to grow. The first place on Stack Overflow when it comes to the most asked questions is a sure sign that people started learning Rust seriously. Hopefully the initial market resistance will be short-lived and Rust will conquer the world of low-level programming.

You can learn more about the language at <https://www.rust-lang.org>
