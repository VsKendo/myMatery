---
title: Cpp 数组与vector操作技巧
date: 2023-10-15 12:54:37
tags: [C/C++, vector]
top: true
author: VsKendo
categories: C/C++
summary: 快速创建数组，包括使用容器和普通数组的方式。
description: 快速创建数组，包括使用容器和普通数组的方式。
---

# memset填充问题

我们知道，使用 `memset()`只能将整数数组填充0和-1，为了将其填充为我们想要的值，可以使用`fill`初始化而非 `memset()`。

```c++
int ary[10];
memset(ary,-2,sizeof ary); // fail
fill(ary, ary + 10, -2); // ok
```

这样，即使要填充我们想要的值是自定义类也无所谓。

```c++
class T
{
  public:
    int a;
    int b;
    int c;
    T(){};
    T(int a, int b) : a(a), b(b)
    {
        c = a + b;
    };
};
T ts[3];
fill(ts, ts + 3, T(1, 3)); // it works
vector<T> vt(3, T(1, 2)); // ok
```

而使用`vector`则更为方便，直接在构造函数中填充3个T对象。

# 非容器对象使用 max_element

容器类对象可以使用 `max_element`方法找到容器中的最大值，如：

```c++
vector<int> v{1, 9, 5, 4};
auto iter = max_element(v.begin(), v.end()); // iterator<vector<int>>
cout << *iter << endl; // value of the max element
cout<< iter - v.begin()<<endl;// index of the max element
```

而非容器对象没有迭代器，所以会返回地址。这里上下两块的代码运行结果完全一致。

```c++
int v[] = {1, 9, 5, 4};
auto iter = max_element(v, v + 4); // address of the max element
cout << *iter << endl; // value of the max element
cout << iter - v[0] << endl; // index of the max element
```

# 获得数组长度

容器类对象提供 `size()和length()`函数都表示存储了几个对象，而普通数组没有。此时可使用

```c++
#define arySize(arr) sizeof(arr) / sizeof(arr[0])
int v[] = {1, 9, 5, 4};
cout<< arySize(v) << endl;
```

第一行的宏定义来获取数组长度，类似于 python 的 `len()`函数。
