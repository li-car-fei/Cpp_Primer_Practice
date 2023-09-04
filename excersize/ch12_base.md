# 内存管理

- new：先分配 memory，再调用 ctor

`Complex *pc = new Complex(1,2);`
编译器转化为：

```cpp
void *mem = operator new(sizeof(Complex)); // 分配内存
pc = static_cast<Complex*>(mem);           // static_cast 转型
// 执行构造函数，pc调用构造函数，this即是pc
pc->Complex::Complex(1,2);
```

- delete: 先调用 dtor，再释放 memory

```cpp
String *ps = new String("Hello");
delete ps;
```

编译器转化为：

```cpp
String::~String(ps);    // 析构函数
operator delete(ps);    // 释放内存
```

- array new 一定要 搭配 array delete

因为如果是 `new String[3]` 实际上 p 存的是三个指针指向 String，在其上方有一个 int 数据，指明了存的数组中元素数量。delete 时，需要一个一个取地址释放对应的元素内存，再释放指针的内存。如果没有 array delete，只会释放第一个元素。

```cpp
String *p = new String[3]
delete[] p;                 // 唤起三次dtor
```

如果不搭配 array delete

```cpp
String *p = new String[3]
delete p;                 // 只唤起一次dtor
```
