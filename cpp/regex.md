## C++ Regex
### Quick Start
```cpp
#include <regex>
string fnames[] = {"foo.txt", "bar.txt", "baz.dat", "zoidberg"};
regex fname_pat("([a-z]+)\\.txt");
smatch m;

for (const auto &fname : fnames) {
    if (regex_match(fname, m, fname_pat)) {
        if (m.size() == 2) {
            ssub_match g1 = m[1];
            string base = g1.str();
            cout << fname << " has a base of " << base << '\n';
        }
    }
}
```

### 基础
- 位于头文件`<regex>`中
- `regex`表示一个正则表达式
  - `regex`是`basic_regex<char>`的别名
  - 默认的正则表达式语法是`ECMAScript`（也就是JS和Python的语法）
- `match_result`类模板用于保存匹配结果
  - `cmatch = match_result<char>; smatch = match_result in string`
  - 下标访问匹配组的信息：`m[0], m[1], ...`
  - `match_result.str()`返回匹配的内容，`match_result.first/second`表示匹配内容的迭代器
- `regex_iterator`：迭代器，访问每个匹配模式的串（相当于findall）