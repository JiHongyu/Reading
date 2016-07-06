

## Part 1: C++ 的编程方向

+ C语言的增强版
+ 面向对象编程
+ 泛型编程
+ STL

## Part 2: 尽量以const enum inline替换 \#define

### \#define的缺点
+ 编译器编译阶段会将宏定义等价替换，宏定义符号不会在后面的阶段出现，若后面出现了错误不好及时定位。
+ \#define不能创建一个基于一段作用域的常量（除非\#undef），缺少封装性。
+ 基于\#define的宏函数在操作自加/自减运算符时也会造成意想不到的错误。

### 基于class 的专属常量

最要不对CA的对象取地址操作，可以只有声明式而不需要定义式。(一下验证验证代码有误)
```c++
//xx.h
class CA{
    static const int NUM = 10;   //常量声明式
};

//xx.cpp
static const int NUM; //常量定义式
```

最好（容易理解）以如下写法
```c++
//xx.h
class CA{
    static const int NUM;   //常量声明式
};

//xx.cpp
static const int NUM = 10; //常量定义式
```
## Part 3: 尽量使用const

const 与变量和对象的结合无须赘述

###const 与函数返回值结合

避免未定义错误

###const 的成员函数

两个成员函数的常量性不同，是可以被重载的。为了避免常量性不同的重载成员函数的代码重复，可以使用non-const函数调用const函数，但是要注意类型转换（一次或两次）。
```c++
class Test
{
public:
    const int& operator[](int input) const{
        //...
    }
    int& operator[](int input){
        return const_cast<int&>(static_cast<const Test&>(*this)[input]);
    }
    ~Test();
};
```

## Part 4: 对象初始化

对象的初始化过程只发生在构造函数初始化列表里，而不是构造函数的函数体里。

成员变量的初始化顺序是依照其在class里的声明顺序而定。
```c++
class CTest{
    //以下两行交换顺序是错误的
    enum {NUM = 10};
    int pa[NUM];
};
```

## Part 5: C++ 的trival函数簇

trival函数簇包括有：

+ default构造函数

当有其他构造函数时，trival default构造函数不会生成。

+ 析构函数

析构函数的virtual性质由其父类决定，默认是非虚的。

+ copy构造函数
+ copy assignement操作符重载

两个函数功能相似，都是努力进行成员变量的复制（深拷贝）。但是operator=的生成局限性较大。比如成员变量有引用属性，常量属性，基类operator=是私有的（不满足复制操作）。

在适时的时候（需要被使用时），这些函数可以在编译阶段自动地生成。

## Part 6: 阻止trival函数簇自动生成

+ 声明并定义的私有函数（最不好）

+ 声明不定义的私有函数（可以）

+ 定义一个Uncopyable基类（Good）

Uncopyable类使用方法如下：
```c++
class Uncopyable{
protected:
    Uncopyable(){}
    ~Uncopyable(){}
private:
    Uncopyable(Uncopyable const&);
    Uncopyable& operator=(Uncopyable const&);
};

class Test: private Uncopyable
{
    //...    
};
```

## Part 7: 论virtual析构函数的重要性

在多态基类中，一般要将析构函数声明为 virtual 的，否则会造成对象的局部销毁。基类指针指向继承类对象，delete基类指针时会导致继承类的部分数据没有被销毁。

## Part 8: 析构函数的异常处理

无论如何，让析构函数抛出异常总归是不好的。可以让容易抛出异常的代码抽象出一个接口，让客户端代码调用，增强代码异常处理能力。

## Part 9: 不要再构造或析构函数中调用virtual函数

在继承链中的各个构造和析构函数调用中，被调的virtual函数都是同属于其等价的继承链中。

## Part 10: 令 operator= 返回一个reference to \*this

目的是为了满足类似于C++内置类型的链式复制。但是这只是一个约定，并无强制性。
类似的有：operator+=, operator-= 等等。

```c++
class CTest
{
public:
    CTest& operator=(CTest const& val){
        //...
        return *this;
    }
    CTest& operator+=(CTest const& val){
        //...
        return *this;
    }
};
```

## Part 11: operator= 处理“自我复制”

对象自我复杂的情况：

+ 
自我复制的成员安全性和异常安全性问题。特别是成员变量包含指针数据。
```c++
struct Data{
    int  len;
    int* pdata;
    Data(int len){/*...*/}
}

class CTest
{
private:
    Data * _data;
public:
    // 不具有成员变量安全性和异常安全性
    CTest& operator=(CTest const& val){
        delete _data;
        _data = new Data(val._data);
        return *this;
    }
    // 不具有异常安全性
    CTest& operator+=(CTest const& val){
        if( this == &val) return *this;
        delete _data;
        _data = new Data(val._data);
        return *this;
    }
    // Good code 虽然没有自我检查
      CTest& operator+=(CTest const& val){
        Data *pori = _data;
        _data = new Data(val._data);
        delete pori;
        return *this;
    }  
};
```
## Part 12: 复制对象勿忘其每一部分

自己本身的成员函数+基类的copy assignment函数。

## Part 13: 将资源封装成对象进行管理

基于对象的资源封装可以减少大量的资源泄露。这是因为资源（文件句柄，堆上的内存）的生命周期是不可控的，但是栈对象的生命周期是可控的。例如C++的内置智能指针，就是对原始指针的封装，保证对象在其作用域外肯定被释放掉：

+ auto_ptr: 指向唯一的智能指针，在copy构造函数或copy assignment函数中进行复制会使原来的指针指向 null。

+ shared_ptr (C++11): 基于引用计数器的智能指针，可以像正常指针一样使用。

注：auto_ptr和shared_ptr是基于delete而不是delete[]来回收资源所以不能将其作用域数组对象。

无论是否使用C++的内置资源管理器，对资源的封装是对资源控制的最好办法。

## Part 14: 资源管理类的copy行为

+ 禁止copy行为
+ 引用计数器共指(shared_ptr)
+ 转移底部资源的拥有权(auto_ptr)
+ 复制底部资源（深拷贝）

## Part 15: 在资源管理类中对原始资源的访问

由于某些API需要原始资源作为输入，这样就必须将原始资源暴露在外面。这样破坏资源管理类的封装性，但也没办法。暴露方法如下：

+ get函数。auto_ptr和shared_ptr都提供get方法获得原始指针
+ 隐式转换函数

下面的例子将呈现C++转换构造函数和隐式转换函数的用法和行为（这个例子能说明很多问题）。
```c++
class CTest{
private:
    int a;
public:
    //转换构造函数，没有explicit关键字的单参数构造函数
    CTest(int a):a(a){}
    //隐式转换函数
    operator int() const{return a;}
    //copy assignment函数
    //CTest& operator=(int a){a = a;return *this;}
};

int main()
{
    CTest test=10; //转换构造函数
    test = 200;    //没有operator=时调用，转换构造函数
    int i = test;  //隐式转换函数
    return 0;
}
```
一般而言，转换构造函数和隐式转换函数的优先级都比较弱。

## Part 16: 成对的new与delete要采取相同的形式

小心 typedef 陷阱。

```c++
int *p1 = new int;
delete p1;

int *p2 = new int[10];
delete[] p2;

typedef int IntAarray[10];
int *p3 = new IntAarray;//数组对象
delete[] p3; //这里要加[]
```

其实为了避免错误，应当尽量少用typedef定义数组别名。

## Part 17: 不要在函数参数调用过多的方法

函数参数中的代码执行顺序是未定义的，取决于编译器的实现方式及代码优化。


## Part 18: 让接口容易被使用，不容易被误用

## Part 19: 设计Class的关键点

+ class关于 operator new、operator new[] 、operator delete、operator delete[] 的设计需求
+ 对象的初始化和赋值的差异对待
+ copy构造函数的值传递实现
+ class 参数的错误检查
+ class 的继承关系链
+ class 的显示或隐式转换规则
+ class 的成员函数以及其public、private和protected性质。
+ class 的弹性范围
+ 甄别是否需要一个新的class

## Part 20: 值传递和引用传递

值传递的适用场景：
+ 内置类型
+ 函数对象（仿函数）
+ STL的迭代器

引用传递的适用场景：除值传递的适用场景。

引用传递的本质是指针传递。引用传递可以避免频繁的对象生成与销毁，对象切割（无法多态）。

## Part 21: 值返回，引用返回

局部对象千万不要引用返回。有时候值返回是必要的，虽然其造成了一点性能上的降低。

## Part 22: 成员变量的控制访问权限

+ 提供封装：private
+ 不提供封装：public、protected

public和protected成员意味着其变量或方法可能会被外部或者其继承链中的其他类任意使用。Class中的public和protected成员越多就意味着该class的封装性越差，其维护成本越高。原则上成员变量一律用private属性。

## Part 23: 成员函数与非成员函数

class的封装性不一定体现在成员函数的数量上。需要有效的利用namespace。

## Part 24: 所有类型皆需类型转换，需要使用非成员函数（什么鬼）

## Part 25: 实现一个高效的swap函数

C++ 内置的swap函数（vs2008，<utility\>）如下：
```c++
namespace std {
    template<class _Ty> inline
        void swap(_Ty& _Left, _Ty& _Right)
        {   // exchange values stored at _Left and _Right
        if (&_Left != &_Right)
            {   // different, worth swapping
            _Ty _Tmp = _Left;
            _Left = _Right;
            _Right = _Tmp;
            }
    }
}
```

该函数函数是基于对象的`copy构造函数`和`copy assignment函数`实现的。默认算法是正确的，但可能也是低效的。例如如下class
```c++
class CTest{
private:
    int * pdata;
};
```
在交换两个`CTest`对象时，我们只需要将其成员变量`pdata`进行交换即可，但是基于`copy构造函数`和`operator=函数`会带来性能上的缺失（一般我们会进行深拷贝）。
为了在标准库的swap基础上构建基于`CTest`类的高效的swap函数，我们可以做以下演进。

基于非成员函数模板特化：
```c++
namespace std{
    template<>
        void swap<CTest>(CTest &_l, CTest &_r){
            //priavet member, disallowed access
            std::swap(_l.pdata, _r.pdata);
        }
}
```
该方法有个致命缺点，需要直接成员变量，所以对私有成员变量会直接报错。而却该方法直接修改了std代码，这是不好的习惯。

基于成员函数-非成员函数模板特化：
```c++
class CTest{
private:
    int * pdata;
public:
    void swap(CTest & _r){
        using std::swap;
        swap(pdata, _r.pdata);
    }
};

namespace std{
    template<>
        void swap<CTest>(CTest &_l, CTest &_r){
            _l.swap(_r);
        }
}
```
该方法看似完美，但是当`CTest`为模板类时，会出现错误（模板函数偏特化）。
解决函数模板偏特化的问题可以使用函数重载。
```c++
namespace std{
    template<typename T>
        void swap(CTest<T> &_l, CTest<T> &_r){
            _l.swap(_r);
        }
}
```
但是该方法也是拆了东墙补西墙的方法，最大的问题是引入了好多杂七杂八的东西在`std`里面。

最好的解决方法是将非成员的`swap函数`放置于`CTest类`同命名空间里。

## Part 26: 尽可能恰好在变量使用前定义

即，变量定义时间越往后越好。

## Part 27: C++转型之苦

C风格转型，作用与C语言的转换类似：

+ (T)expression 
+ T(expression)

C++风格转型：

+ const_cast <T\> (expression)

对象常量性质转移(const <> non-const)

+ dynamic_cast <T\> (expression)

安全的向下转型，无法用C风格转型 **直接** 实现

+ reinterpret_cast <T\> (expression)

任意转换，一般用于低级代码。

+ static_cast <T\> (expression)

强迫隐式转换。无法将 const 转换为 non-const

在C++编程中尽量少用转换，当代码中出现了类型转换，要仔细想想是否自己的代码错了。如果要用类型转换，尽量用C++风格的类型转换，减少非法类型转换带来的错误。

类型转换是需要运算开销的，比如整型转浮点数，以及继承链的指针转换。

```c++
class CBase1{};

class CBase2{};

class CDerived:public CBase1,public CBase2{};

int main()
{
    CDerived d;
    CBase1 *pb1 = &d;
    CBase2 *pb2 = &d;
    // the same as
    //CBase1 *pb1 = static_cast<CBase1*>(&d);
    //CBase2 *pb2 = static_cast<CBase2*>(&d);
    cout<<&d<<endl;
    cout<<pb1<<endl;
    cout<<pb2<<endl;
    return 0;
}
```
上面代码中`pb1`和`pb2`都指向同一个对象，但它们的地址值却不一样。

## Part 28: 避免返回指向对象内部的handle

handle 包括 reference , point, iterator

## Part 29: 异常安全

异常安全的准则：

+ 不泄露任何资源
+ 不允许数据破坏

## Part 30: inline 函数

inline函数一般来说会增加目标代码量，但是有时编译器优化也会使其产生出代码量更小的代码。

inline函数声明方法：

+ 显式声明: inline关键字
+ 隐式声明: 在class内定义的成员函数

inline函数一般是定义在头文件里。同时inline声明也是一个推荐声明，不是强制的。

## Part 31: 文件依赖性

typedef不能用作前置声明。

指针往往是实现对象解耦和的关键。

+ 能使用对象引用或对象指针的地方就不要用对象本身
+ 如果允许，尽量以class声明替代class定义式
+ 为声明式和定义式提供不同的头文件
+ Handle class
+ Interface class

## Part 32: public 继承意味"is-a"


## Part 33: derived类会屏蔽base类的名字

derived类的作用域$\subset$base类的作用域

C++的名字搜索空间是嵌套搜索规则，从某一名字空间开始，迭代的搜索该空间和其超空间，若匹配名字则停止搜索。通过using关键字可以手工设定名字空间的搜索顺序。名字匹配并不意味着正确的匹配，比如类型匹配是被忽略的。

下面的例子说明了继承关系的名字搜索规则
```c++
class Base{
public:
    virtual void mf1()=0;
    virtual void mf1(int);
    virtual void mf2();
    void mf3();
    void mf3(int);
};

class Derived: public Base
{
public:
    virtual void mf1();
    void mf3();
    void mf4(); 
};

Derived d;
d.mf1()    // OK
d.mf1(1)   // ERROR
d.mf2()    // OK
d.mf3()    // OK
d.mf3(1)   // ERROR
d.mf4()    // OK
```

Derived类经过下面修改后，则可以直接调用所有Derived和Base的函数
```c++
class Derived: public Base
{
public:
    using Base::mf1;
    using Base::mf3;
    virtual void mf1();
    void mf3();
    void mf4(); 
};
```

## Part 34: virtual and non-virtual functions

## Part 35: virtual 函数的替代方案

## Part 36: 最好不要重写基类的non-virtual函数

virtual函数：动态绑定
non-virtual函数：静态绑定

## Part 37: 不要重新定义继承而来的缺省参数值

缺省参数值是静态绑定。

## Part 38: Composition means "has-a"

## Part 39: private继承

private继承的使用情况有点类似于Python的装饰器。private继承有一点类似于成员变量对象（Composition）的感觉。

private继承的适用条件：

+ 继承类需要重写基类的virtual函数
+ 继承类需要访问基类的protected成员

private继承可以对空基类的存储空间进行优化。

## Part 40: 慎用多重继承

## Part 41: 隐式接口与编译期多态

面向对象是基于显式接口和运行时多态。

泛型编程是基于隐式接口和编译期多态。（有点问题吧）

## Part 42: typename的使用

+ 模板类型关键字
+ 嵌套从属类型关键字

typename应用场景：
```c++
template<typename T>                  //模板关键字
void func(T const& val){
    typename T::const_iterator iter;  //嵌套从属关键字
    //...
}
```
在非嵌套从属关键字前不能加typename。特例，在基类的嵌套从属列表中不能用。

```c++
template<typename T>
    class Derived: public Base<T>::Nested{//禁止 typename
    public:
        explicit Derived(): Base<T>::Nested   //禁止 typename
            {
                typename Base<T>::Nested t;       //可以 typename
                //...
            }
    };
```

基于嵌套从属的typename关键字的实现好像不好，在不同的编译器特别是老旧编译器会出现各种问题。
