---
title: 从零开始的C语言数组，为何？
tags: [C语言]
categories: C语言
index_img: https://img.czm.cool/post/c_language/C_array.png
banner_img: https://img.czm.cool/post/c_language/C_array.png
date: 2021/07/27 11:13:00
---

在学习C语言的时候，大家总是会产生一个疑惑——C的数组下标，为何从零开始？让我们简单探索一下。

<!--more-->

## 人畜无害的数组

在一个阳光明媚的日子，你快快乐乐的创建了一个数组，打算一探数组的秘密，此刻你有：

```c
#include<stdio.h>

int main(int argc, char *argv[]) {
    int a[3] = {1, 2, 3};
    return 0;
}
```

这是一个简简单单的整形数组，它包含3个数字，C语言非常容易的在内存中创建了这个数组：

```
     -------------------
内存 |  1  |  2  |  3  |
     -------------------
地址 | 101 | 102 | 103 |
     -------------------
```

此时，我们便可以使用```a[下标]```的形式来访问和修改数组中的元素了。但，为什么下标从0开始呢？

## 一切从指针出发

对于我们的数组a，除了用```a[下标]```形式访问，我们还可以用指针形式访问。于是，你有：

```c
#include<stdio.h>

int main(int argc, char *argv[]) {
    int a[3] = {1, 2, 3};
    int *p = a;
    return 0;
}
```

C语言也老实的对内存进行了操作，现在你有：

```
     -------------------
内存 |  1  |  2  |  3  |
     -------------------
地址 | 101 | 102 | 103 |
     -------------------
       p↑
```

此时你敏锐的发现，指针p指向的不就正好是第一个元素吗？所以，你使用```*p```访问了```a[0]```才能访问的第一个元素。这之间似乎有一些隐约的联系。

此时，你思考到，如果要访问第二个元素该怎么办呢？答案是把指针p向后移动一位，也就是```p+1```，于是，你有：

```
     -------------------
内存 |  1  |  2  |  3  |
     -------------------
地址 | 101 | 102 | 103 |
     -------------------
            p+1↑
```

所以，访问第二位变成了```*(p+1)```，而对应的，你用```a[1]```也能访问同样的元素。两者惊人的相似，你也推导出```*(p+i) = a[i]```这个关系。

## 罪魁祸首

此时，你回头查看程序，发现你在使用指针时，并没有使用取地址符```&```，但p却能正确的指向数组a所在的地址。原来，数组名也是指针，它指向了数组第一个元素。

既然指向了第一个元素，那么```*(a+0)```访问第一个元素就变得理所当然了，但这和```a[0]```有什么关系？

关系在于，```a[0]```是```*(a+0)```的简写形式。它们是等价的式子。

因此，数组下标从零开始的原因便找到了。数组下标并不是代表这是数组中的第几个元素，而是代表指针要移动几次才能访问到这个元素。那么自然地，指针只需要移动0次就能访问第一个元素。这也就是为什么访问第一个元素要使用```a[0]```的原因。


## 奇怪的用法

既然```a[i]```等价于```*(a+i)```，我们又知道```*(i+a)```和```*(a+i)```是等价，交换加法两边的顺序显然是不影响结果。因此，我们按照简写的方式，可以推导出一个写法：```i[a]```。下标放在方括号的外面了，这样也是可行的吗？

是的，```i[a]```和```a[i]```是一样的，并不会引起编译错误，也能正确的访问元素。

综上，你得到了一个怪怪的程序：

```c
#include<stdio.h>

int main(int argc, char *argv[]) {
    int a[3] = {1, 2, 3};
    printf("%d\n", a[0]);
    printf("%d\n", 0[a]);
    printf("%d\n", *a);
    printf("%d\n", *(a+0));
    return 0;
}
```