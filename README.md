第一遍阅读，只记最重点的，每章都不能记太多

前七章，除了6.6，6.7节，都要通读。通读完，去刷三个小时习题集巩固一下

---

#### 第一章：开始（10.27）

一个函数的定义包含四部分：返回类型，函数名，一个括号包围的形参列表，函数体

return语句容易被忽略，忘记写分号会导致莫名其妙的编译错误。

标准哭 iostream 库包含两个基础类型 istream 和 ostream，定义了 4 个 IO 对象，cin 为 istream 类型，cout、cerr、clog 为 ostream 类型

作用域运算符：`::`

输出运算符：`<<` ，接受两个运算对象，左侧为 ostream 对象，右侧为值，该运算符计算结果为左侧运算对象，因此可以将输出连起来 `std::cout << "A" << 1`。

C++ 有两种注释：单行注释；界定符对注释。当注释界定符跨越多行时，最好能显式指出内部都为多行注释的一部分，本书采用的风格为：注释每行都以一个星号开头。==注意，注释界定符不能嵌套==

```c++
/*
* 简单主函数
* 读取两个数，求和
*/
```

如果使用一个 istream 对象作为条件，例如 `while (std::cin >> value)` ，则效果是检测流的状态，流未遇到错误则检测成功，遇到文件结束符或无效输入则检测为假。==对 Linux 系统，文件结束符为 `Ctrl + D`==

==编译报错处理的两个好习惯==

1. 按照报错顺序逐个修正错误。单个错误通常具有传递效应，导致编译器报告比实际多得多的错误信息
2. 每修正一个错误就立即重新编译代码，或者修正一小部分明显的错误后就重新编译，这就是所谓的 “编辑-编译-调试”（edit-compile-debug） 周期

我们通常使用 `.h` 文件作为头文件的后缀，但也有程序员习惯 `.H` 、`.hpp`、`.hxx` ，标准库头文件通常不带后缀，事实上，编译器一般不关心文件名的形式，但有的 IDE 对此有特定要求。

包含来自标准库的头文件时，应该用==尖括号==；不属于标准库的头文件，应该用==双引号==。

==文件重定向==：测试程序时，反复从键盘敲入数据作为程序输入是非常乏味的。<font color = blue>自己试了，在 Linux 中，可以通过下列命令进行文件重定向</font>

```shell
./book_store <./data/book_sales >outfile  # book_store为可执行文件，会从./data/book_sales文件读取输入，并将输出写入outfile
```



#### 第二章：变量和基本类型（10.28）

C++ 是一种静态数据类型语言，它的类型检查发生在编译时

C++ 最重要的语法特征大概就是类了，其主要的一个设计目标就是让程序员自定义的数据类型像内置程序一样好用

C++ 基本数据类型包括

1. 算数类型：包含字符、整型数、布尔值和浮点数。它们分为两类：整型（字符、整型数、布尔值）和浮点型
2. 空类型

选择数据类型的建议：

1. 明确知晓数值非负，则选用无符号类型
2. 执行整数运算选 int
3. 执行浮点数运算选 double（double 和 float 相比，计算代价相差无几，甚至更快）

字面值常量，如：

```text
20  // 十进制整型字面值
024   // 八进制整型字面值
0x14   // 十六进制整型字面值
'a'  // 字符字面值
"Hello World!"  // 字符串字面值

可以指定字面值的类型，例如 u8"hi"，代表 utf-8 字符串字面值，utf-8 用 8 位编码一个 Unicode 字符
```

对象，指一块能存储数据并具有某种类型的内存空间

如果想声明一个变量而非定义它，就在变量名前加 `extern` ，同时不要显式初始化变量。记住，变量能且只能定义一次，但可以被多次声明

C++ 中大多数作用域都以花括号分隔，所有花括号之外则是全局作用域，花括号内为块作用域

引用即别名，它会和初始值绑定在一起，定义引用的方法为：

```c++
int a = 1024;
int &refa = a;  // 正确
int &refa = 10;  // 错误，初始值必须为对象
double &refa = a;  // 错误，引用类型的初始值需要和a一致
```

指针是指向另外一种类型的复合类型

```c++
// & 为取地址符
// * 为解引用符
int  dp = 1024;
int *dp2 = &dp;  // dp2指向一个int型的数
int **dp3 = &dp2;  // dp3指向一个int型的指针
double **dp3;  
int *p1 = nullptr;  // 生成空指针，新标准下建议用这种方法
int *p2 = 0;  // 生成空指针
int *p3 = NULL;  // 生成空指针，尽量避免使用NULL
```

同一条定义中，基本数据类型只能有一个，但是声明符的类型却可以不同

```c++
int i = 1024, *p = &i; &r = i;  // i是int型变量，p是int型指针，r是int型引用
```

有时候我们希望定义这么一种变量，它的值不能被改变，可以用 `const` 限定符

由于 const 对象一旦创建就不能更改，因此必须初始化。

如果想在多个文件共享 const 变量，则必须在变量定义前添加关键字 `extern`

```c++
extern const int bufSize;
```

指向常量的指针、顶层 const 、底层 const、constexpr 先跳过

类型别名可以让复杂的名字简单明了，易于使用和理解，有两种定义方法

1. 使用关键字 `typedef`

   ```c++
   typedef double wages;  // wages 是 double 的类型别名
   ```

2. 使用关键字 `using` （新标准规定的新方法）

   ```c++
   using SI = Sales_item;
   ```

编程常常需要把表达式的值赋给变量，要求声明变量的时候就需要知道其数据类型，有时候却做不到，为了解决这个问题，C++11 引入 `auto` 类型说明符，让编译器通过初始值推算便来那个类型

```c++
auto i = 0, *p = &i;  // 正确，i是整型，p是整型指针
auto sz = 0, pi = 3.14;  // 错误，一条声明语句只能有一个基本数据类型，此处 sz 和 pi 的类型不一致
```

delctype 先跳过

##### 自定义数据结构

第七章学习 class 之前，建议先用 struct，到时候会解释原因

很多新手常常忘记在类定义的最后加上分号

类通常定义在头文件，而且头文件名字与类名字相同。例如，库类型 string 在 string 的头文件中定义

头文件经常被多次包含，例如自定义的头文件使用 string 成员，自定义的源文件中操作了该成员，string.h 头文件先后被包含两次。确保头文件多次包含还能安全工作的常用技术是预处理器，例如预处理功能 `#include` 。这里我们要用到另一项预处理功能：头文件保护符。==由于头文件保护符很简单，即使没有被其他文件包含，也建议顺手写上。==

```c++
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```



#### 第三章：字符串、向量和数组（10.30）

除了第二章的内置类型外，C++ 还提供丰富的抽象数据类型库，例如 `string`、 `vector`、和迭代器

现代 C++ 程序应尽量使用 vector 和迭代器，避免使用内置数组和指针；应该尽量使用 string，避免使用 C 风格的基于数组的字符串

---

==`string` 和 `vector` 是两种最重要的标准库类型，迭代器是它们的配套类型==，常常用于访问 `string` 的字符和 `vector` 的元素

- `string` 对象是一个可变长度的字符序列
- `vector` 对象是一组同类型对象的容器
- 迭代器允许对容器的对象进行间接访问

内置数组是更基础的类型，`string` 和 `vector` 都是对它的某种抽象。一般来说，==应优先选用标准库类型==，之后再考虑数组等低层次替代品。

安全使用命名空间成员的方法：using 声明。但要注意，==头文件不应包含 using 声明！==因为这样有可能导致使用该头文件的某个程序发生始料未及的命名冲突

```c++
#include <iostream>
using std::cin;
using std::cout;
using std::endl;

#include <string>
using std::string;

#include <vector>
using std::vector;
```

##### string

```c++
// 定义和初始化string对象
string s1;  // s1是空字符串
string s2 = "hello";  // s2是该字符串字面值的副本，等价于s2("hello")。不过等号属于拷贝初始化，括号属于直接初始化
string s3 = s2;  // s3是s2的副本，等价于s3(s2)

// string的操作
string s;
s.empty();  // s为空返回true，否则返回false
s.size;  // 返回s中字符个数，注意，返回值不是int，而是string::size_type类型，它是一个无符号类型的值并且能存放任何string对象的大小，另一点需要注意的是由于它是无符号类型，因此不要和 int 类型的值相互运算，这种混用可能导致一些问题
s[n];  // 返回第n个字符的引用，不过n从0开始
s1 + s2;  // 连接它们
cin >> s;  // 将string对象读入s，会自动忽略开头的空白，从第一个真正的字符开始，直到遇到下一处空白，例如输入 "  Hello World"  则s结果为"Hello"
string line;
getline(cin, line);  // 保留输入的空白，只会在换行符结束

// 字面值和string相加，切记，字符串字面值和string是不同类型！！！
string s1;
string s2 = s1 + ",";  // 正确，string与字面值相加
string s3 = "hello" + ",";  // 错误，两个非string对象相加
string s4 = s1 + "hello" + ",";  // 正确
string s5 = "hello" + "," + s1;  // 错误，不能把字面值直接相加
```

使用下标时，必须确保其在合理范围内，C++标准并不要求检测下标是否合法，因此一旦超出范围，就会产生不可预知的后果

##### vector

由于 `vector` 容纳着其他对象，因此常被成为容器。

`vector` 最常用的方式：先定义一个空 vector，运行时利用 vector 的成员函数 push_back 向其中添加元素，push_back 负责把一个值当成 vector 对象的尾元素“压到” vector 对象的尾端。（能这么做的原因是：==vector 能在运行时高效快速地添加元素==）

```c++
// 虽然知道vector最后会包含100个元素，但仍然声明成空vector
vector<int> v1;
for (int i = 0; i != 100; ++i)
    v1.push_back();
```

vector 是一个类模板，编译器根据类模板创建类和函数的过程称为实例化，在类模板名字后面跟上尖括号来提供一些额外信息指定到底实例化成什么样的类。

##### 迭代器 iterator

下标运算符可以访问 string 对象的字符或 vector 对象的元素，迭代器也能实现。==所有标准库容器都能使用迭代器，但只有少数几种支持下标运算符==

```c++
// b表示v的第一个元素，e表示v最后一个元素的下一个位置，当v为空容器时，b和e都表示尾后迭代器
auto b = v.begin(), e = v.end();
```

迭代器的递增是往前移动一个位置

```c++
// 下列程序将第一个单词改成大写形式
for (auto it = s.begin(); it != s.end() && !isspace(*it); ++it)
    *it = toupper(*it);
```

<font color = red>vector 改变容量会使该 vector 对象的迭代器失效，因此使用了迭代器的循环体中不要向迭代器所属的容器添加对象</font>

==所有标准库容器的迭代器都定义了 `==` 和 `!=` ，但大多数都没定义 `<` 运算符==。不过 string 和 vector 的迭代器提供了一些额外的运算符，例如：

```
iter + n
iter - n
iter += n
iter -= n
iter1 - iter2
>   >=   <   <=
```

数组与 vector 类似，但大小不能改变，因此==不清楚元素具体个数请使用 vector==



#### 第四章：表达式（10.31）

C++ 提供==丰富的运算符==，并定义了==作用于内置类型==的运算对象时所执行的操作。

运算符作用于==类类型==的运算对象时，用户可自定义其含义，称之为==重载运算符==。

括号无视优先级和结合律。

优先级规定了运算对象的组合方式，但没有规定运算对象按什么顺序求值。

```c++
int i = 0;
cout << i << " " << ++i << endl;  // 该表达式是未定义的，因为编译器可能先输出 i，也可能先求 ++i 的值
```

算术运算符中

- 除法：整数相除，结果为整数，也就是直接切除小数部分。<font color = red>C++11 新标准规定，商一律向 0 取整</font>。

  ```c++
  int i;
  i = 21 / 6;  // 结果是 3
  i = 21 / -5;  // 结果是 -4
  i = -21 / -8; // 结果是 2
  ```

- 取余：运算符 `%` 称为取余或取模运算符，参与==取余运算的对象必须是整数类型==

<font color = red>逻辑与运算符、逻辑或运算符：遵循“短路求值”</font>，即先求左侧运算对象的值，再求右侧运算对象的值，当且仅当左侧运算对象无法确定表达式的结果时才会计算右侧运算对象的值。

```c++
string s("some string");
decltype(s.size()) index = 0;
if (index != s.size() && !isspace(s[index])  // 由于遵循短路求值，因此右侧运算对象是正确且安全的！否则index就超出合理范围了
{
    ......
}
```

除非确定比较的对象是布尔类型，否则不要使用布尔字面值 true 或 false 作为运算对象

```c++
if (val) {};  // val 为任意非零值，条件为真，不建议写成 if (val == true)，因为这相当于 if (val == 1)
if (!val) {};  // val 为0，条件为真
```

==赋值语句==有时需要放在条件中，由于其优先级较低，所以==通常要加括号==

```c++
// 这是烦琐且易错的写法
int i = get_value();  // 得到第一个值
while (i != 42) {
    ......
}3000000000000000

// 将赋值语句放在条件中，是更好的表达
int i;
while ((i = get_value()) != 42) {
    ......
}
```

复合赋值运算符：我们通常需要对对象施以某种运算，然后将计算结果再赋给该对象，这种操作很常见。==每种运算符都有相应的复合赋值形式==。这种区别对于程序性能只有些许影响，可忽略不计

```c++
+=   -=   *=   /=   %=  // 这些是算数运算符
<<=  >>=  &=   ^=   |=  // 位运算符
```

递增和递减运算符：`++` 和 `--` 不仅为对象的加减 1 提供简洁书写，并且它们<font color = red>可用于很多不支持算术运算的迭代器</font>！

<font color = red>建议养成使用前置版本 `++i` 的习惯，除非必须，否则不使用递增递减运算符的后置版本 `i++`。因为后置版本需要将原有值储存下来用于返回，对于复杂的迭代器类型，这种额外工作消耗很大 </font>

==有种写法非常普遍，需要理解其含义，即 `*p++`，这是一种被广泛使用的、有效的写法==。`*p++` 等价于 `*(p++)` ，运行过程为： `p++` 把 p 的值加 1，然后<font color = red>返回 p 的未加 1 的值进行解引用</font>。

```c++
// 使用 *p++ 的形式很简洁，也不易出错，如下
cout << *iter++ << endl;
// 不使用 *p++ 则更冗长易错
cout << *iter << endl;
++iter;
```

成员访问运算符：点运算符获取类对象的一个成员；箭头运算符与点运算符有关，表达式 `ptr->mem` 等价于 `(*ptr).mem`，这里加括号的原因是：<font color = red>解引用运算符的优先级低于点运算符</font>

```c++
string s1 = "a string", *p = &s1;
auto n = s1.size();  // 运行string对象的size成员函数
n = (*p).size();  // 运行p所指对象的size成员函数
n = p->size();  // 等价于(*p).size()
```

sizeof 运算符：返回一条表达式或一个类型名字所占的字节数，返回的值是 size_t 类型（size_t是一种机器相关的无符号类型，它被设计的足够大，以便能表示内存中任意对象的大小）的类型

关于 C++ 显式转换

- 含义：将对象强制转换成另一种类型。（虽然有时候不得不用，但这种方法本质是很危险的）

- 用法有点多，这里举一个简单例子

  ```c++
  // 任何具有明确定义的类型转换，只要不包含底层const，都可以使用static_cast
  // 将j进行强制类型转换，以便执行浮点数除法
  double slope = static_cast<double>(j) / i;
  ```

- 旧版 C++ 中，转换方法为 `type (expr)` 或 `(type) expr`，不是很清晰明了，转换出问题追踪起来也更困难



#### 第五章：语句（11.01）

复合语句：花括号括起来的（可能为空的）语句和声明的序列，也称作“块”。<font color = red>一个“块”就是一个作用域，块中引入的名字只能在块内部以及嵌套在块中的子块内访问</font>。块不以分号作为结束。

```c++
while (int i = get_num())
    cout << i << endl;
i = 0;  // 错误，循环外部无法访问 i
```

if 语句嵌套时，if 分支可能多于 else 分支。if 语句的 else，总是与离它最近的、尚未匹配的 if 匹配，从而消除二义性。如果要指定 else 与某个 if 匹配，则可以使用花括号，使其成为一个“块”。

switch 语句

- case 关键字和它对应的值一起被称为 case 标签
- <font color = red>case 标签必须是整型常量表达式</font>
- 某个 case标签匹配成功，则将执行后续所有 case 分支，直到遇到中断。建议每个 case 分支都要有 break 语句。
- 如果没有 case 标签匹配 switch 的表达式，则将执行 default 标签后面的语句

for 语句

- 传统 for 语句

  ```c++
  // 语法如下
  for (init-statement; condition; expression)
      statement;
  
  // 可以省略三部分中的任何一个，下面是两个示例
  // 示例1
  auto beg = v.begin();
  for (; beg != v.end() && *beg >= 0; ++ beg)
      ;  // 什么也不做
  // 示例2
  vector<int> v;
  for (int i; cin >> i;)
      v.push_back();
  ```

- <font color = red>C++11 新引入一种更简单的 for 语句，可以遍历容器或其他序列的所有元素</font>

  ```c++
  // 语法如下，其中 expression 表示的必须是一个序列，并且能返回迭代器的 begin 和 end 成员
  for (declaration : expression)
      statement;
  
  // 示例
  vector<int> v = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
  for (auto &r : v)  // 使用 auto 是为了确保类型相容，这是最简单的办法
      r *= 2;
  
  // 上述示例等价于下列传统 for 语句
  for (auto beg = v.begin(), end = v.end(); beg != end; ++beg) {
      auto &r = *beg;
      r *= 2;
  }
  ```

跳转语句

- break 语句终止离它最近的 while、do while、for、switch 语句，并从这些语句的下一句开始继续执行
- continue 语句终止最近循环的当前迭代并开始下一次迭代，用于 while、do while、for 语句
- ==goto 语句可以无条件跳转，但不要使用。==

C++ 中异常处理包括

1. throw 表达式，可以抛出一个异常，将控制权移交给相关的 catch 子句
2. try 语句块，用来处理异常。以关键字 try 开始，以一个或多个 catch 子句结束
3. 一套异常类，用于在 throw 和相关的 catch 子句间传递异常的具体信息

C++ 标准库定义了一组类，用于报告标准库函数遇到的问题，分别定义在 4 个头文件中，例如 <exception> 定义了最通用的异常类；<stdexcept> 定义了几种常用的异常类。



#### 第六章：函数（11.01）

<font color = red>函数是一个命名了的代码块。</font>

调用运算符：其实就是一对圆括号 `()`，它作用于一个表达式，该表达式是函数或指向函数的指针。

函数调用主要完成两项工作：用实参初始化函数对应的形参；将控制权转移给被调用的函数

return 语句结束函数，它也完成两项工作：返回 return 语句的值（如果有的话）；将控制权从被调函数转移回主调函数

```c++
// 关于函数的形参列表
void f1() {}                 // 隐式地定义空形参列表
void f2(void) {}             // 显式地定义空形参列表，这主要是为了和 C 语言兼容
int f3(int v1, int v2) {}    // 正确，形参用逗号隔开
int f3(int v1, v2) {}        // 错误，即使形参类型一样，也要把各自类型写出来
```

<font color = red>C++ 中，名字有作用域，对象有生命周期，理解这两个概念非常重要</font>

局部静态对象：在程序第一次经过对象定义语句时初始化，直到程序终止才销毁

```c++
size_t count_calls()
{
    static size_t ctr = 0;  // 调用结束后，值不被销毁
    return ++ctr;
}
int main()
{
    for (size_t i = 0; i != 10; ++i)
        cout << count_calls() << endl;
    return 0;
}
```

<font color = red>函数应该在头文件中声明，在源文件中定义。并且源文件应该将含有函数声明的头文件包含进来，以便编译器验证函数的定义和声明是否匹配。</font>

<font color = red>参数传递时，形参的类型决定形参和实参交互的方式。</font>

- 形参为引用类型，则它将绑到对应实参上。称==实参被引用传递，或函数被传引用调用==。<font color = red>熟悉 C 的程序员常常使用指针类型的形参访问外部对象，C++ 中建议使用引用类型的形参代替指针，并且尽量使用常量引用！（否则会带来一些问题）</font>
- 形参不为引用类型，则实参的值拷贝后赋给形参。称==实参被值传递，或函数被传值调用==

让函数返回多个值的技巧：一个函数只有一个返回值，但利用引用形参，可以实现返回多个值的效果

```c++
// 下列程序实现了既记录第一次出现字符 c 的位置，又记录 c 出现的次数
string::size_type find_char(const string &s, char c, string::size_type &occurs)
{
    auto ret = s.size();
    occurs = 0;
    for (decltype(ret) i = 0; i != s.size(); ++i) {
        if (s[i] == c) {
            if (ret == s.size())
                ret = i;
            ++occurs;
        }
    }
    return ret;
}
```

main 函数

```c++
// main 函数位于 prog 文件内
int main (int argc, char *argv[]) {...}
int main (int argc, char **argv) {...}   // 等价于上面的式子 

// 命令行执行 prog -d -o ofile data0
// 则第二个形参 argv 是一个数组，元素为指向 C 风格字符串的指针，具体内容如下；第二个形参 argc 表示数组中字符串的数量
argv[0] = "prog";  // 第一个元素指向程序名字或空字符串，接下来的元素依次传递命令行的实参
argv[1] = "-d";
argv[2] = "-o";
argv[3] = "ofile";
argv[4] = "data0";
argv[5] = "0";  // 最后一个指针之后的的元素值保证为 0
```

#### 前六章复习（11.06）

##### cin 和 getline

```c++
int main()
{
    string str;
    cout << "输入一个字符串，程序将剔除其中的标点符号：";
    getline(cin, str);                               // 如果允许输入的字符串有空格、换行符等，则使用getline
    // cin >> str;                                   // 如果不允许输入的字符串有空格、换行符等，则使用cin
    for (auto c : str)
    {
        if (!ispunct(c))
        {
            cout << c;
        }
    }
    cout << endl;
    return 0;
}
```



##### 为什么使用 `mid = beg + (end - beg)/2` 而不用 `mid = (beg + end)/2`

因为 C++ 没有定义两个迭代器的加法运算，只定义了减法运算

##### begin 和 cbegin

`begin()` returns an iterator to beginning while `cbegin()` returns a const_iterator to beginning，区别在于是否允许修改它指向对象的值

```c++
// 下面的程序有个 bug，当输入数量为偶数时，it != vInt.cend() - 1 恒成立，从而死循环，输出一大堆东西，然后 Segmentation fault (core dumped)。虽然一般 C++ 程序员更喜欢 != 的风格进行判断（因为所有容器都支持 != 只有部分容器支持 <），但这里也不得不改成 <=

int main()
{
    vector<int> vInt;
    int iVal;
    cout << "请输入一组数字：" << endl;
    while (cin >> iVal)
    {
        vInt.push_back(iVal);
    }
    if (vInt.cbegin() == vInt.cend())
    {
        cout << "没输入任何数字" << endl;
        return -1;
    }
    cout << "相邻两项的和依次是：" << endl;
    for (auto it = vInt.cbegin(); it != vInt.cend() - 1; it++)  // 此处用 != 无法处理输入为偶数的情况，可以改成 <=
    {
        cout << (*it + *(++it)) << " ";
        if ((it - vInt.cbegin() + 1) % 10 == 0)
        {
            cout << endl;
        }
    }
    if (vInt.size() % 2 != 0)
    {
        cout << *(vInt.cend() - 1) << endl;
    }
    return 0;
}
```



##### 相较于使用指针，==使用引用从形式上更简单，并且无需额外声明指针变量==

```c++
// -----------------------指针pointer的方法-----------------------
void mySwap(int *p, int *q)
{
    int tem = *p;
    *p = *q;        // 需要额外声明指针变量
    *q = tem;
}

int main()
{
    int a = 123, b = 789;
    int *p = &a, *q = &b;
    cout << "交换前：a = " << a << ", b = " << b << endl;
    mySwap(p, q);
    cout << "交换后：a = " << a << ", b = " << b << endl;
    return 0;
}


// -----------------------引用reference的方法-----------------------
void mySwap(int &i, int &j)
{
    int tem = i;
    i = j;
    j = tem;
}

int main()
{
    int a = 123, b = 789;
    cout << "交换前：a = " << a << ", b = " << b << endl;
    mySwap(a, b);
    cout << "交换后：a = " << a << ", b = " << b << endl;
}
```

##### 与值传递相比，引用传递有三点优势

1. 能直接操作引用形参的对象
2. 可以避免拷贝大的类类型和容器类型对象
3. ==帮我们从函数中返回多个值==

```c++
// 下面的函数容易犯错，两个地方都要用到引用传递，有一个不对，则无法对原字符串进行修改

/*
* 函数功能：将所有大写字母转换成小写
*/
void changeToLower(string& str)   // 此处采用引用传递，能直接修改原字符串
{
    for (auto &c : str)           // 注意！此处也需要采用引用传递，否则不会对原字符串进行修改
    {
        c = tolower(c);
    }
}
```

##### 使用 main 函数的 argc 和 argv 参数

```c++
// 下面的函数能将输入的参数连接起来，例如命令行运行：./out a b asdf kl，则结果为：abasdfkl。如果for循环是 i = 0 开始，则输出结果为./outabasdfkl

int main(int argc, char **argv)
{
    string str;
    for (int i = 1; i != argc; ++i)
    {
        str += argv[i];
    }
    cout << str << endl;
    return 0;
}
```









































