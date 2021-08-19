**本章将分别介绍数组以及标准库类型string和vector**

## 3.1 命名空间的using声明

有了using声明就无须专门的前缀（形如命名空间::）也能使用所需的名字了，using声明具有如下的形式：

```c++
using namespace::name;
```

每个名字都需要独立的using声明

```c++
#include<iostream>
using std::cin;
using std::cout;
using std::endl;
int main()
{
    int num = 0;
    cin >> num;
    cout << "hello " << num << endl;
    return 0;
}
```

位于头文件的代码一般来说不应该使用using声明，这是因为头文件的内容会被拷贝到所有引用它的文件中去，如果头文件里有某个using声明，那么每个使用了该头文件的文件就都会有这个声明，对于某些程序来说，由于某个不经意间包含了一些名字，反而可能产生始料未及的名字冲突

## 3.2 标准库类型string

使用string类型必需首先包含string头文件，作为标准库的一部分，string定义在命名空间std中

定义和初始化string对象，常用的方式：

```c++
string s1;//默认初始化，是一个空字符串
string s2 = s1;//s2是s1的副本
string s3 = "hiya";//s3是该字符串字面值的副本
string s4(10,'c');//s4的内容是cccccccccc
```

下表列举string对象上的大部分操作

| os<<s         |      |
| ------------- | ---- |
| is>>s         |      |
| getline(is,a) |      |
| s.empty()     |      |
| s.size()      |      |
| s[n]          |      |
| s1+s2         |      |
| s1=s2         |      |
| s1==s2        |      |
| s1!=s2        |      |
| <=,>=,<,>     |      |

size函数返回的是string::size_type类型的值，在具体使用的时候通过作用域操作符来表明名字size_type是在类sting中定义的，它是一个无符号类型的值而且足够存放下任何string对象的大小，假设n是一个具有负值的int，则表达式s.size()<n的判断结果一定是true，这是因为负值n会自动转换为一个比较大的无符号值，所以如果一条表达式中已经有了size()函数就不要再使用int了，这样可以避免混用int和unsigned可能带来的问题

处理sting对象中的字符需要用到cctype头文件中定义的一组标准库函数处理这部分工作，下表列出部分主要函数名及其含义

| isalnum(c) |      |
| ---------- | ---- |
| isalpha(c) |      |
| iscntrl(c) |      |
|            |      |
|            |      |
|            |      |
|            |      |

处理每个字符可以使用**基于范围的for语句**（范围for语句），这种语句遍历给定序列中的每个元素并对序列中的每个值执行某种操作

通过下标运算符（[ ]）来处理一部分字符，接收的参数是string::size_type类型，这个参数表示要访问的字符的位置，返回值是该位置上字符的引用

## 3.3 标准库类型vector

标准库类型vector表示对象的集合，其中所有对象的类型都相同，集合中的每个对象都有一个与之对应的索引，索引用于访问对象，vector也被称为容器，要想使用vector，必须包含适当的头文件

```c++
#include<vector>
using std::vector;
```

C++语言既有类模板也有函数模板，vector是一个类模板，模板本身不是类或函数，编译器根据模板创造类或函数的过程被称为实例化，当使用模板时，需要指出编译器应把类或函数实例化成何种类型

```c++
vector<int> ivec;//ivec保存int类型对象
vector<vector<string>> file;//该向量的元素是vector对象
```

使用列表初始化vector对象或是创建指定数量的元素

```c++
vector<int> ivec;//ivec初始状态为空
vector<int> ivec1{1,2,3,4};//列表初始化
vector<int> ivec2(10,1);//10个int类型的元素，每个被初始化为1
vector<int> ivec3(10);//10个int类型的元素，值被默认初始化为0
```

使用vector的成员函数**push_back**向其中添加元素，压到（push）vector对象的尾端（back），如果循环体内部包含向vector添加元素的语句，则不能使用范围for循环

其他vector操作大多数与string类似，可以使用下标运算符来访问vector对象的元素

## 3.4 迭代器介绍

如果容器为空，则begin和end返回的是同一个迭代器，都是尾后迭代器

所有标准库容器都可以使用迭代器，但是其中只有少数几种才同时支持下标运算，严格来说，string对象不属于容器，但是string支持很多与容器类型类似的操作

## 3.5 数组



## 3.6 多维数组

