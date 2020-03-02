## C++ 面向对象
### static
static在类里面用于声明静态类成员与静态成员函数。

静态类成员一个类只有一份（而不是每个实例都有一份）

1. 静态类成员
   - 在类定义内里加上static，初始化时不用加
   - 其本质是全局/静态区的全局变量，生存期是整个程序运行期间，**不随实例析构而析构**
   - 类似全局变量，需要手动初始化
   - 一般在**类定义以外**初始化（且一般在全局区初始化），语法同一般全局变量的定义，只要再加上类作用域符号
   - 访问控制只限制访问，**类外初始化不受影响**
   - 若是`const static`则可以在类定义内初始化
   - 小心重复初始化，不要在头文件里编写静态类成员初始化
2. 静态成员函数
   - 在类定义内里加上static，编写成员函数的实现时不用加
   - 没有this指针
   - 可以直接访问静态类成员
   - 可以直接访问类作用域来调用，不需要实例
3. 一个应用：实现单例模式
   ```cpp
   // singleton.h
   class Singleton
   {
    private:
        Singleton();
        Singleton& Singleton(const Singleton&);
        Singleton& operator=(const Singleton&);
        static Singleton* instance;
    public:
        static Singleton* getInstance();
   };
   // singleton.cpp
   Singleton* Singleton::instance = nullptr;
   Singleton* Singleton::getInstance()
   {
       if (instance == nullptr)
           instance = new Singleton;
           return instance;
   }
   ```

### 多态
- 运行时多态的典型实现是虚函数表，本质上就是函数指针：同一个接口，不同的行为
- C++的具体编码中，多态需要通过虚函数+继承+指针/引用来实现
- 在很多情形下，多态需要指针来体现，故常常面临内存管理问题（如指针指向的内存需要动态分配，使得构造函数的得不到正确的参数），处理的常见技巧如下：
  1. 智能指针（用`unique_ptr<>`来移交内存所有权，`shared_ptr<>`无脑解决），缺点是要套智能指针，烦
  2. 工厂方法/抽象工厂，直接用工厂组装好再交给使用多态特性的用户类，缺点是容易出错