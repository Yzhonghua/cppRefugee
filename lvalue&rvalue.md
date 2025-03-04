- const对于左值&右值的两个作用(以及编译时字面量和变量)


- const对于左值&右值的两个作用
```
#include <iostream>
#include <string>

void func(const std::string& arg) {
    std::cout << arg << std::endl;
}

void func(const int& arg) {
    std::cout << arg << std::endl;
}

int main(int argc, char* argv[]) {
    // call 1
    std::string str = "string example";
    func(str);

    // call 2
    func("string example 2");

    // call 3
    func(42);

    return 0;
}
```
call 1，体现出了const在引用避免复制开销的情况下，对于左值参数的不修改保证。

call 2，体现出了const对于右值参数的包容，因为 T& 是不接受右值的（因为C++不允许修改临时对象），而 const T& 允许绑定右值，
尽管实际发生的事情是构造了一个临时对象（也只是在string下，如果是const int&那么只涉及字面量到变量的转换）
```
    func("string example 2");
    // 实际发生的事
    // const std::string temp("string example 2");
    // func(temp);
    // temp在func作用域结束后被自动销毁
```

call 3，相比call 2简单一些，因为它没有涉及对象的构造，但是核心仍然是发生了字面量到变量的转换。
```
    func(42);
    // 实际发生的事
    // const int temp = 42;
    // func(temp);
```
编译时字面量（如常量字面量42，"string"等）在变异阶段就确定了其值，他们的存储方式和代码区相关，而不是像普通变量存在堆栈。

TODO：存储于只读数据段或常量区（到底是哪个？）

TODO：相应的内存分区
