# Chapter 1: 新特性概述

# Chapter 2: 保证稳定性和兼容性

### 保证与 C99 兼容

C99中的预定义宏

`__func__`(`__FUNCDNAME__` for VS2013)预定义标识符

比 `#pragma` 更强大的 `_Pragma`

变长参数宏定义以及 `__VA_ARGS__`

宽窄字符串连接

long long：数值用 `LL` 或 `ULL` 后缀

### 扩展整型

可以定义一个48位或者超过64位的整型，但这是编译器实现的，没有较好的移植性。

### 宏`__cplusplus`

数值改了 201103L

### 静态断言与 `static_assert`

常规方法：

预编译期断言：

```c++
#ifdef CONDITION
#error "message"
#endif
```

运行时断言：`assert`宏

新方法（静态断言）：

+ 编译期断言：`static_assert`
+ 模拟方法：1/0（除以0会在编译期查出）

### noexcept 修饰符预 noexcept操作符

替代函数声明的 throw()，表示该函数不会抛出异常，如果抛出了则直接被 `std::terminate` 终止掉。

`noexcept(x)` 操作符返回`false` 当 `x` 有可能返回异常，反之返回 `true`。

### 快速初始化成员变量

C++11 允许使用等号=或者花括号{}对非静态成员变量就地初始化。

```c++
class CTest{
public:
    int i = 0;
    double f{ 1.234 };  
};
```

