### IO calss
##### I0对象不能拷贝或赋值
不能将其设置为形参或返回类型  
##### 刷新输出缓冲区
```C++
cout << "hi" << endl;  // 输出hi和换行符，然后刷新缓冲区
cout << "hi" << flush;  // 输出hi，然后刷新缓冲区，无额外字符
cout << "hi" << ends;  // 输出hi和空字符，然后刷新缓冲区
```
#### 文件输入输出
##### 基本操作
```C++
ifstream f_in(ifile);  // 构造一个ifstream并打开指定文件
ofstream f_out;  // 构造一个ofstream，不打开文件
f_out.open(ifile + ".copy");  // 打开指定文件
if(f_out)  // 判定是否成功打开文件
f_in.colse();  // 关闭打开的文件，才能用这个对象打开其他文件
```
##### 文件模式
in：读  
out：写  
app：每次写操作前都定位到文件末尾  
ate：打开文件后立刻定位到文件末尾  
trunc：截断文件(C++下是清除文件内容)  
binary：二进制  
  
以out模式打开文件会丢弃已有的数据，可以使用app形式阻止此情况  
```C++
// 以下是在声明的时候就打开文件
ofstream out("file1");  // 默认out模式并截断
ofstream out2("file1", ofstream::out);  // 隐含截断
ofstream out3("file1", ofstream::out | ofstream::trunc);
ofstream out4("file1", ofstream::app);  // 隐含输出模式，但是不截断
```
每次调用open时都会确定文件模式，默认是输出和截断。  
`out.open("file1");`  

#### string流
`>>`,`<<`符号的两端应该是string类型和流类型，不能同时是string  
```C++
#include <iostream>
#include <string>
#include <sstream>

using namespace std;

int main()
{
	string line, word;
	getline(cin, line);
	istringstream record(line);
	while (record >> word)  // std::istringstrea >> std::string 
	{
		cout << word << endl;;
	}
    // line >> word;  //会报错， std::string >> std::string 
	return 0;
}

```


