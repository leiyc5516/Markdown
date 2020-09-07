### 第二十七章，Spring的注解编程

#### 1.什么是注解编程

~~~ java 
注解编程主要是指的在类和方法上加上特定的注解，完成特定的开发

@Compotent
public class xxx(){}

注解编程的主要原因：
    *注解编程变得非常方便，代码简介
    *Spring开发的一个潮流
~~~

#### 2.注解的作用

* 替换xml这种配置形式简化配置
* 替换接口，实现调用双方契约性
  * 通过注解的方式，在调用者和提供者之间达成约定，进而达成功能调用

![image-20200821220613138](E:\Markdown\Spring\Spring5学习\image-20200821220613138.png)

#### 3.Spring发展注解的历程

Spring2.x开始支持注解功能：简化xml的书写

Spring3.x希望彻底替换xml

Spring4.x产生了Springboot ：Spring开始提倡使用注解编程

Spring5.x希望用注解开发

#### 4.Spring注解开发问题

Spring中框架配置注解，如果对注解不满意可以使用Spring注解覆盖



