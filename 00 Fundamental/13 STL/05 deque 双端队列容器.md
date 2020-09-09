# deque 双端队列容器

[toc]

> - 头文件 `#include<deque>`
> - deque 双端队列：
>   - `随机访问元素；`
>   - `头部和尾部添加删除元素效率很高；`
>   - `中间删除和添加效率低；`
>   - `元素访问和迭代比 vector 要慢，迭代器不能是普通的指针。`

```C++
template<class _Ty, class _Alloc = allocator<_Ty>> class deque...
```

# 开头代码部分

```c++
#include <iostream>
#include <utility>
#include <string>
#include <deque>
using namespace std;

struct A {
	A(const string& ss1 = "", int i1 = 0) :s1(ss1), d1(i1) {}
	string s1;
	int d1;
};
```

# main 函数部分

## 1. 初始化:与 vector 一样

1. 默认构造：

```c++
deque<int>d1;
```

2. 值初始化构造
   - 单参数
   - 双参数

```c++
deque<string>d7(10);		// 10 个元素，每个都是空串
deque<string>d8(8,"abc"); 	// 8 个元素，每个都是 "abc"
```

3. 初始化列表：

```c++
deque<int>d2{1,2,3};
deque<double>d3 = { 1.0,2.0,3.0 };
```

4. 拷贝
   1. 传参拷贝
   2. 构造拷贝
   3. 移动构造
   4. 迭代器范围拷贝，元素可转换即可

```c++
deque<int>d4(d2);	// 传参拷贝
deque<int>d5 = d2; // 构造拷贝
deque<double>d5 = move(d3);	// 移动构造
deque<string>d6 = (d2.cbegin(), d2.cend();)	// 迭代器范围拷贝，元素可转换即可
```

## 2. 赋值：与 vector 一样

- `=` 号赋值：类型要一致

```c++
d8 = d7;
```



- 用初始化列表赋值：

```c++
d8 = {"aaa","bbb","ccc"};
```

- `assign()成员函数`：
  - 迭代器范围赋值
  - 初始值列表赋值
  - 拷贝容器赋值
  - 赋予特定个数的特定值

```c++
d8.assign(d7.cbegin(), d7.cend()); 	// 迭代器范围赋值
d8.assign({"aa","bb","cc"});		// 初始值列表赋值

initializer_list<string> il = {"a","b","c"};
d8.assign(il);	// 拷贝容器赋值

d8. assign(10, "abc");	// 用 10 个 "abc" 赋值

```

## 3. Swap:  与 vector 一样,两个容器交换， 时间复杂度为 `O(1)`

```c++
deque<int>d10, d11{1,2,3};

d10.swap(d11);
swap(d10,d11);
```

## 4. 容量相关：没有 `capacity` 和 `reserve`，其他与 vector 一样：

- `size()` : 元素个数

- `empty()`: 判断容器是否为空，若 元素个数 == 0, 则返回 true
- `max_size()` ：能存储的最大元素个数
- `resize()`: 
  - 单元素：改变元素个数，删掉多余元素，或当原元素数量不足时以`值初始化`添加元素
  - 双元素：改变元素个数，删掉多余元素，或当原元素数量不足时，以`第二个元素`添加
- `shrink_to_fit()`: 降低容量，单是不是强制性的，只是通知 STL 可以降低容量

```c++
d10.size(); 			// 元素个数
d10.empty();			// 元素个数 == 0，则返回 true
d10.max_size();			// 能存储的最大元素个数
// d10.capacity(); 		// capacity() deque 没有
// d10.reserve(100);	// deque 没有 reserve();
cout<<d10.size()<<endl;
d10.resize(25);			// 改变元素个数，删掉多余元素，或当原元素数量不足时以`值初始化`添加元素
d10.resize(30, 2);		// 改变元素个数，删掉多余元素，或当原元素数量不足时，以`第二个元素`添加
d10.shrink_to_fit();	// 降低容量，单是不是强制性的，只是通知 STL 可以降低容量
```

## 5. 元素访问：与 vector 一样

- 下标访 or `at()成员函数` 随机访问
- `front()成员函数`: 第一个元素的引用 
- `back() 成员函数`：最后一个元素的引用

## 6. 迭代器相关：与 vector、array 一样

详细见 `STL 02 迭代器`

## 7. 插入元素：多了 `push_front、emplace_front、emplace_back`，其他与 vector 一样

- `push_front()`: 头部添加元素

```c++
deque<A> d12;
d12.push_front(A("123", 1));	// 头部添加元素
```



- `push_back()`: 尾部添加元素
- `emplace()`: 在迭代器位置**前面**插入以参数为构造的 A对象，返回指向新插入元素的迭代器
- `emplace_front()`:头部添加元素

```c++
d12.emplace_front("abc",2); // 头部添加元素
```

- `emplace_back()`: 尾部添加元素

- `insert()`: 在迭代器前面插入元素
  - 双元素：
  - 多元素：

## 8. 删除元素：多了 `pop_front` 其他和 vector 一样

- `pop_front（）`：删除第一个元素

```c++
d12.pop_front(); // 删除第一个元素
```

- `clear()`: 清空所有元素，有可能释放空间（一般释放空间—）

## 9. 关系运算 : 和 vector 一样

- 所有容器都支持 `==`、`!=`

- 除无序容器外都支持`<`、`>`、`<=`、`>=`
- 要进行比较的两个容器，每个元素都要进行比较