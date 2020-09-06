# vector 向量容器

> - 头文件：`#include\<vector\>`
> - vector  动态数组：
>   - `可以尾部删除、添加，速度快；`
>   - `遍历非常快；`
>   - `内存紧凑，使用的空间较少`。

```c++
template<class _Ty, class _Alloc = allocator <_Ty>> class vector....
```



# 代码开头部分

```C++
// 代码开始头文件部分
#include <iostream>
#include <utility>
#include <string>
#include <vector>
using namespace std;

struct A {
	A(const string& ss1 = "", int i1 = 0) :s1(ss1), d1(i1) {}
	string s1;
	int d1;
};
```

- 说明：

  **#include \<utility\>** :

  1. utility头文件定义重载的关系运算符，简化关系运算符的写入;
  2. 还定义了pair类型，该类型是一种模板类型，可以存储一对值。这些功能在库的其他地方使用。

# main() 函数部分

## 1. 初始化

1. 默认构造

```c++
vector<int> v1;
```

2. 值初始化构造

```c++
vector<int>v8(10);		// 10 个元素，每个元素初始化为 0
vector<int>v9(10,1);	// 10 个元素，每个元素为 1
```



3. 初始化列表

```c++
vector<double> v2{1.0,2.0,3.0};
vector<string> v3 = {"abc", "C++", "C"};
```

4. 拷贝
   1. 传参拷贝
   2. 构造拷贝
   3. 移动构造
   4. 迭代器范围构造

```c++
vector<double> v4(v2);	// 传参拷贝
vector<string>v5 = v3; 	// 构造拷贝
vector<string>v6 = move(v3);	// 移动构造
vector<double>v7(v2.cbegin(), v2.cend());	// 迭代器范围构造
```

## 2. 赋值（会冲掉原先的内容）

1. 拷贝赋值

```c++
v8 = v1;
```

2. 初始化列表赋值

```c++
v8 = {1,2,3};	// 用初始化列表赋值
```

3. `assign()成员函数`: 
   1. 初始化值列表赋值
   2. 用特定值赋值
   3. 迭代器范围赋值
   4. 拷贝容器赋值

```C++
v8.assign({1,2,3});	// 初始化值列表赋值
v8.assign(10, 2);	// 用 10 个 2 赋值

v8.assign(v9.cbegin(),v9.cend());	// 迭代器范围赋值

initializer_list<int> il = {1,2,3};
v8.assign(il);	// 拷贝容器赋值
```

## 3. Swap 交换:`2 个 vector 交换，O(1) 时间复杂度`

```c++
vector<int> v10(20, 1);	// 20 个元素都是 1
vector<int> v11(10, 2);	// 10 个元素都是 2

v10.swap(v11);
swap(v10,v11);	//	会执行上面的函数
```

## 4. 容量相关

- `size() `: 元素个数

```c++
v10.size();
```

- `empty() `: 元素个数 == 0 ，返回 true

```c++
v10.empty();
```

- `max_size()`: 返回能存储的最大元素的个数

```c++
v10.max_size();
```

- `capacity()` : 返回**当前**能存储的最大元素个数

```c++
v10.capacity();
```

- `reserve()`: 只能扩容，不能缩容，不会改变元素个数

```c++
v10.reserve(100);
```

- `resize()`:
  - 单参数：改变元素个数，删掉多余元素，或当原元素数量不足时以值初始化添加元素
  - 双参数：改变元素个数，删掉多余元素，或当原元素数量不足时，以第二个元素添加

```c++
v10.resize(25);
v10.resize(30, 2); // 改变元素个数，删掉多余元素，或以 2 添加元素
```

- `shrink_to_fit()`: 降低容量，但是不是强制性的，只是通知 STL 可以降低容量

```c++
v10.shrink_to_fit();
```

## 5. 元素访问

- `下标访问` or `at()成员函数` ： 
  - 下标访问，要保证下标不越界，否则未定义结果 
  - 下标越界抛出 out_of_range 异常

```c++
v11[1] = 2;	// 下标访问
v11.at(1) = 20; // at() 成员函数
```

- `front()`:第一个元素的引用 ：

```c++
if(!v11.empty()) 
    v11.front() = 1;
```

- `back()`: 最后一个元素的引用 ：

```c++
if(!v11.empty())
    v11.back() = 2;
```

## 6. 迭代器相关

详细见 `STL 02 迭代器`

```c++
// begin() - end()
vector<int>::iterator it1_begin = v11.begin();
vector<int>::iterator it1_end() = v11.end();

// cbegin() - cend()
vector<int>::const_iterator it2_cbegin = v11.cbegin();
vector<int>::const_iterator it2_cend = v11.cend();

// rbegin() - rend()
vector<int>::reverse_iterator it3_rbegin = v11.rbegin();
vector<int>::reverse_iterator it3_rend = v11.rend();

// crbegin() - crend()
vector<int>::const_reverse_iterator it4_crbegin = v11.crbegin();
vector<int>::const_reverse_iterator it4_crend = v11.crend();

```

## 7. 插入元素

- `push_back()`: 尾部添加元素

```c++
vector<A> v12;
v12.push_back(A("123", 1)); // 尾部添加元素

A a1("abc", 2);
v12.push_back(a1);	// 元素存入容器是拷贝
v12.push(move(a1));	// 尾部添加元素，直接调用 A 的构造
```

- `emplace()`: 在迭代器位置**前面**插入以参数为构造的 A对象，返回指向新插入元素的迭代器

```c++
// 在迭代器位置前面插入以参数 "aac",9 为构造的 A对象，返回指向新插入元素的迭代器
v12.emplace(v12.begin(), "aac", 9);
```

- `insert()`: 在迭代器前面插入元素

  - 双元素：

  ```c++
  A a2("acc", 3);
  
  // 在迭代器位置前面插入 a2,返回指向新插入元素迭代器
  vector<A>::iterator it_insert = v12.insert(v12.begin(), a2);
  cout << (*it_insert).s1 << "--" << (*it_insert).d1 << endl;
  
  // 在迭代器前面插入 3 个 a2, 返回指向第1个新元素的迭代器
  v12.insert(v12.begin(),3,a2);
  ```

  - 多元素：

  ```c++
  vector<A> v13;
  
  // 在迭代器位置前面插入 v12.begin() 到 v12.end() 之间的元素，返回指向第1个新元素的迭代器
  // 若 v12.begin() == v12.end() 表示不插入一个元素，返回原迭代器位置
  v13.insert(v13.begin(), v12.begin(), v12.end());
  v13.insert(v13.begin(),{{"aa",1},{"bb",2}});	//	用初始值列表插入
  ```

  - 注意：begin() == end() 表示不插入一个元素，返回原迭代器位置

## 8. 删除元素

- `pop_back()` : 删除最后一个元素

```c++
v13.pop_back();
```

- `erase()`:

  - 单元素：删除迭代器位置的元素，返回指向被删元素后面元素的迭代器，若迭代起是 end()，则行为未定义

    ```C++
    v13.erase(v13.begin());
    ```

    

  - 双元素：删除迭代器范围，返回最后1个删除元素之后元素的迭代器

    ```c++
    v13.erase(v13.begin() + 1, v13.end());
    ```

    

- `clear()`:清空所有元素，但是容量不会变（capacity()不变）

```C++
v13.clear();
```

## 9. 关系运算

- 所有容器都支持 `==`、`!=`
- 除无序容器外都支持`<`、`>`、`<=`、`>=`
- 要进行比较的两个容器，每个元素都要进行比较

```c++
vector<int> v14 = {1,2,3};
vector<int> v15 = {1,2,4};

v14 < v15; // true : 每个元素比较
v14 == v15;	// false
```



## 10. 与 C 的接口

```c++
vector<char> v16(10, '\0');
strcpy(&v16[0], 'abcd');
printf("%s\n", &v16[0]);	// abcd
strcat(v16.data(),"1234");
printf("%s\n",v16.data());	// abcd1234
// printf("%s\n",v16.begin());	// error
printf("%s\n",&(*v16.begin()));	// OK
// 行为像指针，但是所指对象的地址是 &* 迭代器
```

