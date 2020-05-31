## C++模板
### concept入门
笼统地说，Concept就是C++中的“类型的类型”

C++ Concept实际上是指模板对类型的某种约束（要求某类型具有某特性）。区分于抽象基类，Concept从*语义*上给出约束，而非从*语法*上（抽象基类的子类必须要有纯虚函数的实现）。经典的Concept主要是STL的几种迭代器（ `ForwardIterator, BidirectIterator, RandomAccessIterator`），它们约束的是语义而不要求具有特定继承关系。也就是说同样满足`RandomAccessIterator`的几个类不见得继承自同一个基类。

传统的Concept实现是基于所谓的SFINAE（Substitution Fail is not an error），这种晦涩难懂的特性本质上依赖的是编译器的模板匹配规则，通过匹配更合适的模板，并在匹配失败时使编译不通过来规定传入模板的类型的约束。它不仅极其不直观，而且容易给出不知所云的错误提示。

SFINAE举例：
```cpp
#include <type_traits>
struct T {
    enum { int_t,float_t } m_type;
    template <typename Integer,
              std::enable_if_t<std::is_integral_v<Integer>, int> = 0
    >
    T(Integer) : m_type(int_t) {}
 
    template <typename Floating,
              std::enable_if_t<std::is_floating_point_v<Floating>, int> = 0
    >
    T(Floating) : m_type(float_t) {}
};
```
对上例的分析：
- `is_intergral_v<T>`即`is_intergral<T>::value`，该布尔值为常量，表示T是否是整形。其可能的实现是模板偏特化（对所有整型偏特化value为`true`，其他为`false`）
- `is_floating_point_v`同上
- `enable_if_t<bool, T>`即是`enable_if<bool, T>::type`，其在第一个模板参数为`true`时就相当于`T`类型，在第一个模板参数为`false`时直接导致这一套模板不匹配（若所有备选的模板都不匹配则编译不通过）
  
  `enable_if`可以说是SFINAE的灵魂组件，虽然可能的实现很简单（还是模板偏特化）：
  ```cpp
  template<bool B, typename T=void>
  class enable_if {};
  template<typename T>
  class enable_if<true, T> { typedef T type; };
  ```

其他有用的组件
- `is_arithmetic` 是否为算术类型（也就是内置的数字类型）
- `decltype` 类型推断运算符。这不是库的一部分，是C++的语言特性
- `declval` 把给定的类型转换为引用类型
  
  这允许在编译阶段不进行实例化的情况下检查成员函数（比如测试是否成员函数是否存在，返回类型是什么），例如`decltype(declval<T>.foo())`得到了`T`类型的`foo()`函数的返回类型

C++20 将Concept作为语言特性引入了C++，目前编译器的支持还不完全
Concept举例：
```cpp
template <typename T>
concept Addable = requires(T x){
    x + x;
};

template <typename T>
concept Printable = requires(std::ostream& out, T x){
    out << x;
};

template <typename T>
class Array requires Addable<T> && Printable<T>
{
    // class body ...
};
```
说明：
- 定义concept的开头类似定义模板类，只是把class换成concept
- 虽然形式上相似，但concept不存在所谓的显式实例化、偏特化
- concept里面是由requires开头的*requires表达式*，表示这个concept对类型的要求，有两种基本语法：
  ```cpp
  requires {statments;}
  requires (args){statments;}
  ```
  其中`args`是一个形式上的“形参列表”，它是可选的，且完全不影响任何运行，这个列表只是为了形式上给出某些类的对象的名字，一遍书写约束表达式

  大括号内的表达式同样不会运行，它们表征的是“这个表达式必须是合法的，是可以通过编译的”
- class里面的requires开头的部分是*requires子句*（区分requires表达式！！），requires后面跟的是concept，用以表明该模板参数要满足哪一些concept
- 形象地区分两种requires用法：将requires表达式理解为一个布尔表达式，若给定类型满足要求则表达式为真，否则为假；而requires子句可以理解为类似`const` `override`的*限定符*，它相当于要求编译器进行检查
- requires子句中可以用`&&` `||` 来组合concept
### 应用
#### 单例类模板
```cpp
// singleton.h
template <typename T>
class Singleton
{
public:
    static T* getInstance(){
        return instance;
    }
private:
    static T* instance;
    Singleton(){}
};
template <typename T>
T* Singleton<T>::instance = new T;
// test.h
class Test
{
    friend class Singleton<Test>; // 为了Singleton能访问Test的构造，必须
private:
    Test();
    // 复制构造和赋值也禁掉
};
// main.cpp
Test* t = Singleton<Test>::getInstance();
```
这样，就可以方便地用友元和模板来复用单例模式