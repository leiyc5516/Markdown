~~~c
#include <stdio.h>
#include<string.h>
char * s_gets(char *st,int n);
#define MAXTITL 41
#define MAXAUTL 31
struct book{
    char title[MAXTITL];
    char author[MAXAUTL];
    float value;
};
int main(void)
{
    struct book library;/*将library声明为一个book类型的变量*/
    printf("please enter the book title.\n");
    s_gets(library.title,MAXTITL);
    printf("Now enter the author.\n");
    s_gets(library.author,MAXAUTL);
    printf("Now enter the value");
    scanf("%f",&library.value);
    printf("%s by %s :$%.2f\n",library.title,library.title,library.value);
    
    printf("%s: \"  %s \" ( :$%.2f)\n",library.title,library.title,library.value);
    printf("Done.\n");


    printf("Hello world\n");
    return 0;
}
char * s_gets(char * st, int n)
{
    char * ret_val;
    char * find;
    ret_val = fgets(st,n,stdin);
    if(ret_val)
    {
        find = strchr(st,'\n');
        if(find)
        {
            * find = '\0';
        }else
        {
            while(getchar()!= '\n')
                continue;
        }

    }
    return ret_val;
}

~~~

#### 上述是结构体

* 建立结构声明

  * ~~~c
    struct book{
        char titile [MAXTITLE];
        char author [MAXAUTOL];
        float value;
    };
    ~~~

  * 该声明描述了一个由两个字符数组和一个float 类型变量组成的结构。该声明并未创建实际的数据对象。只描述了该对象由什么组成。

  * ~~~c
    struct book library;//这样主要是声明library是 booK结构体使用这种结构存储数据
    ~~~

  * 结构有两层含义：一层含义是结构布局，另外一层是结构变量。结构体的声明是为了更好的为变量申请空间。

  * ~~~c
    struct book{
        char titile [MAXTITLE];
        char author [MAXAUTOL];
        float value;
    }library;
    //简化声明方式
    ~~~

  * ~~~c
    struct book library｛
        “the pious private and the Devious Damsel”，
        “Renee Vivotte”,
    	1.95
        ｝;//简言之，我们在一对花括号括起来，初始化列表进行初始化，各初始化项用逗号分隔。因此，每个成员变量都可以与自己的实际值匹配。
    ~~~

  * 访问成员变量使用   **点.**  ，例如：library.title 就是访问title的值。

  * c99与c11为结构体提供指定初始化器，其语法与宿主的指定初始化器类似。

  * ~~~c
    struct book surprise = ｛.value = 10.99｝;
    //可以按照任意顺序初始化指定初始化器
    struct book gift{
        .value  = 25.99,
        .author = "James Broadfool",
        .title = "Rue for the Toad"
    };
    //最后一次赋值才是变量实际获取值
    struct book gift{
        .value  = 25.99,
        .author = "James Broadfool",
        0.25
    };
    
    ~~~

  *   结构数组

  * ~~~c
    struct book library [10];
    //结构数组的声明，以上代码主要是声明一盒包含10个book结构体大小的空间的名为library的数组
    ~~~

  * ~~~c 
    library[0].value //就是对第一个变量中的value 访问
    ~~~

  * ~~~c
    library[0].value[0] //就是对第一个变量中的value中的第一元素 访问
    ~~~

  * 结构体可以包含结构体，结构体嵌套

  * ~~~c
    struct addr{
            char country[20];
            char xiancheng[20];
            char street[20];
    };
    struct pmsg{
    	struct addr address；
        char tel[12];
        int age;
    };//个人信息包含着住址，住址可以单独是一个结构体
    ~~~

  * 指向结构的指针

  * ~~~markdown
    * 使用指针指向结构体的好处
    * 第一就是：指向数组的指针比数组本生更容易操控
    * 早期的C语言不可以将结构体作为参数传递给函数，但是结构体指针可以
    * 可以传递结构体也没有传递指针高效
    * 一些用于标书数据结构的结构中包含指向其他结构的指针
    ~~~

  * 声明和初始化结构指针

  * ~~~c
    #defines LEN 20
    struct names{
        char first[LEN];
        char last[LEN]
    }
    struct guy {
        struct names handle;
        char favfood[LEN];
        char job[LEN];
        float income;
    };
    struct guy * him;
    //声明结构指针很简单,类似普通指针的声明。
    ~~~

  * 用指针访问成员

  * ~~~c
    //第一种访问方式使用：->
    him = &barney;
    him->income;
    //这就是访问barney.income
    him = &fellow[0];
    him->income;
    //这就是访问fellow[0].income
    him = &fellow[0];
    //第二种访问方式：fellow[0].income == (*him).income
    
    ~~~

  * 向函数传递结构的信息

  * ~~~c
    //传递结构成员
    #define FUNDLEN 50
    struct funds{
        char bank[FUNDLEN];
        double bankfund;
        char save[FUNDLEN];
        double savefund;
    }
    double sum(double ,double);
    double sum(double x,double y){
        return x+y;
    }
    
    ~~~

  * 传递结构的地址

  * ~~~c
    double sum (const struct funds * money){
        return (money->bankfund +money->savefund);
    }
    //sum（）函数使用指向funds结构指针作为他的参数。把地址传递给函数，使得指针money指向结构变量。然后通过->运算符号获取变量存储的值
    //这种指针传递的方式可以访问其数值
    ~~~

  * 传递结构

  * ~~~c
    double sum(struct funds moolah){
        return (moolah.bankfund + moolah.savefund);
        
    }
    ~~~

* 关于结构指针和结构的选择

  * 指针作为参数主要两个优点：无论如何C语言版本如何都能实现这种参数传递方式；指针传递方式能够更快，不需要创建副本。缺点：无法保护数据。
  * 结构传递：可以保护数据，因为创建副本，这样降低空间利用率和程序效率。

* 符合字面量和结构：

  * 如果只需要一个临时结构值，符合字面量很好用。例如，使用符合字面量创建一个结构作为函数参数或则赋值另外一个结构。

  * 语法：把类型名放在圆括号中，后面紧跟一个花括号阔气初始化列表。

  * ~~~c
    //例如：
    （struct book）｛"the Idoil","Fyodor Dostoyevsky",6.99｝
    ~~~

* 绳索型数据成员

  * ~~~c
    struct flex{
        int count;
        double average;
        double scores[];//伸缩性数组成员
    };
    ~~~

  * ~~~markdown
    * 伸缩性变量必须是最后一个变量
    * 结构中至少有一个成员
    * 伸缩数组的声明类似普通数组，只是它的括号是空的
    ~~~

  * ~~~c
    struct flex * pf;
    pf = malloc(sizeof(struct flex))+5*sizeof(double))//这样保证了结构体实化后存储空间可以报含5个double类型数据
    ~~~

* 匿名结构

  * ~~~c
    struct person{
        int id;
        struct {char first[20],char last[20]};//匿名结构
    }
    ~~~

  