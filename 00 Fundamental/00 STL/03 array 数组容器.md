# array 数组容器

- array 没有空间配置器

- \# include \<array\> 原生数组的一个封装

  ```c++
  template<class _Ty, size_t _Size> class array{_Ty _Elems[_Size];...}
  ```

- 明知道最多要多少个元素，可以考虑 array

## 语法

```c++
array<type, size> array_name;

// 例子：
array<int, 10> a;
```

- 元素是在栈上的，注意大小，因为栈空间有限；

## 1. 初始化

1. 默认构造，值没有初始化 ：

```C++
array<int, 10> a1;
```

2. 值初始化，都是 0：

```c++
array<int, 3> a2{};
```

3. 第1个按列表初始化，后面的默认初始化 "":

```c++
array<string, 4> a3 = {"abc"};
```

4. 拷贝：
   - `注意：内置数组不能这样操作`

```c++
array<int, 3> a4(a2);

array<int, 3> a5 = a2;
```

5. 移动：
   - 每个元素都执行行移动（或拷贝）

```c++
array<string, 4> a6 = std::move(a3);
```

6. 不能使用的初始化：

- 不支持迭代器范围初始化

```c++
array<int, 3> a7(a2.begin(), a2.end()); // ×
```

- 指定几个元素初始化不行

```c++
array<int, 6> a8(6);	// ×
    
array<int, 6> a8(6, 0); // ×
```



## 2. 赋值

- 拷贝赋值，但是，`内置数组不支持该操作`：

```c++
a5 = a4;
```

- 用初始化列表赋值:

```c++
a = {2，3};
```

- `fill(value)`成员函数，所有元素都赋值为 value:

```c++
a5.fill(9);	// 所有元素都赋值为 9
```

- 不支持 assign 操作

## 3. Swap 交换：

- `2` 个 array 交换,$O(n)$的时间复杂度（每个元素交换）：
  - 成员函数 `swap()`: array1.swap(array2)
  - 函数 `swap(array1,array2)`

```c++
array<int, 10> a10 = {1,2,3};
array<int，10> a11 = {5,6,7,8,9};

a10.swap(a11);
swap(a10,a11);
```

## 4. 容量相关

- `size()` : 返回元素个数，同非类型模板参数

```c++
a10.size();
```

- `empty()` : 判断是否为空
  - 除非 `array<int, 0> a10`, 否则不可能为 `true`

```C++
a10.empty();
```

- `max_size()` : 同 `size()`

```c++
a10.max_size();
```

- 不支持 `capacity()、reserve()、resize()`:

```c++
a10.capacity(); // error
a10.reserve();	// error
a10.resize();	// error
```

## 5. 元素访问

- `下标访问` or `at()` 成员函数：
  - 要保证下标不越界，否则未定义结果
  - 下标越界抛出 out_of_range 异常

```c++
a10[2] = 10;
a10.at(1) = 20;
```

- `front（）` ：第 1 个元素的引用

```c++
a10.front();
```

- `back()`: 最后一个元素的引用

```c++
a10.back();
```

## 6. 迭代器相关

- 尺寸要匹配：

```c++
array<int, 10>::iterator it1 = a10.begin();
array<int, 10>::const_iterator it2 = a10.cbegin();
array<int, 10>::reverse_iterator it3 = a10.rbegin();
array<int, 10>::const_reverse_iterator it4 = a10.crbegin();
```

## 7. 插入元素，`不存在`

## 8. 删除元素，`不存在`,`clear()`也没有

## 9. 比较运算符

- `==`、`!=`、`<`、`>`、`<=`、`>=`，类型要一致：

```c++
array<int, 2> a12 = {1,2};
array<int, 3> a13 = {1,2,3};

// cout << (a12 == a13) << endl;	不行，类型不匹配
```

## 10. C 接口

```c++
array<char,10> a16 = {'a','b','c','\0'};
printf("%s\n",a16.data()); // abc
// printf("%s\n",a16.begin());	// Error
printf("%s\n", &(*a16.begin()));
```











​												`Time:2020-9-4 12:09:59`