### operator
int类型除法(/)一律向下取整  
取模运算(%)只能由整数相除  

#### 成员访问运算符
ptr->member  等价于  (*ptr).member，用于获取对象的一个成员
```C++
string s1 = "a string", *p = &s1;
auto n = s1.size();  // 运行s1的size成员
n = (*p).size();  // 运行p所指对象的size成员
n = p -> size();  // 等同于上一句
*p.size();  // error,这是先运行P的size成员，再解引用
```

#### 位运算符
位运算最好用于无符号类型  
运算对象会由“小整形”提升为整数类型  
位移运算：将对象转为二进制的整数类型(32位)，在进行位移，出界的部分丢掉，缺失的部分用0补齐  
IO运算符满足做结合律，其优先级比算数运算符低，比关系/幅值/条件运算法高  

#### 强制类型转换
*cast-name<type>(expression);*  
cast-name : `static_cast   dynamic_cast   const_cast   reinterpret_cast`  
type : 转换的目标类型  
exprssion : 待转换对象  

### statement
**switch语句**
执行流：遇到目标选项，就一直执行后面的语句，包括该选项之后的，其他选项内的语句。因此需要编写合适的break。  
default语句，当没有任何状况时的选项。  

**范围for语句**
常用于遍历容器或序列  
```C++
for(declaration : expression)
    statement
```
expression是一个序列(vector,string,数组，列表等)  
declaration定义一个变量，让序列中所有元素都能转换为该变量，如果需要对序列中的元素进行读写操作，就要声明成引用类型  
```C++
vector<int> v = {0, 1, 2, 3, 4};
for (auto &r :v)
{
    r *= 2;
}
```

**break语句**
终止离它最近的**while,do while, for switch** 语句，并从第一条语句开始执行。

**continue语句**
continue只能用在**for,while,do while**内部。  

**异常处理语句**
*throw* 格式如下：`throw runtime_error("Data must refer to same ISNB")`  
要求导入runtime_error标准库  
*try语句块* 
```C++
try{
    program-statements  // 某执行语句，如果失败产生一个异常信号
} catch (exception-declaration){  // 跳转到响应的异常声明，并执行对应的处理语句
    handler-statments
} catch (exception-declaration){
    handler-statments
}
```

### function
局部静态变量：加上static关键词前缀，这一变量不会随着函数的结束而释放。  
#### 引用参数
```C++
void reset1(int i);
void reset2(int &i);
int main(void)
{
    int n = 10, &k = n, j = n;
    reset1(j);  // 此操作对n,i,j都没有影响
    reset2(k);  // 使用引用后，传入函数内部的变量也与n和k绑定，所以n,k归0
}

void reset1(int i){i = 0;}  // 形参是实参的拷贝
void reset2(int &i){i = 0;}  // 形参是实参的引用，对于实参空间较大时，引用更快速
```
引用形参可以使得函数具有多个返回值。  
当使用引用形参而不对其进行操作时，可以添加cosnt前缀  

#### 数组形参
本质是输入指针，下方三行语句等价，且是底层形参，即指针的值可以改变，指针指向对象的值不能变。  
```C++
void print(const int* a);
void print(const int[]);
void print(const int[10]);
```
防止数组下标越界的方法  
1.传递首指针p后，每次操作前判定*p是不是数组的末尾元素。  
2.传递首指针p_beg和尾指针p_end。  
3.额外传递表示数组大小的形参。  

**数组引用形参**
```C++
void print(int (&arr)[10])
{
    for (auto elem : arr)
    cout << elem << endl;
}
```
上方函数中，括号必不可少，传入的参数强制要求是10个整形的参数，类型和个数都要正确  

#### 返回值
不要返回**局部对象**的引用或指针  
当函数的返回类型是引用时，其是左值，可以被赋值  
```C++
// 返回的是对str[ix]的引用
char &get_val(string &str, string::size_type ix)
{
    return str[ix];
}

int main(void)
{
    string s("a value");
    cout << s << endl;
    get_val(s, 0) = 'A';  // 将s[0]改为A
    cout << s << endl;
    return 0;
}
```
**返回函数**——递归
**返回数组指针**
1.类型别名
```C++
typedef int arrT[10];
using arrT = int[10];  // 等价上一条语句
arrT* func(int i);
```
2.直接声明  
形式`Type (*function(parameter_list))[dimensino]`  
例子`int (*func(int i))[10];`  
解释  
`func(int i)` 调用func需要int类型形参  
`*func(int i)` 可以对函数调用的返回值进行解引用  
`(*func(int i)[10])` 解引用func的结果得到的是有10个元素的数组  
`int (*func(int i)[10])` 表示数组中的元素类型是int  
3.尾置返回类型
`auto func(int i) -> int(*)[10];`
4.使用decltype
```C++
int a[3] = {0, 2, 6};
decltype(a) *fun (int i);
```
解释  
declype(a)的结果是数组，*使得返回结果是数组指针  

#### 函数重载
同一作用域内函数名字相同但形参列表不同  
```C++
void print(int num);
void print(char* strdata);
```

#### 默认实参
声明：`string screen(int ht = 24, int wid = 80, char backgrnd = 'window');`  
调用：  
`screen();`  
`screen(, , 'new');`  

#### 内联函数
`inline int fun(int a);`  作用是减小调用该函数是产生的开销。  
一般用于调用规模较小的函数，而且部分编译器不支持。  

#### constexpr函数
表示能用于常量表达式的函数

#### 函数指针
```C++
int fun(int a);  // 声明了返回int类型的函数
int *fun(int a);  // 声明了返回int*类型的函数
int (*fun)(int a);  // 声明了返回int类型函数的指针
typedef int (*pc)(int)  // 声明了该函数指针的类别名，为pc
pc pfun1 = fun;  // 定义了函数指针pfun1，指向fun
decltype(fun) *pfun2;  // decltype(fun)代表函数，需要用*将其转化为指针类型
decltype(fun) *pfun3(int a);  //声明了返回函数指针的函数
```
函数的名字就是一个地址  
```C++
int read(int a)
{
    return a++;
}
int (*pfun)(int) = read;  // 创建指向read函数的指针pfun
pfun = read;  // 可以直接用函数名对指针赋值
(*pfun)(int 6);  // 调用指针指向的函数，就是进行解引用
```
函数指针使用思路：可以将函数指针作为返回值，实现选择函数形参相同，实现功能不同的函数的功能。  
用一个函数指针来接收该返回值，即可进行函数调用。
