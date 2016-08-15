## Chapter 1: Object Lessons

C++ class在布局和存储时间上的开销主要由 **虚函数，虚基类和多重继承** 导致的。

C++ 中同一 access section 的数据的声明顺序和其内存布局顺序是一直的，但是不同的 access section 之间的就不能保证了。(说法不精确)

C++ 的多态只存在于 public class 体系中：

+ 对象指针的隐式转换
+ dynamic_cast 运算符
+ virtual 函数

class object 的空间计算：

+ 非静态数据成员的大小
+ alignment 补齐
+ 由于 virtual 特性而引入的开销（虚函数表指针和虚基类表指针）

```c++
struct SA
{
    short a;
    //long long c;
};
class BASE{};
class CA: virtual BASE{
public:
    SA _a;
    char b;
    virtual ~CA() {}
};

// sizeof(CA) == 12
```

## Chapter 2: The Semantics of Constructors

### 默认构造函数

编译器可能会生成两种默认构造函数：

+ trivial default constructor

+ non-trivial default constructor
    * 成员对象带有 default constructor
    * 基类带有 default constructor
    * 带有虚函数
    * 虚基类

### 拷贝构造函数

适用地方：

+ 定义并初始化
+ 函数参数值传递
+ 函数返回值

默认的拷贝构造语义：内置类型值拷贝，对象 **递归** 拷贝构造。


