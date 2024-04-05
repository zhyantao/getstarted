# extern

C++ 中的 `extern` 关键字用于说明一个变量或者函数是在其他地方定义的，而不是在当前文件中定义。它的作用是告诉编译器，某个变量或函数的定义在其他源文件中，并且在链接时会找到它的实际定义。

对于变量而言，`extern` 声明告诉编译器，该变量在其他地方定义，不要为它分配存储空间。在链接阶段，编译器会在其他文件中找到该变量的定义，确保所有文件中对该变量的引用都指向同一地址。

对于函数而言，`extern` 用于声明函数的原型，表示该函数的定义在其他地方。这样在编译时，编译器就知道函数的接口，而在链接时会找到实际的函数定义。

```cpp
// 在文件 A.cpp 中定义变量
int globalVar = 42;
```

```cpp
// 在文件 B.cpp 中使用 extern 声明该变量
extern int globalVar;

// 在文件C.cpp中使用该变量
#include <iostream>
int main()
{
    std::cout << globalVar << std::endl;
    return 0;
}
```

在这个例子中，文件 `B.cpp` 使用 `extern` 声明了在文件 `A.cpp` 中定义的 `globalVar` 变量，确保在链接时能够正确找到它。