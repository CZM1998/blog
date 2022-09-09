---
title: C语言基本语法速览
tags: [C语言]
categories: C语言
date: 2020/12/01 22:22:22
index_img: https://img.czm.cool/post/c_language/C%E8%AF%AD%E8%A8%80.png
banner_img: https://img.czm.cool/post/c_language/C%E8%AF%AD%E8%A8%80.png
---

C语言诞生于1969年至1973年间，由丹尼斯·里奇与肯·汤普逊在B语言的基础上改造而成。C语言以其简洁、高效、灵活的语法和极强的性能迅速流行起来。大量的基础性软件由C语言制作，并成为现在许多服务的基础。日至今日，C语言仍然在编程语言中占据极为重要的地位，是目前最流行的语言之一。而在C语言诞生之后的语言，也或多或少的受到C语言的影响。诸如C++，Java，C#等语言中，都能看到C语言的影子。

<!--more-->

## C的诞生与影响

C语言诞生于1969年至1973年间，由丹尼斯·里奇与肯·汤普逊在B语言的基础上改造而成。C语言以其简洁、高效、灵活的语法和极强的性能迅速流行起来。大量的基础性软件由C语言制作，并成为现在许多服务的基础。日至今日，C语言仍然在编程语言中占据极为重要的地位，是目前最流行的语言之一。而在C语言诞生之后的语言，也或多或少的受到C语言的影响。诸如C++，Java，C#等语言中，都能看到C语言的影子。

## 一段包含基本语法的C程序

C语言语法简单，结构清晰，学习容易。你可以通过一个简单的程序，了解到C语言的大部分语法知识。并能够运行一个简易的C程序。

以下程序中包含C语言的一些基本语法，更多更详细的语法可以查阅C标准文件。相信聪明的你，一定能在很短的时间内掌握C语言。

```c
//  单行注释到行尾结束
/*
    多行注释可以任意放置但要注意嵌套问题
    注释是用来说明解释程序的，他非常重要
*/

// 引入头文件便能使用头文件声明的函数
#include "my.h"    // 引入自己的头文件
#include <stdio.h> // 引入标准库头文件
#include <stdlib.h>
#include <string.h>
#include <math.h>

// 使用宏定义一些直接替换文本的值
#define MAX_WIDTH 22
#define MAX_RADIUS 11

// 宏还可以进行判断，按照条件进行不同的编译
#ifdef __unix__
#define SYSTEM "unix"
#elif __linux__
#define SYSTEM "linux"
#elif _WIN32
#define SYSTEM "windows"
#elif _WIN64
#define SYSTEM "windows"
#else
#error "unkonw system"
#endif

// 宏还可以定义函数，会将参数原样替换到后面的语句，多个语句用\换行
// sizeof 是C关键字，用于计算变量的大小
// C语言将 (类型) 放在变量前进行类型强制转换
#define MALLOC(type, name, len)                     \
    type* name = (type*)malloc(sizeof(type) * len); \
    name = memset(name, 0, sizeof(type) * len)      \

// 宏里可以嵌套另一个宏，但嵌套自己的结果由编译器决定
#define CREATE_CIRCLE(type, name, len)                          \
    MALLOC(type, name, len);                                    \
    name->radius = 0;                                           \
    name->width = 0;                                            \
    name->printCircle = print##type;                            \
    printf("type " #type " has been created in %s\n", SYSTEM)   \
    //在宏里可以使用两个#号拼接一个标识符
    //使用一个#将其转化成字符常量

// 结构体可以将不同的数据类型组成一个更加复杂的数据类型
struct Position {
    int x;
    int y;
};

// typedef 可以用另一个名字来代替当前类型
typedef struct _Circle {
    int radius;
    int width;
    struct Position point;
    // 函数指针可以指向一个同形式的函数并像正常函数一样调用
    // 在结构体中声明自己类型的变量
    void (*printCircle)(struct _Circle*);
} Circle;


// 函数先声明再使用
void printCircle(Circle* circle) {
    // 使用&获取变量的地址
    struct Position* position = &circle->point;

    // 即使在同一个语句中，你可以声明多个变量，并为其单独赋值
    int w, h = 0;
    // 你可以像这样从右往左依次赋同一个值
    int i = w = 0;
    do {
        // C语言中，0表示假，非0表示真。所有表达式的结果都会成为0或非0
        while(1) {
            // 变量加一，有四种写法
            for(h = 0; h < circle->width; h += 1) {
                for(w = 0; w < circle->width; w = w + 1) {
                    int distance = pow(h - position->x, 2) + pow(w - position->y, 2);
                    int r = pow(circle->radius, 2);
                    // 对于一些二选一的判断，可以使用三目运算
                    char result = distance <= pow(circle->radius, 2) ? '*' : ' ';
                    // 结果繁多时，可以使用switch选择某种已知结果操作，如果没有加上break，会一直执行下去
                    switch (result) {
                        case '*': printf("%c",result); break;
                        case ' ':
                        default: // 所有结果不匹配时会执行此处语句
                            printf("%c",' '); break;
                    }
                }
                printf("\n");
            }

            // C语言的条件表达式中，你可以使用逻辑运算来组成复杂的判断
            // 如 与&& 或|| 非! 等== 不等!= 小于 < 大于> 小于等于 <= 大于等于 >=
            if (w != circle->width && h != circle->width) {
                continue; // 继续吧！这个语句便是让循环跳过后面的语句，直接进入下一个循环中
            } else if(w == circle->width && h == circle->width) {
                // 看上面，它是对另一种情况进行判断
                break;
            }
        }
        break;
    } while (++i); // 这是C语言中的一种循环，他保证循环至少执行一次
}

// 函数定义可以和声明分开，但声明一定要在main之前
Circle* createCircle(int x, int y, int radius, int width);

// 一切C程序均从main函数执行
int main(int argc, char** argv) {
    int width[2] = {22, 18}; // 数组采用此种方式声明并赋值初始化，如果初始化，则[]中的数字是不必要的
    int* w = width; // 数组名等价于一个指针
    int* radius = (int*)malloc(sizeof(int) * 2); // 动态分配大小的数组由此产生

    *(radius + 1) = 8;  // 数组并不只能通过[]来访问，指针加偏移量也是可以的
    radius[0] = 10;     // 同样的，也可以用[]来访问动态大小的数组元素

    Circle* circle; // 声明变量可以不初始化，但这样比较危险
    // 循环的一种形式，由仅执行一次的初始语句，每次循环开始前执行的判断语句，每次循环结束后执行的语句组成
    // i++ 表i执行后自增1 ++i表执行前自增1 其他符号同理
    for(int i = 0; i < 2; i++) {
        // 一般调用函数是这样的
        circle = createCircle( w[i] / 2, *(w + i) / 2, radius[i], w[i]);
        void (*print)(Circle*) = circle->printCircle;
        // 当然也可以是这样的
        (*print)(circle);

        // 动态申请的内存需要自己手动释放，避免出现内存泄露
        free(circle);
    }

    // 非void函数需要返回值
    return 0;
}

Circle* createCircle(int x, int y, int radius,int width) {
    CREATE_CIRCLE(Circle, circle, 1);
    struct Position position;
    position.x = x;
    position.y = y;             // 在栈上申请的内存可以直接访问
    circle->point = position;   // 在堆上申请的内存需要解引用才能访问
    (*circle).radius = radius;  // 对结构体解引用有两种写法，->是这种写法的简化
    circle->width = width;
    return circle;
}
```

~~想必你已经掌握了C语言的基本操作了，现在就去写一个linux吧！~~

## 简单的小测试

下面的代码段运行后的结果是什么呢？相信难不倒聪明的你。

```c
#define X (int (*)(int,int,int))
typedef struct F{
	int a;
	int (*b)(int,int,int);
}F;

int c(int d,int e,int f){
	return !d?e:(*c)(--d,f,(long)&(((char*)e)[f]));
} 

int main(){
	F *f = (F*)malloc(sizeof(F));
	int g[] = {&((*f).a),*c};
	memcpy(f,g,sizeof(F));
	printf("%d\n",(X(*(int*)((int)(&f[f==f])-sizeof(int))))((int)((long)((int*)f+(f>NULL))-f->a)+!!f,NULL,'b'-'a'));
    return 0;
}
```

~~上面的程序都是能够运行的，但我用的什么编译器呢~~