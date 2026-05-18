+++
date = '2026-05-18T17:57:38+02:00'
draft = true
title = 'Multiplatform Programming in C++'
image = 'featured.jpg'
+++

In the age of multiple available operating systems, a programmer often faces the problem of choosing the platform for their project. This choice is often not trivial. Sometimes certain functionality is available only on selected platforms, sometimes user preferences change and they migrate from one platform to another (for example Symbian was dropped because of huge interest in Android), and many applications are written for multiple platforms from the beginning. It is often best to write code that is platform-independent, and if that is not possible – to somehow manage the existence of multiple platforms in the simplest way.

### Two approaches to multiplatform programming

There are two main approaches to multiplatform programming. The first assumes that you know all target platforms and in your code you perform the right calls to your operating system API depending on the platform. The second approach assumes that instead of directly using your operating system you should rather use a multiplatform library that hides the complexity of using the operating system. In the second case the library creates a unified and consistent API for all supported operating systems.

#### Your implementation

If you want to handle the complexity of multiple platforms on your own, then you need to use preprocessor directives to perform calls to the right OS API in your code. A good example is the function that loads dynamic link libraries (the "so", "dll" or "dynlib" files – the extension is specific to your platform). To load a dynamic link library you need to write something like this:

```c++
//Define a dynamic link library handle specific to the given platform:

//Windows version
#ifdef WIN32
#define SHLIB_HANDLE HMODULE
#define INVALID_SHLIB_HANDLE NULL
#endif //WIN32

//Unix, Linux and OSX version
#ifdef __unix
#define SHLIB_HANDLE (void *)
#define INVALID_SHLIB_HANDLE NULL
#endif //__unix

inline SHLIB_HANDLE load_dynamic_library(const char * fileName)
{
#ifdef WIN32
return LoadLibrary(fileName);
#endif //WIN32
#ifdef __unix
return dlopen(fileName, 0);
#endif //__unix
}
```

The code above was simplified to highlight the most important elements. Preprocessor directives were used to define universal data types such as the dynamic link library handle and the invalid value of that handle. Then the `load_dynamic_library()` function is defined. Depending on the operating system, it performs calls to a specific OS API function. An application using only the `SHLIB_HANDLE` type, the `load_dynamic_library()` function and the `INVALID_SHLIB_HANDLE` value is going to be multiplatform. Of course in practice you may want to support a large number of platforms. Thanks to the POSIX standard, the majority of operating systems provide a similar API and the above code should work well with them. If you want to use a non-standard operating system or if you want to handle functionality that is not within the scope of POSIX, you have to implement multiple versions of your functions. Unfortunately you also need to test your code on multiple platforms.


#### Use of libraries

Another option is the use of multiplatform libraries. An example of such a library is the standard C++ library. In the modern C++ 14 version it wraps functionality such as file streams, threads and signals. The C++ standard library is actually only a definition of an interface. All platforms provide their own implementation of those interfaces. This approach allows you to use the library on all platforms in exactly the same way. In the case of other libraries, the implementation for the most typical platforms is provided by creators of the library.

Unfortunately the scope of the standard library is very limited. If you want to implement anything that is outside of C++ std, you need to do your own research on libraries that cover the functionality that you need.

### Comparison of the Two Methods

Each of the aforementioned approaches has its pros and cons. The main advantages of your own implementation are full control over the code and the ability to optimize the implementation according to your needs. However, using libraries tends to be a better solution in most cases because of ease of use and much lower effort required. The advantages of using libraries include:

- You don't need to learn about all operating systems, you only need to learn how to use the library.
- Using libraries does not require writing your own code that wraps the behavior of operating systems.
- Popular libraries tend to be very well designed. Usually they are created by large groups of developers and architects. It's hard to achieve the same level of quality on your own.
- Libraries are usually very well tested. Your own code would be new and prone to many potential issues.
- Libraries tend to be very well optimized.
- Libraries are usually very well documented.
- If the library is popular, there may be programmers on the market who already know it very well. In this case you save time needed for research and learning.
- Libraries usually support a much broader range of operating systems than you need, and therefore give you the potential for migrating your product to more platforms in the future.


### Example Libraries

Many such libraries already exist. Here is a short list of libraries that I have worked with and can recommend for usage in certain scenarios:

- **Apache Portability Runtime** – this is the only C library on this list. APR is a simple C library that concentrates on optimizing your memory allocation by doing it in big chunks. <https://apr.apache.org>
- **Boost** – at the moment of writing, Boost is the most popular and widely accepted C++ library. Its many sub-libraries allow you to access your operating system API indirectly. For example, the Asio library allows you to use asynchronous input-output operations (highly specific to your operating system) using a multiplatform API. You can learn more about Boost at <http://www.boost.org>.
- **Adaptive Communication Environment** – a solid wrapper around the operating system API. What distinguishes ACE from Boost is that Boost tries to mimic the OS API very closely, while ACE provides several components that – while having the same external functionality on all platforms – work in a totally different way internally depending on the platform. <http://www.cs.wustl.edu/~schmidt/ACE.html>.
- **POCO** – a set of libraries created mainly to simplify the creation of web-related services. <https://pocoproject.org>.

