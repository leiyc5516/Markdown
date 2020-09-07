
>下面是一个基本的Date使用：
```
package com.study.fourday;

import java.util.Date;

public class DateUse {
	public static void main(String args[]) {
		Date date =  new Date();
		System.out.println(date);
		//时间格式转换
		
	}
}
```
下面是Java中Date的无参构造
```
 public Date() {
        this(System.currentTimeMillis());
    }
```

下面是Java中Date的有参构造
```
 public Date(long date) {
        fastTime = date;
    }

```
明显：
Date类是一种对long 数据的包装，所Date类中一定提供有，相关数据与long 数据类型的转换
1.将long 数据转换为日期 public Date(long date)//long数据转为Date
2.public long getTime()将日期转换成long 数据类型
```
package com.study.fourday;

import java.util.Date;

public class DateUse {
	public static void main(String args[]) {
		Date date =  new Date();
		System.out.println(date);
		//时间格式转换
		long current = date.getTime();//Date转换成数据long
		current += 8640000;
		System.out.println(new Date(current));//将long 数据类型转换成Date
		
	}
}
```
输出：
Fri Dec 27 10:07:16 CST 2019
Fri Dec 27 12:31:16 CST 2019
long可以保存毫秒数量级。


>在java.text中提供了SimpleDateFormat类可以执行时间的格式转换
年（yyyy）,月（MM），日（dd），时（HH），分（mm）,秒（ss）,毫秒（SSS）
```
package com.study.fourday;

import java.text.SimpleDateFormat;
import java.util.Date;

public class DateUse {
	public static void main(String args[]) {
		Date date =  new Date();
		System.out.println(date);
		//时间格式转换
		long current = date.getTime();//Date转换成数据long
		current += 8640000;
		System.out.println(new Date(current));//将long 数据类型转换成Date
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss:SSS");
		String string = sdf.format(date);
		System.out.println(string);
		
	}
}
```
输出：
Fri Dec 27 10:21:46 CST 2019
Fri Dec 27 12:45:46 CST 2019
2019-12-27 10:21:46:298

相应的：
转换回标准格式：
```
	String string = "1998-3-2 20:13:14:520";
		SimpleDateFormat strDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss:SSS");
		Date date = strDateFormat.parse(string);
		System.out.println(date);
```
