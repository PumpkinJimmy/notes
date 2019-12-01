## 异常机制笔记
- 主要涉及exception和stdexception两个头文件
- throw obj 抛出异常，返回一个对象。可以使基本类型（如int），也可以是任何对象
- try/catch( *ExceptionType* e) try块中抛出的一场由类型相应catch块捕获，抛出的对象赋值给e。另外，catch(...)表示捕获所以类型的异常。
- throw 单用，用于catch块中重新抛出
- 适当构建异常类继承层次并适当声明catch类型可以利用多态
- 异常抛出时会进行栈解退，即正确析构所有局部变量
- 被忽视的异常最终导致std::terminate的调用（terminate的默认行为是调用abort）。这一默认行为可以修改
- 函数可以声明可能抛出的异常类型throw( *Exception1*, *Excetion2*)函数抛出的这个列表之外的异常不允许被捕获。这个特性不建议用。唯一好用的是noexcept声明。
