## 标准库模板 map 映射
map是一种关联性容器，使用了键值对来存储元素，在存储一对一的数据时很有用。   
map的键值key总是有序的且唯一的，并且对应一个value。   
### 头文件支持   
```c  
#include <map>   
```   
### 初始化
map由于键值对的特殊性，不支持批量同样值的初始化，仅支持以下三种。   
```c
map<char, int> m;	//无值初始化	  
map<char, int> m1 = { {'a',1},{'b',2} };	//初始化列表初始化   

//使用pair作为元素初始化   
pair<char, int> p1 = { 'a',1 };   
pair<char, int> p2 = { 'b', 2 };   
map<char, int> m2 = { p1, p2 };    
```   
### 添加删除元素
添加元素使用了一个重载运算符[]以及三个相关的函数。   
```c
map<char, int> m;	//无值初始化	  
	
//（1）通过insert函数   
//插入单个元素     
m.insert({ 'a', 1 });	//通过初始化列表   
m.insert(pair<char, int>('b', 2));	//通过pair    
m.insert(map<char, int>::value_type('d', 4));	//通过value_type   

map<char, int>::iterator itr = m.find('d');   
//如果位置指向插入元素之前的元素，函数将优化其插入时间。    
m.insert(itr, { 'e',5 });	//d在e之前一个，效率最高        
m.insert(itr, { 'f',6 });	//d并不是f之前第一个，效率没有被优化    

map<char, int> anothermap;    
anothermap.insert(m.begin(), m.end());	//通过区间[)插入    

//（2）通过[]   
m['c'] = 3;	//直接通过[]来进行赋值     

//（3）通过emplace插入单对元素   
//emplace是c++11新推出的函数，它直接在容器中调用构造函数，因此比以前的insert以及其他函数更加高效。   
m.emplace(pair<char, int>('g', 7));    
m.emplace( 'h',8 );		//使用emplace不需要初始化列表    
m.emplace(map<char, int>::value_type('i', 9));	//通过value_type     

//（4）通过emplace_hint来插入单对元素   
//如果位置指向插入元素之前的元素亦或者指向最后，那么函数将优化插入时间     
itr = m.find('i');    
m.emplace_hint(itr,  'j',10 );	//指向前一个元素  
m.emplace_hint(m.end(),  'k', 11 );	//指向最后   
```   
删除操作主要通过两种，一种是erase函数，一种是clear函数。   
```c
map<int, int> m;  
for (int i=0; i < 10; i++)   
	m[i] = 100 * i;   

//（1）通过erase函数。c++98不会返回任何值。c++11会返回最后一个被删除的元素紧随其后的一个元素的迭代器，也可能是map::end    
map<int,int>::iterator itr= m.erase(m.begin());		//通过迭代器位置删除单个元素，返回的是1，100    
itr = m.erase(m.begin(), m.end());	//以(]通过范围删除      
m.erase(9);	//通过key直接删除，但是这种情况不返回迭代器。      

//（2）通过clear删除    
m.clear();	//删除全部内容  
```  
### 赋值操作
赋值的方式比较有限，没有其他标准库模板普遍支持的assign函数。   
```c
map<int, int> m;   
for (int i=0; i < 10; i++)   
	m[i] = 100 * i;  

map<int, int> n;   
n = m;	//通过赋值号整体赋值   

n[100] = 100;	//通过[]号赋值   

n.at(1)= 5;	//通过at赋值   
```
### 调整空间
不能调整空间。  
### 功能函数
map的功能函数不少，如下。   
map<char, int> m= {{'a',1},{'b',2}};   

//查找函数，返回值为两个迭代器组成的范围，第一个迭代器指向查找的值，第二个函数指向下一个值（可能是end）。    
pair<map<char, int>::iterator, map<char, int>::iterator> ret;   
ret = m.equal_range('b');    

map<char, int>::iterator itr= m.find('a');	//返回查找到的迭代器，没查找到返回map::end。   
int ret= m.count('a');	//返回相同key的数量，但是由于key是唯一的，所以只会返回0或者1。    
m.empty();	//返回是否为空。   
m.size();	//返回大小  
m.maxsize();	//返回最大的大小，即内存可容纳的大小。  
itr= m.lower_bound('a');	//返回a的迭代器。该函数返回第一个不在a之前的迭代器。   
itr= m.upper_bound('a');	//返回b的迭代器。该函数返回第一个在a之后的迭代器。   
m1.swap(m2);	//互相交换元素  
#### map::key_comp和map::value_comp函数
这是一个类内的成员函数，用作key值的比较。由于在其它模板类中比较少见，单独拿出来说。   
```c
map<char, int> m = { pair<char,int>('a',1), pair<char,int>('b',2), pair<char,int>('c',3) };   
map<char, int>::key_compare compar = m.key_comp();	//返回一个key比较函数，默认比较为小于   
char big = 'c';   
map<char, int>::iterator itr = m.begin();   
while (compar(itr->first, big))	//如果迭代器的first比big小则返回true   
{    
	cout << itr->first << " " << itr->second << endl;    
	itr++;  
}   

//value_compare同理   
map<char, int>::value_compare compar= m.value_comp();	//返回一个value的比较函数，默认为小于。   
```   
以上就是全部的功能函数了。   
### 迭代器及遍历
迭代器是一种检查容器内元素并遍历元素的数据类型。C++更趋向于使用迭代器而不是下标操作，因为标准库为每一种标准容器定义了一种迭代器类型。   
```c
map<char, int> m = { pair<char,int>('a',1), pair<char,int>('b',2), pair<char,int>('c',3) };     
map<char, int>::iterator itrBegin = m.begin();	//正常迭代器     
map<char, int>::iterator itrEnd = m.end();    
map<char, int>::const_iterator citrBegin = m.cbegin();	//常迭代器     
map<char, int>::const_iterator citrEnd = m.cend();    
map<char, int>::reverse_iterator ritrBegin = m.rbegin();	//反向迭代器   
map<char, int>::reverse_iterator ritrEnd = m.rend();    
map<char, int>::const_reverse_iterator critrBegin = m.crbegin();	//常反迭代器    
map<char, int>::const_reverse_iterator critrEnd = m.crend();     

//由于该容器为键值对，因此迭代器并非常用的解引用，而是调用first和second来获取key和value     
for (; itrBegin != m.end(); itrBegin++)    
{     
	cout << itrBegin->first << " " << itrBegin->second;   
}    
```   
### 总结
1 map的key总是有序的。如果key使用自定义类型，那么需要在里面重载<号。   
2 map的key总是唯一的。如果key使用自定义类型，需要重载==号。   
3 map既可以用初始化列表，也可以用pair，也可以用map::value_type进行初始化   
4 map适用于一些一对一的数据类型。    
5 偶尔可以用map的有序和唯一做一些骚操作，不过效率不明。   