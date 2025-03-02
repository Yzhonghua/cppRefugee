- initializer_list可以用来初始化同构容器

```
#include <iostream>
#include <tuple>
#include <vector>
#include <list>
#include <array>
#include <set>
#include <map>
#include <string>

std::tuple<int, int> create_tuple() {
    return {1, -1};  // Error until N4387
    // 等价于进行了一次 std::tuple<int, int> ret_val = {1, -1};
    // 由于tuple是异构容器，而initializer_list只支持同构容器，所以在c++11和c++14都是非法的
    // 在c++17以后合法
    return std::tuple<int, int>{1, -1};
    return std::make_tuple(1, -1);
    // 后面两种都可以，第二种自动推断类型更简洁

    return std::tuple<int, int>{1, -1};
    // 列表初始化
    // 1:禁止窄化转换，比如double --> int
    // 2:如果类型定义了接受std::initializer_list的构造函数，优先调用
    // 对于tuple这种异构类型容器，则会作为参数调用构造函数
    return std::tuple<int, int>(1, -1);
    // 直接初始化
    // 1:允许窄化转换
    // 2:直接匹配构造函数，不考虑初始化列表
}

// 而其他的同构容器都可以使用initializer_list来构造，比如
void test_func() {
    std::vector<int> vec = {1,2,3,4,5};
    std::list<int> lst = {1,2,3,4,5};
    std::array<int, 5> arr = {1,2,3,4,5};
    std::set<int> s = {1,2,3,4,5};
    std::map<int, std::string> m = {{1, "one"}, {2, "two"}};
}

```

- TODO: why initializer_list?

- TODO: source code
