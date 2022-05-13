# void类型小记

1、void的作用

　　c语言中，void为“不确定类型”，不可以用void来声明变量。如：void a = 10；如果出现这样语句编译器会报错：variable or field ‘a’ declared void。

　　在C语言中void 常常用于：**对函数返回类型的限**定**和对函数参数限定**　　

　　（1）对函数返回类型的限定：当函数不需要返回类型是必须用void 来限定返回类型，限定了函数的返回类型为void后函数不能有返回值；如：void fun（int a）；

　　（2）对函数参数类型的限定：当函数不允许接受参数时必须用void 来限定函数参数，限定了函数的参数类型为void后函数不能有参数；如：int fun（void）；

2void * 的作用

2,、在C语言中void *是一个很有意思的玩意。

　　C语言中void * 为 “不确定类型指针”，void *可以用来声明指针。如：void * a；

　　（1）void *可以接受任何类型的赋值：

　　　　void *a = NULL；

　　　　int * b = NULL；

　　　　a = b；//a是void * 型指针，任何类型的指针都可以直接赋值给它，无需进行强制类型转换

　　我们可以认为void就是一张白纸可以在上班写任何类型的数值。

　　（2）void *可以赋值给任何类型的变量 但是需要进行强制转换：

　　　　例：

　　　　int * a = NULL ；

　　　　void * b ；

　　　　a = （int *）b；

　　但是有意思的是：void* 在转换为其他数据类型时，赋值给void* 的类型 和目标类型必须保持一致。简单点来说：void* 类型接受了int * 的赋值后 这个void * 不能转化为其他类型，必须转换为int *类型；

例：

```c
#include <stdio.h>

int main (){
     int a= 10;
     void *b = &a;

    printf("int a = %d\n",a);
    printf("void (int *)b =%d \n",*(int *)b);
    printf("void (double *)b =%d \n",*(double*)b);  //编译器并不会报错但是其结果却有点出人意料
    return 0;
}
```

