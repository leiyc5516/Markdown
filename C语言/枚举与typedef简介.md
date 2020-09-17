#### 枚举

枚举类型可以用来声明符号名称来表示整型常量。

~~~c
//枚举声明
enum spectrum{red,orange,yellow,green,blue,violet};
enum spectrum  color;

//使用
int c;
color = blue;
if(color==yellow){
    .....;
}
for(color=red; color <= violet;color++)
    .........;
~~~

默认情况下，enum列表的常量都会被赋值，0，1，2，3....

枚举的用法，枚举的目的是为了提高程序的可读性和可维护性。

枚举类型是整数类型，所以表达式中可以使用整数变量方式使用枚举变量，这样在switch case语句中就方便很多。



#### typedef简介

typedef工具是一个高级数据特征，利用typedef可以为某一类型自定义名称

* 与#define 不同，typedef 创建的符号名只受限于类型，不能用于值
* typedef 由编译器解释，不是预处理器
* 在受限范围内，typedef比起#define更加灵活



##### typedef工作原理

~~~c
typedef unsigned char BYTE;
BYTE x,y[10],*z;
//通常typedef定义的量 都是大写的字母
~~~

~~~c
通过 struct union typedef 可以有效的为C语言提供数据移植工具。 
~~~

