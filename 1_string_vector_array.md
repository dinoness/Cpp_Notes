#### string
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
*s.getline()* 进行读取，换行符终止。  
*s.empty()* 判定是否为空，若空返回1，否则返回0。  
*s.size()* 字符串中字符的长度,其返回值类型是string::size_type，是无符号的值。  
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

#### vector
同种类型对象的集合，每个对象都有对应的索引。  
```C++
#include <vector>
using std :: vector;

vector<int> v1 = {1, 2, 3, 4};  // v1向量的元素是int类型
vector<int> v2 = v1, v3(v1);  // v2,v3是v1的副本
vector<string> v6;  // 默认初始化，无元素
vector<int> v7 (3, 8);  // {8, 8, 8}
vector<int> v8 (3, 0);  // 初始化的值为0，string时是空字符串

v1.push_back(6);  // 把6加到v1的末端
```
**note**:用循环语句向vector添加元素时，应当使用while而不是for,因为for语句内不应改变其所遍历序列的大小。
*v.push_back(待添加元素)*  不能用下表的形式添加元素，因为只能对已经存在的元素执行下表操作    
*v.empty()*  
*v.size()* 返回元素个数，返回值的类型是vector<int>::size_type.(是具体的vector内容类型)  
*v[n]*  

**迭代器**：类似于指针
```C++
string s("some");
if (s.begin() != s.end() )  // 确保非空
{
    for(auto it = s.begin(); it != s.end(); ++it);  // ++it 是令it指向下一个元素
    *it = toupper(*it);  // *it 是获取迭代器所指向的元素
}
```
**note**：for语句中的终止条件使用！=是因为这个符号基本可以覆盖所有容器类型  
*iter + n* 迭代器向下游移动n次  
*inte1 - inter2* 迭代器之间的距离，类型是difference_type，带符号的整形  
*>, >=* 比较迭代器的位置  

#### array
在定义的时候要求数组的长度是已知且固定的  
数组不能引用，不能直接拷贝  
```C++
int *ptrs[10];  // 修饰符绑定从右向左，ptrs是存有10个整形指针的数组
int (*Parray)[10] = &arr;  // 先看括号再从右向左，Parray是指针，指向整形数组
int (&arrRef)[10] = arr;  // arrRef是引用，引用整形数组
```
数组下表的类型通常定义为size_t  
指针也可以当做迭代器，++p让指针指向下一个元素  
```C++
#include <iterator>
int arr[10];

int *p = arr;  // p是arr首地址
int *beg = begin(arr);  // 获得首地址

int *e = &arr[10];  // 指向arr尾元素的下一位置
int *last = end(arr);  // 指向arr尾元素的下一位置

int k = p[3];  //k是arr中第四个元素，即arr[3]

for(int *b = arr; b != e; ++b)
{
    cout << *b << endl;
}
```
C风格字符串：存放在字符数组中并以'\0'结束  
需要考虑数组溢出的问题  
```C++
#include <cstring>

strlen(p);  // 返回字符串长度，不计算空字符
strcpm(p1, p2);  // p1==p2,return0   p1>p2,返回正值    p1<p2,返回负值
strcat(p1, p2);  // p2加到p1尾部
strcpy(p1, p2);  // p2拷贝到p1
```
数组初始化vector  
```C++
int arr[] = {0, 1, 2, 3};
vector<int> ivec(begin(arr), end(arr));  // ivec = {0, 1, 2, 3}
```
