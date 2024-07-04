### Sequential Container
##### 顺序容器类型
`vector`(可变大小数组),`deque`(双端队列),`list`(双向链表),`forward_list`(单向链表),`array`(固定大小数组),`string`(字符类型的vector)  
  

##### 迭代器
确定收尾位置，在这一区间里操作，该区间通常为**左闭合区间**，开头是第一个元素(begin)，末尾是最后一个元素的下一个位置(end)  
当begin==end时，迭代结束  


##### 容器定义和初始化
**拷贝初始化**：拷贝容器/拷贝由一个迭代器指定的元素范围  
拷贝时要求容器类型和元素类型都匹配，迭代器指定范围时只要元素类型符合要求即可（类型转换）  
```C++
vector<const char*> articals = {"xz", "tu", "sd"};
vector<string> words(articals);  // wrong
deque<string> words(articals.begin(), articals.end());  // right，const char* 可以转换成string
//由于迭代器的左闭区间特性，拷贝的最后一个元素是范围末端的前一个元素
```

**列表初始化**：显式定义  
`vector<const char*> articals = {"xz", "tu", "sd"};`  

**容量初始化**：定义容器的大小和初值  
`vector<int> v1(10, 3);`  // 10个Int,初值为3，若没有初值，则为默认初始值0  

**array**  
`array<int, 10> a1 = {0, 6};`  
需要指定类型和大小，对于数组类型a[10]不能进行拷贝初始化，但是array类型可以  
`array<int, 10> a2 = a1;  // right`  
只有初始化的时候才能进行不完整的赋值，当后续的拷贝操作中，传入的array元素个数必须相等  
注：
```C++
#include <iostream>
#include <array>

using namespace std;

int main()
{
	array<int, 3> a1 = { 1, 3 };
	a1 = { 3};  // {3}被转换为{3,0,0}赋给a1
	for (auto t1 = a1.begin(); t1 != a1.end(); ++t1)
	{
		cout << *t1 << endl;
	}
	return 0;
}
```
##### 容器基本操作
**assign(仅顺序容器)**  
赋值要求对象类型相同，而assign的使用更宽泛
```C++
list<string> names;
vector<const char*> oldstyle;
names = oldstyle;  // wrong
names.assign(oldstyle.cbegin(), oldstyle.cend());  // 元素拷贝
names.assign(10, "Ba");  // 给names 10个“Ba”元素
```

**swap**  
交换两个类型相同的容器的内容  
`swap(s1, s2);`
除了array之外，swap只是对容器的的数据结构进行交换，若有指针p指向s1，则swap之后p指向s2  
如果是string类型，会导致迭代器，引用和指针失效  
尽量使用非成员版本的swap  

**size()**:返回元素个数  
**empty()**：无元素，返回True，否则False  
**max_size()**：返回一个大于或等于该类容器能容纳的最大元素值  

**关系运算符**  
所有容器都可以使用`==`,`!=`,判断容器是否相同  
`> >= < <=`只在顺序容器中有效  

**push_back(element)**  
将元素的拷贝添加到容器末尾，array,forward_list不支持  

**push_front(element)**  
将元素的拷贝添加到容器头部，list,forward_list,deque支持  

**insert**  
在任意位置插入任意数量的元素，一般是迭代器指定位置之前的位置，vector,deque,list,string支持  
```C++
list<string> slist;
vector<string> v = {"a","b","c","d"};
slist.insert(slist.begin(), "Blue");  // 在开头插入
slist.insert(slist.end(), 5, "Red");  // 在末尾批量插入
slist.insert(slist.begin(), v.end() - 2, v.end());  // 将v的两个末尾元素插入到slist前部

auto iter = slist.begin();
while (cin >> wrod)
    // 下一条语句等价于push_fron
    iter = slist.insert(iter, word);  // insert的返回值是元素插入位置的迭代器
```

**emplace**  
若容器的元素类型是某个类，则emplace可以通过调用类的构造函数，创建对象，插入到容器中  
而push_front,insert,push_back只能通过对已有对象的拷贝将元素插入的容器中  
emplace_front,emplace,emplace_back  
```C++
c.emplace_back();  // 调用默认构造函数 
c.emplace(iter, "999");
c.push_back();  // wrong
```

**元素访问**  
元素访问的返回是**引用**，如果引用对象不是const，可以进行修改  
访问前需要确认容器中是否有元素，且确保下标不会越界  
back 不适用与forward_list  
下标访问仅适用于string,vector,deque,array  
```C++
auto iter = c.begin();
auto val1 = *iter;  // 通过对迭代器的解引用来访问
auto val2 = c.front();  // 返回首元素的引用，c为空时报错
auto val3 = c.back();  // 返回尾元素的引用，c为空时报错
auto val4 = c[n];  // 返回下标为n的引用，n >= c.size()时报错
auto val5 = c.at(n);  // 返回下标为n的引用，下标越界会出现out_of_range报错
```

**删除元素**  
forward_list不支持pop_back  
vector,string不支持pop_front  
`pop_back()`：删除尾元素  
`pop_front()`：删除首元素
`clear()`：删除所有元素  
`erase(iter)`：删除迭代器指向的元素
`erase(iter1, iter2)`：删除迭代器所指范围之间的元素，若iter1!=iter2，则删除iter1指向的元素  

**forwaed_list中有特殊的插入/删除元素操作**  
**改变容器大小**
```C++  
c.resize(n, t);  // 将容器的元素个数改为n个，多出来的元素用t作为初值 
c.capacity();  // 查询不重新分配内存的情况下，c可以保存多少元素
c.reserve(n);  // 告知c分配至少能弄那n个元素的内存空间
c.shrink_to_fit();  // 让capacity()与size()大小相等
```

**字符串的独有操作**  
《Primer》p321  

**容器适配器**  
《Primer》p329  
stack, queue, priority_queue  

