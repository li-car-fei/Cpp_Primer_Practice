# 内存管理

- new：先分配 memory，再调用 ctor

`Complex *pc = new Complex(1,2);`
编译器转化为：

```cpp
void *mem = operator new(sizeof(Complex));
// 分配内存，内部调用的是 malloc
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
operator delete(ps);    // 释放内存, 内部调用的是 delete
```

// delete 和 new 的操作顺序相反

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

# 拷贝构造与拷贝赋值

- 拷贝构造（构造函数）：new 一块内存，指针指向新建的要拷贝的内容
- 拷贝赋值（成员函数）：先把当前动态指针指向的 delete 掉，再 new 一块内存，指针指向要拷贝的内容

# 一些常见的关键字

virtual: 基类中声明虚函数，表现多态
override: 子类中声明这是对虚函数的多态（不是重写、重载，重写是基类和子类参数列表不一样所有子类覆盖基类该函数，重载是同一个类中同名函数但参数列表不一样执行不同的操作）
constexpr: 声明在编译时求值，使得某个量变为常量，某个函数可以编译时求出常量
explicit: 禁止隐式类型转换
extern: 声明一个变量是外部文件引入的
static_cast<> : 对变量进行转型
