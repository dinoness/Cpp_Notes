### Class
类是一种特殊的数据结构  
#### 类的声明
类内部的命名可以与结构体外部的命名重合，因为两者作用域不同。  
类的成员函数的声明必须在类的内部，定义可以在类的内部或外部，而作为借口的非成员函数，则要定义和声明在类的外部。 
```C++
struct Sales_data{
    // 操作函数
    std :: string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    // 数据成员
    std :: string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
// 非成员的接口函数
Sales_data add(const Sales_data&, const Sales_data&);
std :: ostream &print(std :: ostream&, const Sales_data&);
std :: istream &read(std :: istream&, Sales_data&);

double Sales_data :: avg_price() const 
{
    if (units_sold)
    {
        return revenue/units_sold;
    }
    else
    {
        return 0;
    }
}
// 返回this对象，this是调用该函数的结构体的指针
Sales_data& Sales_data :: combine(const Sales_data &rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

```
当调用成员函数时，会引入**this**隐式参数来调用对象的成员，**this**存的是结构体的地址。因此要让**this**指向bookNo常量，就要将其转化为底层const，所以在函数后面加上const。因此只有结构体中的常量才能调用常量成员函数。  
**::** 作用域运算符，`Sales_data :: avg_prcie()`表示只在Sales_data这个类中寻找avg_price函数。  

**非成员函数的定义**  
```C++
istream &read(istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;  // 从数据流中将数据独到对象里
    item.revenue = price * item.units_sold;
    return is;
}

ostream &print(ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " "
       << item.revenue << " " << item.avg_princ();
    return os;
}
// 不添加引用符，说明生成的是新的Sales_data
Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;  // 拷贝出一个新的结构体
    sum.combine(rhs);
    return sum;
}
```

#### 类的初始化
**构造函数**：用于初始化类的数据成员，构造函数的名字与类的名字相同。一个类可以有多个构造函数，运作模式与重装函数相同。  
默认构造函数：系统默认对各个数据成员赋值  
含有构造函数的类：  
```C++
struct Sales_data{
    // 构造函数
    Sales_data() = defalut;  // 默认构造函数
    Sales_data(cosnt std::string &s) : bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p) :
                bookNo(s), units_sold(n), revenue(p*n) { }
    Sales_data(std::istream &);
    //委托构造函数，对其他构造函数进行调用的构造函数
    Sales_data(std::stream s) : Sales_data(s, 0, 0) { }
    // 操作函数
    std :: string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    // 数据成员
    std :: string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
// 在类的外部的构造函数
Sales_data::Sales_data(std::istream &)
{
    read(is, *this);
}
```
`Sales_data(cosnt std::string &s) : bookNo(s) { }`
从函数的形参输入参数，由冒号和花括号之间的参数表给结构体赋初值，未涉及的参数按照默认初始化。  
如果成员是const或引用，必须将其初始化。  
最好令构造函数的初始值顺序与成员的声明顺序一致，尽量避免应某些成员初始化其他成员。  


#### 类的拷贝和赋值
类的拷贝可以直接用类的名字进行，如上文中的add函数  
但是涉及动态内存的类不能进行整体操作  

#### 类的封装
```C++
class Sales_data{
// 声明友元
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::ostream &print(std::ostream&, const Sales_data&);
friend std::istream &read(std::istream&, Sales_data&);

public:
    // 构造函数
    Sales_data() = defalut;  // 默认构造函数
    Sales_data(cosnt std::string &s) : bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p) :
                bookNo(s), units_sold(n), revenue(p*n) { }
    Sales_data(std::istream &);
    // 操作函数
    std :: string isbn() const {return bookNo;}
    Sales_data &combine(const Sales_data&);

private:
    double avg_price() const;
    // 数据成员
    std :: string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);
```
将类型关键词由struct替换为class，区别在于strcut默认下所有成员都是public，而class默认下所有成员都是private。  
**public**说明成员可以被其他程序访问  
**private**说明成员可以被类的成员函数访问，但是不能被使用该类的代码访问  
为了让非成员的接口函数可以调用非公有(private)成员，需要在类的内部声明这些函数，并添加*friend*前缀，称为**友元**，在类的外部依旧需要进行声明。 
如果其他类需要访问非公有成员，则可以对类进行友元声明，如 `friend 类的名称;`。且友元关系不具有传递性。也可以只对其他类中的某个函数申明友元`friend class_a::fun1(参数表);`



```C++
class Screen{
public:
    typedef std::string::size_type pos;  // 定义一种类型
    Screen() = default;  // 默认初始化
    Screen(pos ht, pos wd, char c) : height(ht), width(wd), 
        contents(ht * wd, c)  { }  // 构造函数
    char get() const { return contents[cursor]; }  // 获取光标位置
    inline get(pos ht, pos wd) const;  // 获取窗口尺寸
    Screen &move(pos r, pos c);  // 移动位置
    Screen &set(char);  // 改变光标所在位置字符
    Screen &set(pos, pos, char);  // 移动位置并改变字符
    Screen &display(std::ostream &os)  // 当对象是非常量时调用该函数
            {do_display(os); return *this;} 
    const Screen &display(std::ostream &os)  // 当对象是常量(const Screen)时调用该函数
            {do_display(os); return *this;}

private:
    pos cursor = 0;  // 光标位置
    pos height = 0, width = 0;  // 尺寸
    std::string contents;  // 内容字符串

    // 当display调用时，此时通过常量指针进行操作，而在display中的this任然是一个非常量指针,进而实现功能函数的连续调用
    void do_display(std::ostream &os) const {os << contents;}
}

inline Screen &Screen::move(pos r, pos c)  // 在外部定义为inline
{
    pos row = r * width;
    cursor = row + c;
    return *this;
}

char Screen::get(pos r, pos c) const
{
    pos row = r * width;
    return contents[row + c];
}

inline Screen &Screen::set(char c)
{
    contents[cursor] = c;
    return *this;  // 返回值是正在操作的Screen的引用
}

inline Screen &Screen::set(pos r, pos col, char ch)
{
    contents[r*width + col] = ch;
    return *this;  // 返回值是正在操作的Screen的引用
}

// 当在类外定义一个返回类内数据类型的函数时，对于函数名和数据类型都要进行作用域的限制
Screen::pos Screen::fun();
```
如果set的返回值不是Screen&而是Screen，那么只能对操作过程中的temp(临时变量)进行修改，而无法修改Screen本身。  
`MyScreen.set(3, 0, '*')`与`MyScreen.move(3, 0).set('*')`等价。  
解释：`MyScreen.move(3, 0)`返回的是修改后的MyScreen，因此再次执行`MyScreen.set('*')`  
如果要使用量函数，如`const Screen &display(std::ostream &os)`，则最后返回的this指向的是一个常量对象的引用，则之后不能在连续使用move,set等函数，改进方法见上述代码。  
note：类中函数的参数名尽量不要和其他成员的名称相同，因为类内的函数会对其他成员进行this调用。  


#### 聚合类
所有成员都是public，没有定义构造函数，没有类内初始值，用户可以直接访问和初始化所有成员。  

#### 类的静态成员
改变其会使得所有该类型的类的这个成员的值都发生变化。  
```C++
class Account{
public:
    void calculate() {amount += amount * interestRate; }
    static double rate() {return interestRate; }
    static void rate(double );

private:
    std::string owner;
    double amount;
    static double intersetRate;  // 声明静态成员
    static double initRate();
}
// static只能出现在类内部的声明语句中
void Account::rate(double newRate) { interesetRate = newRate; }
```
使用类的静态成员
```C++
r = Account::rate();
Account ac1;
Account *ac2 = &ac1;
r = ac1.rate();
r = ac2 -> rate();
```