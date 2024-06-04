## C++笔记
#### Markdown基本操作
**加粗**
*斜体*
***斜体加粗***
公式的插入同Latex
在行中插入`code`
下方是代码块

```C++
# include <iostream>

using namespace std;

int main()
{
	int num = 0; 
	double year = 5e3;  //科学计数法
	cin >> num;         //先输入num,再输入year
	cout << "re" << num + year << '\n' << endl;
	int &name = num;    //引用
	int *p = nullptr;   //生成空指针
	name = 6;
	cout << num;
	return 0;
}
```

#### 基本概念
20 十进制   024 八进制  0x14 十六进制
科学计数法6.5e3
**声明**：让程序“知道”有这个名字
**定义**：“创建”与名字相关的实体，任何包含显式初始化的声明就是定义，结构和类只有声明

```C++
extern int i;  //只有声明，没有定义
int i;  //有声明也有定义
extern int i = 6; //有声明也有定义，但会引发报错
extern const int bufsize;  // 声明
```

**命名规范**：变量名小写，类名开头字母大写
**变量定义**：建议第一次使用时定义
**引用**：引用即“别名” ，与对象进行绑定
`&value1 = value2` 定义了value1，它是value2的引用。修改value1就是修改value2
void* 指针  可以用于储存指向任何类型的地址，常用于进行比较
**const**：定义变量时就要赋值，一旦定义便不能修改。
const 变量只能在定义的文件内使用，可以通过`extern`前缀解决，要求是**不管是定义还是声明**都要加`extern`。
const 变量的引用也只能是const类型的变量。

```C++
const int c1 = 100;
const int &r1 = c1;  // right
int &r2 = c1;  // false
```

***typedef***
给类型定义别名

```C++
typedef double wages;    //wages,base是double
typedef wages base, *p;  //p是double*
```
***auto***
在初始化的同时让系统自动辨识变量是什么类型，要求一行语句内类型一致。
```C++
auto i = 0, *p = &i;  // right
auto i = 0, pi = 3.14;  // false
```
***decltype***
用某个式子的返回值作为变量类型

```C++
decltype(f())  sum = x;  // sum 的类型是f()的返回值
```

**头文件**
"xx.h" 首先在当前文件夹里找，然后再到其他地方找
$\text{<xx.h>/<xx>}$ 在系统文件里找
头文件编写示例

```C++
#ifndef _LED_H_
#define _LED_H_

//  头文件内容

#endif
```

#### 字符串、向量、数组
要使用string类型，需要包含string头文件

```C++
#include <string>
using std :: string;
```
初始化方式
```C++
string s1 = "read";  // 拷贝初始化
string s2 = s1;  // 拷贝初始化
string s3(s1);  // 直接初始化
string s4(5, 'a');  // 直接初始化，内容是aaaaa
```
string类型的读取会以空格、换行符、制表符终止
```C++
string s1, s2;  // 若输入“   read  note  ”
cin >> s1 >> s2;  // s1为“read”,s2为"note"
cout << s1 << s2 << endl;
```
*getline()* 进行读取，换行符终止
*empty()* 判定是否为空，若空返回1，否则返回0
*size()* 字符串中字符的长度,其返回值类型是string::size_type，是无符号的值。
attention:若将size()与一个负数进行比较，那么这个负数会先转化为无符号类型的值，则size()会始终小于这个负数。
*+* 字符串相加就是收尾相接。
```C++
string s1;
getline(cin, s1);  // 将cin内的内容独到s1中,且s1中无换行符
if( ! s1.empty())
{
	auto length = s1.size();
	cout << s1 << endl;
}
```
循环遍历字符串中的元素
```C++
string str("Read Book.");
for(auto c : str)
{
	cout << c << endl;
}
for(auto &c : str)  // c是引用
{
	c = toupper(c);  // 将字符改为大写
}
cout << str << endl;
```
下标操作法，只要字符不是常量（const）就可以进行更改。
```C++
string str("read book.");
if(!s.empty())
{
	s[0] = toupper(s[0]);
}
for(decltype(s.size()) index = 0; index != s.size(); index++)
{
	s[index] = toupper(s[index]);
}
```

看到pdf阅读器112


