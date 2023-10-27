---
title: Cpp使用pair或tuple返回多个值
date: 2023-10-15 12:37:12
tags: [C/C++, pair, tuple]
author: VsKendo
mathjax: true
categories: C/C++
summary: 使用C++时使用tuple返回多个值的使用示例。
description: 使用C++11标准的方法完成类似python返回多值的功能。
---



# 解决的问题

函数接受3个参数，并将它们由大到小排序。

```c++
tuple<int, int, int> func(int a, int b, int c)
{
    vector<int> ary{a, b, c};
#define rall(v) v.rbegin(), v.rend()
    sort(rall(ary));
    return make_tuple(ary[0], ary[1], ary[2]);
}

int main(int argc, char *argv[])
{
    int a, b, c;
    auto res1 = func(1, 2, 3);
    tie(a, b, c) = res1;
    cout << a << b << c << endl;
    return 0;
}
```

结果输出321.

# 详解

`tuple<>`可以理解为一个包，可以对多个值进行打包。而这个“包”本身只是一个变量，这样就能达成“C++函数只能返回一个变量”的条件，同时满足“返回了多个值”。

上述例子中，函数`func()`返回了一个tuple变量（通过使用`make_tuple()`函数生成的）。这样调用方就能接收一个变量。

但是这个变量不能直接使用，需要使用`std::tie()`函数来解包才能使用。

上述13与14行可以改为

```c++
tie(std::ignore, std::ignore, c) = res1;
cout << a << endl;
```

从而只取最小值

# 其他听闻

`tuple` 即元组，可以理解为pair的扩展，可以用来将不同类型的元素存放在一起，常用于函数的多返回值。在c++ 11标准库中，加入了std::tie，在c++ 14中改进，方便使用。[1]

pair只能把2个数据打包，而tuple可以打包更多的数据，虽然超过了9个时，其方式就比较搞笑了。[2]

# References

1. https://www.cnblogs.com/RioTian/p/14076214.html
2. https://blog.csdn.net/whereismatrix/article/details/125071222
