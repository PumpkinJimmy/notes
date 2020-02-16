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