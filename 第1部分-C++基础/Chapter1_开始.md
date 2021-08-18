# 1 开始

**本章通过解决一个简单问题，来介绍C++大部分的基础内容：类型、变量、表达式、语句及函数。**

**书店问题**

书店电脑里保存着所有的销售记录，每一条记录都有某本书的ISBN号，售出的册数以及书的单价信息，编写一个C++程序，能够计算每本书的销售量、销售额及平均售价

## 1.1 main函数

每个C++程序都包括一个或多个函数，其中一个必需命名为**main**，操作系统通过调用main来运行C++程序，虽然main函数在某种程度上比较特殊，但其定义和其他函数是一样的，在大多数操作系统中，main的返回值被用来指示状态，返回值为0表示成功，非0的返回值含义由系统定义

```c++
int main()
{
    rerurn 0;
}
```

## 1.2 iostream库

C++语言并没有定义任何输入输出（IO）语句，取而代之，包含了一个全面的标准库（standard library）来提供IO机制

**iostream库**包含两个基础类型istream和ostream，分别表示输入流和输出流

标准库定了了4个IO对象：

| 对象 | cin      | cout     | cerr     | clog           |
| ---- | -------- | :------- | -------- | -------------- |
| 类型 | istream  | ostream  | ostream  | ostream        |
| 作用 | 标准输入 | 标准输出 | 标准错误 | 输出一般性信息 |

系统通常将程序所运行的窗口与这些对象关联起来，因此，当我们读取cin，数据将从程序正在运行的窗口读入，当我们向cout、cerr和clog写入数据时，将会写到同一个窗口

```c++
#include <iostream>
int main()
{
	std::cout << "enter two numbers:" << std::endl;
	int v1 = 0, v2 = 0;
	std::cin >> v1 >> v2;
	std::cout << "the sum of " << v1 << " and " << v2
		<< " is " << v1 + v2 << std::endl;
	return 0;
}
```

**std**通过作用域运算符**::**，指出名字cout、cin和endl是定义在名为std的命名空间中的，命名空间可以帮助我们避免不经意的名字定义冲突以及使用库中相同名字导致的冲突，标准库定义的所有名字都在命名空间std中

**endl**是被称为操纵符的特殊值，写入endl的效果是结束当前行，并将与设备关联的缓冲区中的内容刷到设备中，缓冲刷新操作可以保证到目前为止程序所产生的所有输出都真正写入输出流中，而不是仅停留在内存中等待写入流

**<<**和**>>**是输出/输入运算符，两个运算符作用类似，这里只介绍<<运算符，它接受两个运算对象，左侧必须是一个ostream对象，右侧的运算对象是要打印的值，此运算符将给定的值写到给定的ostream对象中，输出符的计算结果就是左侧运算对象，也就是我们写入给定值的那个ostream对象，例如在上个示例第4行就等同于

```c++
(std::cout << "enter two numbers:") << std::endl;
//与下面两条语句执行结果一样
std::cout << "enter two numbers:"；
std::endl;
```

同理>>运算符，上个示例第6行就等同于

```c++
(std::cin >> v1) >> v2;
//与下面两条语句执行结果一样
std::cin >> v1;
std::cin >> v2;
```

## 1.3 控制流语句

语句一般是顺序执行，但程序设计语言提供了多种不同的**控制流语句**，允许我们写出更为复杂的执行路径

用**while语句**写1到10的求和程序

```c++
#include <iostream>
int main()
{
	int i = 1,sum = 0;
	while (i <= 10)
	{
		sum += i;//复合赋值运算符，等价于sum = sum + i;
		++i;//前缀递增运算符，等价于i = i + 1;
	}
	std::cout << sum << std::endl;
	return 0;
}
```

用**for语句**重写该程序

```c++
#include <iostream>
int main()
{
	int sum = 0;
	for (int i = 1; i <= 10; ++i)
	{
		sum += i;
	}
	std::cout << sum << std::endl;
	return 0;
}
```

读取数量不定的输入数据，遇到**文件结束符**（Windows下是Ctrl+Z，然后回车）或是无效输入，istream对象的状态会变为无效，处于无效状态的istream对象会使得条件为假

```c++
#include <iostream>
int main()
{
	int sum = 0,i = 0;
	while (std::cin >> i)
	{
		sum += i;
	}
	std::cout << sum << std::endl;
	return 0;
}
```

用**if语句**写一个能够统计在输入中每个值连续出现了多少次的程序

```c++
#include <iostream>
int main()
{
	int val = 0,currval = 0;
	if (std::cin >> currval)
	{
		int count = 1;
		while (std::cin >> val)
		{
			if (val == currval)
			{
				++count;
			}
			else
			{
				std::cout << currval << " occurs " <<
					count << " times" << std::endl;
				currval = val;
				count = 1;
			}
		}
		std::cout << currval << " occurs " <<
			count << " times" << std::endl;
	}
	return 0;
}
```

## 1.4 类简介

在C++中，我们通过定义一个**类**（class）来定义自己的数据结构，一个类定义了一个类型，以及与其关联的一组操作，类机制是C++最重要的特性之一，实际上，C++最初的一个设计焦点就是能定义使用上像内置类型一样自然的**类类型**（class type）

对于本节开头的书店问题，我们假定类名为Sales_items，头文件Sales_items.h中已经定义了这个类，习惯上，头文件根据其中定义的类的名字来命名

每个类实际上都定义了一个新的类型，其类型名就是类名，因此我们的Sales_items类定义了一个名为Sales_items的类型，与内置类型一样，我们可以定义类类型的变量

```c++
Sales_items item;//item是一个Sales_items类型的对象
```

**成员函数**是定义为类的一部分的函数，有时也被称为**方法**，我们通常以一个类对象的名义来调用成员函数

```c++
item.isbn();
```

点运算符**.**只能用于类类型的对象，其左侧必须是一个类类型的对象，右侧运算对象必须是该类型的一个成员名，运算结果为右侧运算对象指定的成员

