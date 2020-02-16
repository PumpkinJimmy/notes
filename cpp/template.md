## C++模板
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