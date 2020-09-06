# list 双向链表容器

[toc]

## 介绍

> - list `双向链表`
>   - 不支持随机访问元素；
>   - 访问头部和尾部元素很快；
>   - 在任何位置插入删除元素都很快，时间复杂度为 O(1);
>   - 插入和删除不会造成迭代器失效；
>   - 空间成本较高，需要两个指针。
> - 头文件 `#include<list>`

```c++
template<class _Ty, class _Alloc = allocator<_Ty>> class list...
```

## 1. 初始化：与 vector 一样

1. 默认构造

```c++
list<int> l1;
```

2. 值初始化构造

```c++
list<int>l7(10);		// 10 个元素，每个元素初始化为 0
list<int>l8(10,1);		// 10 个元素，每个元素为 1
```



3. 初始化列表

```c++
list<int> l2{1, 2, 3};
list<string> l3 = {"abc", "C++", "C"};
```

4. 拷贝
   1. 传参拷贝
   2. 构造拷贝
   3. 移动构造
   4. 迭代器范围构造

```C++
list<int>l4(l2);	// 传参拷贝
list<int>l5 = l2; 	// 构造拷贝
list<string>l6 = move(l3);	// 移动构造
list<int>l7(l2.cbegin(), l2.cend());	// 迭代器范围构造
```

##  2. 赋值：与 vector 一样

- `=` 号赋值：类型要一致

```c++
l1 = l2;
```

- 用初始化列表赋值：

```c++
l2 = {3,4,5};
```

- `assign()成员函数`：
  - 迭代器范围赋值
  - 初始值列表赋值
  - 拷贝容器赋值
  - 赋予特定个数的特定值

```c++
l2.assign(l1.cbegin(), l1.cend()); 	// 迭代器范围赋值
l2.assign({1,2,3});		// 初始值列表赋值

initializer_list<int> il = {6,6,6};
l2.assign(il);	// 拷贝容器赋值

l6. assign(10, "abc");	// 用 10 个 "abc" 赋值

```

## 3. Swap:  与 vector 一样,两个容器交换， 时间复杂度为 `O(1)`



```c++
list<int>l9(20, 1);	// 20 个元素都是 1
list<int>110(10, 2);	// 10 个元素都是 2

l9.swap(l10);
swap(v9,v10);	//	会执行上面的函数
```

## 4. 容量相关：不需要预先准备空间,并且没有扩容、缩容的`capacity、reserve、shrink_to_fit`操作

- `size()`:元素个数
- `empty()`:判断容器是否为空，若个数==0,则返回 true
- `max_size()`:能存储的最大元素个数
- `resize()`:
  - 单元素：改变元素个数，删掉多余元素，或当原元素数量不足时以`值初始化`添加元素
  - 双元素：改变元素个数，删掉多余元素，或当原元素数量不足时，以`第二个元素`添加

```C++
l2.size();	//	元素个数
l2.empty(); //	元素个数 == 0，则返回 true
l2.max_size();	//	能存储的最大元素个数
// l2.capacity(); // list 没有 capacity(不存在扩容这样的操作)
// l2.reserve(100);	// 没有
l2.resize(25);	// 改变元素个数，删掉多余元素，或当原元素数量不足时以`值初始化`添加元素
l2.resize(30, 2); // 改变元素个数，删掉多余元素，或当原元素数量不足时，以`第二个元素`添加
// l2.shrink_to_fit(); // 不需要预先准备空间，有1个元素就开辟1个空间
```

## 5. 元素访问：不能随机访问，头尾快，中间慢

- 不支持 下标[] or `at()` 随机访问：

```C++
// l2[1] = 2; 		// list 不行
// l2.at(1) = 20; 	// list 不行
```

- list 访问中间的元素需要`遍历`：
  - `advance(迭代器_it，length)`: 迭代器_it 向前 移动 length 步
  - `next(迭代器_it, length)` : 从 迭代器\_it 向前移动 length 步，但 迭代器\_it 不变，返回值是移动后的位置

```c++
// 访问第5个元素
list<int> l10 = {1,2,3,4,5,6,7};
auto it = l10.begin();
for(int i = 0; i < 4; ++i) ++it;
cout << *it << endl;

std::advance(it, 4); // it 迭代器向前移动 4 步
cout << *it << endl;

auto it0 = std::next(it, 4); // 从 it 向前移动 4 步，但是 it 不变，则返回值是移后的位置
cout << *it10 << " " << *it << endl;
```

- `front()`:第一个元素的引用 ：

```c++
if(!l2.empty()) 
    l2.front() = 1;
```

- `back()`: 最后一个元素的引用 ：

```c++
if(!l2.empty())
    l2.back() = 2;
```

## 6. . 迭代器相关：与 vector 一样，因为是双向链表，所有反向迭代器都 OK 

详细见 `STL 02 迭代器`

## 7. 插入元素：支持 `push_front(类似deque)`，其他与 vector 一样

- push_back
- emplace_back
- emplace
- insert

```c++
l10.push_front(100);
```

## 8. 删除元素：

- `pop_back()`: 删除最后 1 个元素
- `pop_front()`: 删除第1 个元素

```c++
if(!l10.empty()) l10.pop_back();	//	删除最后 1 个元素
if(!l10.empty()) l10.pop_front();	// 	删除第1 个元素
```

- `erase()`:
  - 单迭代器参数：删除**迭代器位置的元素**，**返回指向被删除元素后面元素的迭代器**，若迭代器是 end(), 则行为未定义
  - 双迭代器参数：删除**迭代器范围**，**返回最后1个删除元素之后元素的迭代器**

```c++
l10.erase(l10.begin());
l10.erase(l10.begin()/*+1 不支持*/, l10.end());
```

- `clear()`: 清空所有元素

```c++
l10.clear()
```



## 9. 关系运算 : 和 vector 一样

- 所有容器都支持 `==`、`!=`

- 除无序容器外都支持`<`、`>`、`<=`、`>=`
- 要进行比较的两个容器，每个元素都要进行比较