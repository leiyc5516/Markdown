>java 能够使用如此多的开源框架最大的受益来源就是反射机制：
所谓的正：但是用一个类的时候，程序先导入包，然后根据对象的实例化，并且依靠调用
所类中的方法
所谓的反：有对象，根据实例化对象反推出其类型
正向操作：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-c372e414af7737e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
反向操作
获取类信息：public final [Class](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/Class.html "class in java.lang")<?> getClass()帮助使用者找到根源
![image.png](https://upload-images.jianshu.io/upload_images/14935748-e29e2819fe88e559.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
反射当中核心操作都是通过反射类找到Class类对象找到展开的，Class类是反射根源类，但是这个类，获取其实例化对象可以通过三种方式获得。
【Object类支持】Object类可以实例化Class对象通过public final [Class](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/Class.html "class in java.lang")<?> getClass()帮助使用者找到根源
![image.png](https://upload-images.jianshu.io/upload_images/14935748-3f08b3249f8c0dbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【JVM支持】采用"类.class"的形式实例化
需要导入包
![image.png](https://upload-images.jianshu.io/upload_images/14935748-fb8db7bf91b77c71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【Class类支持】在Class类里面提供有一个static方法
不需要导入包
public static [Class](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/Class.html "class in java.lang")<?> forName​([String](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/String.html "class in java.lang") className) throws [ClassNotFoundException](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/ClassNotFoundException.html "class in java.lang")
![image.png](https://upload-images.jianshu.io/upload_images/14935748-47eec364364d926a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>[反射机制详解](https://blog.csdn.net/a745233700/article/details/82893076)

