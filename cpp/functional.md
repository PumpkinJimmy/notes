## functional
### function
这个模板的意义在于把C++里的各种可调用的东西：
- 函数指针
- 函数对象（重载了operator()的可调用对象）
- lambda函数
- 成员函数
抽象出来，作为一个**适配器**
### bind
mvp之一，他可以把参数预先绑定到可调用对象的某个参数上，它返回的是一个函数对象。
注意placeholders::_1之类的占位符的使用，让bind可以部分绑定。
最赞的是可以用这个方法使用成员函数：
```cpp
Foo foo;
auto func = std::bind(&Foo::bar, &foo);
```
注意：
1. 成员函数取地址不可少（不知道为什么）
2. 这种操作理解为成员函数的第一个参数其实是this指针（是你吗self），但是取地址可有可无
3. 绑定了this之后之后不要忘了成员函数本身的参数（用placeholders）（不可用bind1st偷懒）
