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

而且花括号{}比圆括号()的初始化适用范围更好。

初始化列表的优先级总是大于就地初始化。
```c++
class CTest{
public:
    int i = 0;    // the same as : int i{0};
    CTest(){ cout << i << endl; }
    CTest(int i) :i(i){ cout << i << endl; }
};
int main(){
    CTest t;      // output: 0
    CTest{ 1 };   // output: 1
    CTest(2);     // output: 2
    return 0;
}
```

### 非静态成员的sizeof

VS2013 貌似支持的不好

### 扩展的 firend 语法

+ 原始：`friend class Cls`
+ 拓展：`friend Cls` 或 `friend TypeDefCls`

该拓展方式可以方便的将模板声明成友元。


### final/override 控制

这两个关键字都与 OO 的多态行为控制有关，主要作用于虚函数。

+ final: 该函数不能被继承类覆盖(override)了
+ override: 该函数是父类的覆盖(override)函数

这样的作用是约束程序员，辅助编译检查。

附：C++ 中 Overload,Override 和 Overwrite(Hide) 几个概念的区别

+ Overload(重载)：函数名相同，参数或返回值不同，与 virtual 函数无关系。

+ Override(覆盖)：多态特性，继承类函数与基类函数的所有属性都一样，必须是 virtual 函数。

+ Overwrite(重写)：是指派生类的函数屏蔽了与其同名的基类函数，规则如下：

    + 如果派生类的函数与基类的函数同名，但是参数不同。此时，不论有无virtual
关键字，基类的函数将被隐藏（注意别与重载混淆）。

    + 如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual
关键字。此时，基类的函数被隐藏（注意别与覆盖混淆）。

### 模板函数的默认模板参数

略

### 外部模板

略

### 局部和匿名类型做模板参数

C++11 放宽了这些限制， 则在C++98 是禁止的。

```c++
struct {int i;} a;            // 匿名类型变量
typedef struct {int i;} B;    // 匿名类型

void func(){
    struct C{};               // 局部类型
}
```

# Chapter 3: 通用设计解决方案

### 继承构造函数

以下是最简单的用法，但是也还存在着更加复杂的情况，先略过。

```c++
class CBase{
public:
    CBase(){}
    CBase(int a){}
    CBase(int a, int b){}
};

// 原来的方式
class CDerived1: public CBase{
public:
    CDerived1() :CBase(){}
    CDerived1(int a) :CBase(a){}
    CDerived1(int a, int b):CBase(a,b){}
};

// C++11 新方式
class CDerived2 : public CBase{
public:
    using CBase::CBase;
};
```

### 委派构造函数

委托构造函数是不能有初始化成员列表的。构造函数之间可以相互委托，但是不能互相死锁。

### 右值引用 :-)

移动构造函数：参数是 **右值引用** ，语义上类似于 `auto_ptr`的 copy 操作。

移动构造函数可以优化值传递时 **深拷贝** 的效率。

常规的左右值概念：

+ 左值：可以取地址，有名字
+ 右值：不能取地址，没有名字

C++11 中将 **右值** 的概念扩展了：收纳了 **将亡值** 的概念(比如函数返回值)，而原来的右值称为 **纯右值**。

C++98 的引用称之为 **左值引用**；
C++11 提出一种新的引用 **右值引用**。

引用类型：
```c++
int i = 0;
int & lr = i;            // 左值引用
int const & clr1 = i;    // 左值常引用
int const & clr2 = 10;   // 左值常引用
int && rr = 1;           // 右值引用
int const && crr = 1;    // 右值常引用，好像没什么用
rr = 100;                // VS2013 里面 右值引用可以修改右值？
                         // 不知道这个说法是否正确，但是确实可以执行

```

函数值传递、引用传递的应用：

```c++
class CTest{
public:

    int i = 0;
    CTest(){ cout << "Default!" << endl; }
    CTest(CTest const & r){ (*this).i=r.i; cout << "Copied!" << endl; }
};

CTest ReturnRValue(){ return CTest(); }

void AcceptValue(CTest a){ a.i = 10; }
void AcceptLRef(CTest const& a){}
void AcceptRRef(CTest && a){}

int main(){

    cout << "Accept Value" << endl;
    AcceptValue(ReturnRValue());    // "Default!"
    cout << "Accept LRef" << endl;
    AcceptLRef(ReturnRValue());     // "Default!"
    cout << "Accept RRef" << endl;
    AcceptRRef(ReturnRValue());     // "Default!"
    return 0;
}
```
上面的代码不会调用拷贝构造函数。值传递的写法 `AcceptValue(ReturnRValue())` 这样的写法也能获得使用右值引用写法 `AcceptRRef(ReturnRValue())` 相同的效果。

右值引用的一个作用是绑定临时对象/变量，延长其生命周期至该引用的结束。

__RVO(Return Value Optimization)/NRVO(Named RVO)__ : 返回值优化

RVO/NRVO 可以部分的解决 **移动语义** 的问题。

完美转发：这是什么鬼

### 显式转换操作符

explict 可作用于 class 的自定义转换类型，强制要求显式转换，禁止隐式转换。

### 列表初始化 {}

基本形式：

```c++
int a { 0 };
int b = { 1 };
int *c = { nullptr };
int *d = new int{ 10 };
int *e = new int[2]{ 10, 100 };
int f[] {1, 2, 3, 4, 5};
int g[] = { 6, 7, 8, 9, 0 };
```

列表初始化支持所有的STL容器（应该吧）。

自定义类型的列表初始化需要用到头文件 `<initializer_list>` 的 模板类 `initialize_list<T>` 来构造 **initialize_list 构造函数**。该模板类也可以用来构造函数参数的列表初始化行为。

列表初始化可以防止类型收窄 (narrowing) 问题，出现类型则会编译报错。

### POD (Plain Old Data) 类型

C++11 将满足两个基本特性的数据结构称为 POD：

+  **平凡性 (trivial)**：
    + 拥有 trivial 函数；
    + 没有多态属性（虚函数和虚基类）；
+  **标准布局 (standard layout)**：
    + 非静态成员拥有相同的访问权限；
    + 在继承关系里，继承类和基类不能同时拥有非静态成员
    + 不能多继承
    + 继承类的第一个非静态成员类型与基类不同（这是因为C++允许优化不包含成员的基类产生的）
    
POD 的好处：

+ 字节赋值（安全的 `memset` 和 `memcpy`）；
+ 对 C 内存布局兼容；
+ 保证了静态初始化的安全有效。静态初始化能提高程序性能。

### 非受限的 union

啥玩意儿

### 用户自定义字面值

啥玩意儿

### 内联名字空间

C++98 不允许不同的名字空间（连续范围）中对模板进行特化。

### 模板的别名

`using` 也有 `typedef` 的功能，而且 `using` 可以匹配模板，这是 `typedef` 不具有的功能。

### 一般化的 SFINEA 规则

SFINEA: Subsitution failure is not an error

意思就是最优匹配原则。


# Chapter 4: 哇哈哈

### 右尖括号 `>` 的改进

### `auto` 类型推导

P.S：静态类型和动态类型的主要区别在于对变量进行类型检查的时间点上。

C++11 加入 `auto` 并不意味着 C++11 带有动态类型的特性。

`auto` 最大的优势就是简化对象声明。

`auto` 不会解决数据溢出的问题，它只负责类型推导。

`auto` 改善性能的一个例子（[来源](http://gcc.gnu.org/onlinedocs/gcc/Typeof.html)）：

```c++
// 原始方法
 #define max1(a,b) \
   ({ a > b ? a : b; })

// 新方法
  #define max2(a,b) \
   ({ auto _a = (a); \
      auto _b = (b); \
 _a > _b ? _a : _b; })

// 两个宏的计算量是不一样的
max1(1*2*3, 4*5*6)
max2(1*2*3, 4*5*6)
```

`auto` 使用细则：

+ `auto` 可以推导指针类型，但是引用类型必须显式标明 `auto &`；
+ `auto` 可以与 `volatile` 或 `const` 连用，但是类型推导却不会推导这些属性；
+ 在同一个赋值语句中，`auto` 声明的变量类型必须相同；
+ `auto` 可用于初始化列表。

`auto` 的限制（语义或者实现难度）：

+ 函数形参不能用 `auto`；
+ `class` 的非静态成员不能用 `auto`。
+ `auto` 数组是搞笑的；
+ 模板类型不能写成 `auto`。

### typeid 与 decltype

C++98 支持的动态类型部分 (RTTI)：

+ `typeid` `type_info`

`decltype` 是不同于 `auto` 的类型推导，`decltype` 以一个表达式作为参数，返回该表达式的类型。

`decltype` 的推导规则，原型 `decltype(e)`，`e` 的类型为 `T`：

1. 如果，`e` 没有带括号，常规使用。注意 `e` 不能是重载函数；（写法有问题）
2. 否则，`e` 是将亡值（右值的一种），则结果为 `T &&`；
3. 否则，`e` 是左值，则结果为 `T &`；
4. 否则，`e` 是其余右值，则结果为 `T`。

注意以下区别

```c++
decltype(++i)

decltype(i++)
```

不同于 `auto` 推导不携带 CV限制符信息，`decltype` 是携带的。

### 追踪返回类型

`auto` 和 `decltype` 搭配起来用的强大功能。

举个例子：

```c++
template<typename T1, typename T2>
auto Sum(T1 const& r, T2 const& l) -> decltype(t1+t2) {
    return r+l;
}
```

追踪返回类型还可以使函数指针的表达变得简单。以下两个函数的表达了相同的意思。
```c++
int (*(*pf())())(){
    return nullptr;
}

auto pf1() -> auto (*)() -> int (*)(){
    return nullptr;
}
```

### 基于范围的 for 循环

举个例子
```c++
vector<int> vi;

for (auto e: vi){
    // ...
}

```