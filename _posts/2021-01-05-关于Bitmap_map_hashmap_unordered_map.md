---
layout: post
title: "关于Bitmap、map、hashmap、unordered map"
date: 2021-01-07
author: Vajuw
tags: [map存储形式]
---

为了应对不同场景，c++中以key value的形式进行存储的数据结构有多种，其中各自有其底层的实现形式与对应的不同时间复杂度。eg: map底层是红黑树；hash map是hash table + 链表/红黑树......。

<!-- more -->

# bitmap

## 简介

> bitmap可以理解为一个bit数组来存储特定数据的一种数据结构；
>
> 1. 由于bit是数据的最小单位，==这种存储结构非常节省存储空间==。
> 2. bitmap的单位是0/1，非常方便进行位的运算

## 数据结构 or 图片类型

### 数据结构——单位：位

>位图`bitmap` 是一种非常常用的结构，在索引，数据压缩等方面有广泛应用。
>
>

### 图片类型 ——位图的单位：像素

> 位图，又称为点阵图像、像素图或栅格图像，是由像素（图片元素）的单个点组成。这些点可以进行不同的排列和染色以构成图样。
>
> 像素（Pixel）：指可以表现亮度甚至色彩变化的一个点，是构成数字图像的最小单位。像素具有大小相同、明暗和颜色的变化。特点是有固定的位置和特定的颜色值

## 参考链接

- [什么是bitmap？](https://www.jianshu.com/p/6e2285c85295)

---

#  map

map是C++中充当字典（key-value）来使用的数据类型，底层原理是红黑树。想要书顺序的遍历出map的value值==需要重定义<==(operator <)【一般的数据类型系统已经自动实现，若是用户的自定义数据类型，需要重新定义！】

- map的基本操作

```c++
#include<iostream>
#include<map>
#include<string>

using namesapce std;

int main(){
    map<string, int> mymap;
    
   // three ways to insert data
    mymap.insert(pair<string, int>("apple", 2));
    mymap.insert(map<string, int>::value_type("orange",3));
    mymap["banana"] = 6;
    
    // determine whether the element exists?
    if (mymap.empty())
        cout << "this map is empty" << endl;
    else
        cout << "this map has " << mymap.size() << "elements" << endl;
    
    
    // traverse the element
    map<string, int>::iterator iter;
    for(iter = mymap.begin(); iter!= mymap.end(); iter++)
        cout << iter -> first << ends << iter -> second << endl;
    
}
```

- 若map用到的是自定义类型， ==需要重定义<==

```c++
struct Person{
    string m_name;
    int m_age;
    
    Person(string name, int age){
        this -> m_name = name;
        this -> m_age = age;
    }
 
    // 重载<运算符 注意点：1.利用const修饰，限定为只读，避免参数被修改；2.p是作为引用方式传入的，所以调用成员函数时，应该用"."
    bool operator< (const Person& p) const{
        return this -> m_age < p.age
    }
}
```

---

# unordered_map

与map类似，都是key-value进行存储，可以通过key快速索引到value。

差异：

- unordered_m