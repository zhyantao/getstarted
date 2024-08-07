# template

## 函数模板

### 实例化

```cpp
#include <iostream>
#include <typeinfo>

using namespace std;

template <typename T>
T sum(T x, T y) {
    cout << "The input type is " << typeid(T).name() << endl;
    return x + y;
}

// 显式实例化
template double sum<double>(double, double);

int main() {
    auto val = sum(4.1, 5.2);
    cout << val << endl;

    // 隐式实例化：sum<int>(int, int)
    cout << "sum = " << sum<int>(2.2f, 3.0f) << endl;
    // 隐式实例化：sum<float>(float, float)
    cout << "sum = " << sum(2.2f, 3.0f) << endl;

    return 0;
}
```

### 特例化

```cpp
#include <iostream>
#include <typeinfo>

using namespace std;

template <typename T>
T sum(T x, T y) {
    cout << "The input type is " << typeid(T).name() << endl;
    return x + y;
}

struct Point {
    int x;
    int y;
};

// 特例化函数模板为 Point + Point
template <>
Point sum<Point>(Point pt1, Point pt2) {
    cout << "The input type is " << typeid(pt1).name() << endl;
    Point pt;
    pt.x = pt1.x + pt2.x;
    pt.y = pt1.y + pt2.y;
    return pt;
}

int main() {
    Point pt1{1, 2};
    Point pt2{2, 3};

    // 实例化函数模板
    Point pt = sum(pt1, pt2);
    cout << "pt = (" << pt.x << ", " << pt.y << ")" << endl;
    return 0;
}
```

## 类模板

### 实例化

```cpp
#include <iostream>

using namespace std;

template <typename T>
class Mat {
    size_t rows;
    size_t cols;
    T* data;

public:
    Mat(size_t rows, size_t cols) : rows(rows), cols(cols) {
        data = new T[rows * cols]{};
    }

    ~Mat() {
        delete[] data;
    }

    Mat(const Mat&) = delete;            // 不允许拷贝构造
    Mat& operator=(const Mat&) = delete; // 不允许赋值构造
    T getElement(size_t r, size_t c);
    bool setElement(size_t r, size_t c, T value);
};

template <typename T>
T Mat<T>::getElement(size_t r, size_t c) {
    if (r >= this->rows || c >= this->cols) {
        cerr << "getElement(): Indices are out of range" << endl;
        return 0;
    }
    return data[this->cols * r + c];
}

template <typename T>
bool Mat<T>::setElement(size_t r, size_t c, T value) {
    if (r >= this->rows || c >= this->cols) {
        cerr << "setElement(): Indices are out of range" << endl;
        return false;
    }

    data[this->cols * r + c] = value;
    return true;
}

int main() {
    // 使用显式实例化类模板
    Mat<int> imat(3, 4);
    imat.setElement(1, 2, 256);

    // 隐式实例化类模板
    Mat<float> fmat(2, 3);
    fmat.setElement(1, 2, 3.14159f);
    Mat<double> dmat(2, 3);
    dmat.setElement(1, 2, 2.718281828);

    cout << imat.getElement(1, 2) << endl;
    cout << fmat.getElement(1, 2) << endl;
    cout << dmat.getElement(1, 2) << endl;

    // Mat<float> fmat2(fmat); // error
    // Mat<float> fmat3(2,3);
    // fmat3 = fmat; // error

    return 0;
}
```

### 特例化

```cpp
#include <iostream>

using namespace std;

template <typename T>
class MyVector {
    size_t length;
    T* data;

public:
    MyVector(size_t length) : length(length) {
        data = new T[length]{};
    }
    ~MyVector() {
        delete[] data;
    }
    MyVector(const MyVector&) = delete;
    MyVector& operator=(const MyVector&) = delete;
    T getElement(size_t index);
    bool setElement(size_t index, T value);
};

template <typename T>
T MyVector<T>::getElement(size_t index) {
    if (index >= this->length) {
        cerr << "getElement(): Indices are out of range" << endl;
        return 0;
    }
    return data[index];
}

template <typename T>
bool MyVector<T>::setElement(size_t index, T value) {
    if (index >= this->length) {
        cerr << "setElement(): Indices are out of range" << endl;
        return false;
    }
    data[index] = value;
    return true;
}

// 特例化类模板
template class MyVector<int>;

// 特例化类模板
template <>
class MyVector<bool> {
    size_t length;
    unsigned char* data;

public:
    MyVector(size_t length) : length(length) {
        int num_bytes = (length - 1) / 8 + 1;
        data = new unsigned char[num_bytes]{};
    }

    ~MyVector() {
        delete[] data;
    }

    MyVector(const MyVector&) = delete;
    MyVector& operator=(const MyVector&) = delete;
    bool getElement(size_t index);
    bool setElement(size_t index, bool value);
};

bool MyVector<bool>::getElement(size_t index) {
    if (index >= this->length) {
        cerr << "getElement(): Indices are out of range" << endl;
        return 0;
    }
    size_t byte_id = index / 8;
    size_t bit_id = index % 8;
    unsigned char mask = (1 << bit_id);
    return bool(data[byte_id] & mask);
}

bool MyVector<bool>::setElement(size_t index, bool value) {
    if (index >= this->length) {
        cerr << "setElement(): Indices are out of range" << endl;
        return false;
    }

    size_t byte_id = index / 8;
    size_t bit_id = index % 8;
    unsigned char mask = (1 << bit_id);

    if (value)
        data[byte_id] |= mask;
    else
        data[byte_id] &= ~mask;

    return true;
}

int main() {
    MyVector<int> vec(16);
    vec.setElement(3, 256);
    cout << vec.getElement(3) << endl;

    MyVector<bool> boolvec(17);
    boolvec.setElement(15, false);
    boolvec.setElement(16, true);

    cout << boolvec.getElement(15) << endl;
    cout << boolvec.getElement(16) << endl;
    return 0;
}
```

## std::function

`std::function` 是 C++ 标准库中的一个模板类，用于包装可调用对象（函数、函数指针、成员函数指针等），并提供一种通用的方式来存储、传递和调用它们。这个类位于 `<functional>` 头文件中。使用 `std::function` 可以实现函数对象的泛型封装，使得在编写泛型代码时更加灵活。

下面是一个简单的例子，演示了 `std::function` 的基本用法：

```cpp
#include <functional>
#include <iostream>

// 定义一个普通的函数
int add(int a, int b) {
    return a + b;
}

int main() {
    // 使用 std::function 包装普通函数
    std::function<int(int, int)> addFunction = add;

    // 调用包装后的函数
    std::cout << "Result: " << addFunction(3, 4) << std::endl;

    return 0;
}
```

这个例子中，`std::function<int(int, int)>` 表示一个能够接受两个整数参数并返回整数的 `std::function` 对象。通过将普通函数 `add` 赋值给 `addFunction`，我们可以使用 `addFunction` 来调用 `add` 函数。

总的来说，`std::function` 是 C++ 中处理可调用对象的通用机制，为泛型编程提供了更大的灵活性。
