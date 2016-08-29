
本文均摘自 Internet

[设计模式原则详解](http://blog.csdn.net/hguisu/article/details/7571617 "设计模式原则详解")

[设计模式](http://www.runoob.com/design-pattern/design-pattern-intro.html)
# 设计模式基本原则

我们在应用程序开发中，一般要求尽量两做到可维护性和可复用性。

应用程序的复用可以提高应用程序的开发效率和质量，节约开发成本，恰当的复用还可以改善系统的可维护性。而在面向对象的设计里面，可维护性复用都是以面向对象设计原则为基础的，这些设计原则首先都是复用的原则，遵循这些设计原则可以有效地提高系统的复用性，同时提高系统的可维护性。 面向对象设计原则和设计模式也是对系统进行合理重构的指导方针。

常用的面向对象设计原则包括7个，这些原则并不是孤立存在的，它们相互依赖，相互补充。

+ 单一职责原则
+ 开闭原则
+ 里氏替换原则
+ 依赖倒转原则
+ 迪米特法则
+ 合成复用法则
+ 接口隔离法则

# 设计模式

## 工厂模式

在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。

```c++
// 产品类
class Product{
public:
    virtual void func() = 0;
};

class ProductA: public Product{
public:
    void func(){/* Do something */}
};

class ProductB: public Product{
public:
    void func(){/* Do something */}
};

// 工厂类
class Factory{
public:
    virtual Product* product() = 0;
};

class FactoryA: public Factory{
public:
    Product* product(){return new ProductA;}
};

class FactoryB: public Factory{
public:
    Product* product(){return new ProductB;}
};


int main(){
    int choose = 0;
    cin>>choose;

    Product* p = NULL;
    Factory* f = NULL;

    switch(choose){
        case 1:
            f = new FactoryA;
            p = f->product();
        case 2:
            f = new FactoryB;
            p = f->product();
    }
    exit(0);
}
```

## 抽象工厂

抽象工厂模式（Abstract Factory Pattern）是围绕一个超级工厂创建其他工厂。该超级工厂又称为其他工厂的工厂。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

在抽象工厂模式中，接口是负责创建一个相关对象的工厂，不需要显式指定它们的类。每个生成的工厂都能按照工厂模式提供对象。

```c++
// 一系列的产品类
class ProductA{
public:
    virtual void funcA() = 0;
};

class ProductA1: public ProductA{
public:
    void funcA(){}
};

class ProductA2: public ProductA{
public:
    void funcA(){}
};

class ProductB{
public:
    virtual void funcB() = 0;
};

class ProductB1: public ProductB{
public:
    void funcB(){}
};

class ProductB2: public ProductB{
public:
    void funcB(){}
};

// 工厂类
class Factory{
public:
    virtual ProductA* CreateProductA() = 0;
    virtual ProductB* CreateProductB() = 0;
};

class Factory1: public Factory{
public:
    ProductA* CreateProductA(){ return new ProductA1;}
    ProductB* CreateProductB(){ return new ProductB1;}
};

class Factory2: public Factory{
public:
    ProductA* CreateProductA(){ return new ProductA2;}
    ProductB* CreateProductB(){ return new ProductB2;}
};


int main(){
    int choose = 0;
    cin>>choose;

    ProductA* pa = NULL;
    ProductB* pb = NULL;
    Factory* f = NULL;

    switch(choose){
        case 1:
            f = new Factory1;
            pa = f->CreateProductA();
            pb = f->CreateProductB();
        case 2:
            f = new Factory2;
            pa = f->CreateProductA();
            pb = f->CreateProductB();
    }
    exit(0);
}
```

## 单例模式

单例模式一般会涉及到多线程问题，所以单例模式一般会有两个版本。

单线程版单例模式：

```c++
class Singleton{
private:
    Singleton(){}
    static Singleton* m_obj = nullptr;
public:
    static Singleton* Instance(){
        if (m_obj == nullptr)
            m_obj = new Singleton;
        return m_obj;
    }
};
```

多线程版单例模式：

```c++

// 外部加锁解锁函数
extern void Lock();
extern void Unlock();

// 懒汉式，双重锁
class Singleton{
private:
    Singleton(){}
    static Singleton* m_obj = nullptr;
public:
    static Singleton* Instance(){
        if (m_obj == nullptr){
            Lock();
            if (m_obj == nullptr){
                m_obj = new Singleton;
            }
            Unlock();
        }
        return m_obj;
    }
};

// 内部静态实例的懒汉式
class Singleton2{
private:
    Singleton2(){}
public:
    static Singleton2* Instance(){
        Lock(); // C++11 不需要
        static Singleton2 m_ins;
        Unlock(); // C++11 不需要

        return &m_ins;
    }
};

// 饿汉式，即程序一开始就会产生该类的实例
class Singleton3{
private:
    Singleton3(){}
    static const Singleton3* m_obj;
public:
    static Singleton3* Instance(){
        return m_obj;
    }
}

static const Singleton3::m_ins = new Singleton3;

```

## 建造者模式

将一个复杂的对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

[解惑](http://www.cnblogs.com/BeyondAnyTime/archive/2012/07/19/2599980.html)

```c++
class Product;

class Builder{
public:
    virtual void build1() = 0;
    virtual void build2() = 0;
};

class ConcreteBuilder1: public Builder{
private:
    Product  m_product;
public:
    void build1(){}
    void build2(){}

    Product GetResult(){return m_product;}
}

class Director{
public:
    void Construct(Builder &builder){

        builder.build1();
        builder.build2();

    }
}

int main(){
    Director director;
    Builder * builder = new ConcreteBuilder1;
    director.Construct(*builder);
    Product product = builder->GetResult();
    delete[] builder;
    exit(0);
}
```

## 原型模式

在 C++ 中，原型模型本质上相当于 **拷贝构造函数**。

## 适配器

[参考](http://www.cnblogs.com/jiese/p/3166396.html)

适配器可以分为：类适配和对象适配。

```c++
class Target{
public:
    virtual void Request() {} 
};

class Adaptee{
public:
    virtual void SpecialRequest() {}
};

// 类适配
class Adapter1: public Target,   // 接口继承
               private Adaptee  // 实现继承
               {
public:
    void Request(){    // 适配
        // do something
        SpecialRequest();
    }
}

// 对象适配
class Adapter2: public Target{
private:
    Adaptee*  m_adaptee;
public:
    Adapter2(Adaptee* adaptee):m_adaptee(adaptee){}
    void Request(){    // 适配
        // do something
        m_adaptee->SpecialRequest();
    }
}
```
## 桥接模式

将抽象部分与它的实现部分分离，使它们都可以独立地变化。

```c++
class Implementor{
public:
    virtual void OperationImp() = 0;
};

class ConcreteImplementor1: public Implementor{
public:
    void OperationImp(){}
};

class ConcreteImplementor2: public Implementor{
public:
    void OperationImp(){}
};

class Abstraction{
public:
    virtual void Operation() = 0;
};

class RefinedAbstraction: public Abstraction{
private:
    Implementor*   m_imp;
public:
    RefinedAbstraction(Implementor* imp):m_imp(imp){}
    void Operation(){
        m_imp->OperationImp();
    }
};
```

## 组合模式

组合模式（Composite Pattern），又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。这种类型的设计模式属于结构型模式，它创建了对象组的树形结构。
这种模式创建了一个包含自己对象组的类。该类提供了修改相同对象组的方式。
我们通过下面的实例来演示组合模式的用法。实例演示了一个组织中员工的层次结构。

## 装饰器模式

动态地给一个对象添加一些额外的职责。

```c++
class Component{
public:
    virtual void Operation() = 0;
};

class ConcreteComponent: public Component{
public:
    void Operation(){}
};

class Decorator: public Component{
private:
    Component * m_component;
public:
    void SetDecoration(Component * _cmp){m_component = _cmp;}
    void Operation(){ m_component->Operation();}
}

class ConcreteDecorator: public Decorator{
public:
    void Foo(){}
    void Operation(){
        Decorator::Operation();
        Foo();
    }
}

int main(){

    Component * c = new ConcreteComponent;

    // 原始操作
    c->Operation(); 

    Decorator * dc = new ConcreteDecorator;
    dc->SetDecoration(c);

    // 增加装饰器后的操作
    dc->Operation();

    exit(0);
}
```

## 外观模式

有点类似于组合模式。

为子系统中的一组接口提供一个一致的界面，Facade模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

## 享元模式

[参考资料](http://www.cnblogs.com/cxjchen/p/3194379.html)

## 代理模式

[参考资料](http://www.cnblogs.com/jiese/p/3177491.html)

代理模式又有点像装饰器。。

为其他对象提供一种代理以控制对这个对象的访问。

适用性：

在需要用比较通用和复杂的对象指针代替简单的指针的时候，使用Proxy模式。下面是一 些可以使用Proxy 模式常见情况： 
1) 远程代理（Remote Proxy ）为一个对象在不同的地址空间提供局部代表。 NEXTSTEP[Add94] 使用NXProxy 类实现了这一目的。

2 )虚代理（Virtual Proxy ）根据需要创建开销很大的对象。

3) 保护代理（Protection Proxy）控制对原始对象的访问。保护代理用于对象应该有不同 的访问权限的时候。

4 )智能指引（Smart Reference）取代了简单的指针，它在访问对象时执行一些附加操作。 它的典型用途包括：
对指向实际对象的引用计数，这样当该对象没有引用时，可以自动释放它(也称为SmartPointers[Ede92 ] )。

```c++
class Subject{
public:
    virtual void Request() = 0;
};

class RealSubject: public Subject{
public:
    void Request(){}
};

class Proxy: public Subject{
private:
    Subject*  m_subject = nullptr;
public:
    void Request(){
        if (m_subject == nullptr){
            m_subject = new RealSubject;
        }
        m_subject->Request();

        // do something
    }
};
```

## 解释器模式

解释器模式（Interpreter Pattern）提供了评估语言的语法或表达式的方式，它属于行为型模式。这种模式实现了一个表达式接口，该接口解释一个特定的上下文。这种模式被用在 SQL 解析、符号处理引擎等。

## 模板方法模式

定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。TemplateMethod 使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

```c++
class AbstractClass{
public:
    void TemplateMethod(){
        PrimitiveOperation1();
        PrimitiveOperation2();
    }
protected:
    virtual void PrimitiveOperation1() = 0;
    virtual void PrimitiveOperation2() = 0;
};

class ConcreteClass: public AbstractClass{
protected:
    void PrimitiveOperation1(){/* Do something*/}
    void PrimitiveOperation2(){/* Do something*/}
};
```

## 责任链

```c++
class Request{};

class Handler{
protected:
    Handler *m_successor;
public:
    void SetSuccessor(Handler* handler){
        m_successor = handler;
    }
    virtual void GetRequest(Request* request) = 0;
};

class ConcreteHandler1: public Handler{
private:
    bool SomeCondition(Request* r){ /* Do something */ }
public:
    void GetRequest(Request* request){
        if ( SomeCondition(request) ){
            // Do something
        }else{
            m_successor->GetRequest(request);
        }
    }
}

class ConcreteHandler2: public Handler{
private:
    bool SomeCondition(Request* r){ /* Do something */ }
public:
    void GetRequest(Request* request){
        if ( SomeCondition(request) ){
            // Do something
        }else{
            m_successor->GetRequest(request);
        }
    }
}

int main(){
    
    // 请求对象
    Request *r = new Request;

    // 任务处理对象
    Handler * h1 = new ConcreteHandler1;
    Handler * h2 = new ConcreteHandler2;

    // 设置任务链关系
    h1->SetSuccessor(h2);

    // 分发请求数据
    h1->GetRequest(r);

    exit(0);
}
```

## 命令模式

命令模式（Command Pattern）是一种数据驱动的设计模式，它属于行为型模式。请求以命令的形式包裹在对象中，并传给调用对象。调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象，该对象执行命令。

[参考资料1](http://blog.csdn.net/lcl_data/article/details/9080909)
[参考资料2](http://www.cnblogs.com/jiese/p/3190414.html)

```c++

class ReceiverA;
class ReceiverB;

// 命令类
class Command{
public:
    virtual void Execute() = 0;
};

class CommandA{
private:
    ReceiverA*    m_receiver;
public:
    CommandA( ReceiverA* r):m_receiver(r){}
    void Execute(){m_receiver->ActionA();}
};

class CommandB{
private:
    ReceiverB*    m_receiver;
public:
    CommandB( ReceiverB* r):m_receiver(r){}
    void Execute(){m_receiver->ActionB();}
};

// 接受者，命令真正的执行体，可以是任何类
class ReceiverA{
public:
    void ActionA(){}
};

class ReceiverB{
public:
    void ActionB(){}
}

// 请求者
class Invoker{
    
private:
    deque<Command*>   m_command_queue;

public:

    void AddCommand(Command* command){
        m_command_queue.push_back(command);
    }

    void Invoke(){
        Command * _c = NULL;
        while(!m_command_queue.empty()){
            _c = m_command_queue.front();
            _c->Execute();
            m_command_queue.pop_front();
        }
    }
}

int main(){

    //模拟两个命令执行
    Invoker invoker;

    ReceiverA  ra;
    ReceiverB  rb;

    Command * c1 = new CommandA(&ra);
    Command * c2 = new CommandB(&rb);

    invoker.AddCommand(c1);
    invoker.AddCommand(c2);

    invoker.Invoke(); 

    exit(0);
}
```

## 迭代器模式

提供一种方法顺序访问一个聚合对象中各个元素, 而又不需暴露该对象的内部表示。

例子写的不好，还是要参考 STL 的代码。

```c++
template<typename T>
class Iterator{
public:
    virtual void Next() = 0;
    virtual bool Finished() = 0;
    virtual T* CurrentItem() = 0;
    virtual ~Iterator(){}
};

template<typename T, unsigned int NUM>
class CreateIterator: public Iterator<T>{
private:
    ConcreteAggregate<T> *   m_aggregate;
    unsigned int m_idx;
public:
    CreateIterator(ConcreteAggregate<T,NUM>* agg):m_aggregate(agg),m_idx(0){}
    void Next(){ ++m_idx;}
    bool Finished(){ return m_idx==NUM? true: false;}
    T* CurrentItem(){ &((*m_aggregate)[m_idx])}
};

class Aggregate{
public:
    virtual Iterator* CreateIterator() = 0;
    virtual ~Aggregate(){}
};

template<typename T, unsigned int NUM>
class ConcreteAggregate: public Aggregate{
private:
    T  m_data[NUM];
public:
    Iterator* CreateIterator(){
        return new CreateIterator<T, NUM>(this);
    }

    T& operator[](unsigned int idx){
        return m_data[idx];
    }
};
```

## 中介者模式

定义一个中介对象来封装系列对象之间的交互。中介者使各个对象不需要显示地相互引用，从而使其耦合性松散，而且可以独立地改变他们之间的交互。

相当于将对象之间的通信用中介者对象封装起来。

## 备忘录模式

对一个需要保存状态的 Originator类 增添两个类：

1. 状态类：存储 Originator对象的状态以及方便恢复。
2. 容器类：存储这些状态类以便恢复。

```c++
#include <iostream>  
#include <string>  
using namespace std;  

class Memento {  

private:  
    string state;  
public:  
    Memento(string state){  
        this->state = state;  
    }  
    string getState() {  
        return state;  
    }  
    void setState(string state) {  
        this->state = state;  
    }  
};  
  
  
class Originator {  

private :  
    string state;  
public:  

    string getState() {  
        return state;  
    }  
    void setState(string state) {  
        this->state = state;  
    }  
    Memento createMemento(){  
        return Memento(this->state);  
    }  
    void restoreMemento(Memento memento){  
        this->setState(memento.getState());  
    }  
};  

class Caretaker { 

private :
    Memento memento;  
public :  
    Memento getMemento(){  
        return memento;  
    }  
    void setMemento(Memento memento){  
        this->memento = memento;  
    }  
}; 

int main (int argc, char *argv[])     
{  
    Originator originator;  
    originator.setState("状态1");  
    cout<<"初始状态:"<<originator.getState()<<endl;  
    Caretaker caretaker;  
    caretaker.setMemento(originator.createMemento());  
    originator.setState("状态2");  
    cout<<"改变后状态:"<<originator.getState()<<endl;  
    originator.restoreMemento(caretaker.getMemento());  
    cout<<"恢复后状态:"<<originator.getState()<<endl;  
}  
```

## 观察者模式

当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。比如，当一个对象被修改时，则会自动通知它的依赖对象。观察者模式属于行为型模式。

```c++
// Observer
class Observer{
public:
    virtual void update(Subject*)=0;
    ~Observer(){}
};

class ConcreteObserverA: public Observer{
public:
    void update(Subject*){
        /* Do something. */
    }
};

class ConcreteObserverB: public Observer{
public:
    void update(Subject*){
        /* Do something. */
    }
};

// Subject

class Subject{
private:
    list<Observer*>    m_observers;
public:
    void attach(Observer* observer){
        m_observers.push_back(observer);
    }
    void detach(Observer* observer){
        m_observers.erase(observer);
    }

    virtual void notify()=0;

};

class ConcreteSubjectA: public Subject{
public:
    void notify(){
        for(e : m_observers){
            e->update(this);
        }
    }
}

class ConcreteSubjectB: public Subject{
public:
    void notify(){
        for(e : m_observers){
            e->update(this);
        }
    }
}
```

## 状态模式

在状态模式（State Pattern）中，类的行为是基于它的状态改变的。这种类型的设计模式属于行为型模式。
在状态模式中，我们创建表示各种状态的对象和一个行为随着状态对象改变而改变的 context 对象。

上下文环境（Context）：它定义了客户程序需要的接口并维护一个具体状态角色的实例，将与状态相关的操作委托给当前的Concrete State对象来处理。

抽象状态（State）：定义一个接口以封装使用上下文环境的的一个特定状态相关的行为。

具体状态（Concrete State）：实现抽象状态定义的接口。 


## 策略模式
在策略模式（Strategy Pattern）中，一个类的行为或其算法可以在运行时更改。这种类型的设计模式属于行为型模式。

在策略模式中，我们创建表示各种策略的对象和一个行为随着策略对象改变而改变的 context 对象。策略对象改变 context 对象的执行算法。


## 访问者模式

在访问者模式（Visitor Pattern）中，我们使用了一个访问者类，它改变了元素类的执行算法。通过这种方式，元素的执行算法可以随着访问者改变而改变。根据模式，元素对象已接受访问者对象，这样访问者对象就可以处理元素对象上的操作。