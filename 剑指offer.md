
## 面试题 45：圆圈中最后剩下的数字（Josephuse 环）

#### 解法 1：

环形链表模拟

#### 解法 2：

递推公式。

## 面试题 46：不用控制流语句求累加和

#### 解法 1

利用静态成员变量和构造函数

#### 解法 2

虚函数方法。

关键语句：`!!n == 1 if n != 0 else 0`

#### 解法 3

函数指针方法。


## 面试题 47：不用加减乘除做加法

#### 解法

用位运算模拟：

1. 数字各位相加
2. 数字个进位相加
3. 各位数与各进位数迭代相加


## 面试题 48：不能被继承的类

#### 解法 1

把构造函数设为私有函数

缺点：只能在对上创建

#### 解法 2

友元模板 + 虚继承

缺点：可移植性不好。

