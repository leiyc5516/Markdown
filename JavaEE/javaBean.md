###javaBean
```
/*
 *对于 javaBean必须要为成员提供get与set方法
 *必须要有默认构造器
 *一般对于具有默认get与set方法成员变量称之为属性
 *其实就算一个属性没有对应成员变量只有get与set方法也是可以的
 */
```



案例
```
public class Person {
	private String name;
	private int age ;
	private String sex;
    private  boolean bool;
	//只有方法无对应的变量也是可以的
    public  boolean isbool(）｛
        return bool；
    ｝
	public String getFather() {
		return "baba";
		
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}

	public Person(String name, int age, String sex) {
		super();
		this.name = name;
		this.age = age;
		this.sex = sex;
	}
	public Person() {
		super();
		
	}	
}
```
![image.png](https://upload-images.jianshu.io/upload_images/14935748-3f3b5a80ce4288a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
BeanInfo info = Introspector.getBeanInfo(类型);
```
![image.png](https://upload-images.jianshu.io/upload_images/14935748-95a06a25faffdef9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


案例1
```
package com.admin.javaBean;
/*
 *对于 javaBean必须要为成员提供get与set方法
 *必须要有默认构造器
 *一般对于具有默认get与set方法成员变量称之为属性
 *其实就算一个属性没有对应成员变量只有get与set方法也是可以的
 */

import java.beans.BeanInfo;
import java.beans.Introspector;

public class Person {
	private String name;
	private int age ;
	private String sex;
	//只有方法无对应的变量也是可以的
	public String getFather() {
		return "baba";
		
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}

	public Person(String name, int age, String sex) {
		super();
		this.name = name;
		this.age = age;
		this.sex = sex;
	}
	public Person() {
		super();
		
	}
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + ", sex=" + sex + "]";
	}
	
	

}
```

```package com.admin.javaBean;

import java.lang.reflect.InvocationTargetException;

import org.apache.commons.beanutils.BeanUtils;

public class Test {
	public static void main( String args[]) throws ClassNotFoundException, InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException, NoSuchMethodException, SecurityException {
		String  classname = "com.admin.javaBean.Person";
		Class clazz = Class.forName(classname);
		Object beanObject = clazz.getDeclaredConstructor().newInstance();//反射获取bean
		
		BeanUtils.setProperty(beanObject, "name","张三");
		BeanUtils.setProperty(beanObject, "age","12");
		BeanUtils.setProperty(beanObject, "sex","男");
		System.out.println(beanObject);
	}

}
```
案例2
```
package com.admin.javaBean;

public class UserInfo {
	private String username;
	private String password;
	@Override
	public String toString() {
		return "UserInfo [username=" + username + ", password=" + password + "]";
	}
	public UserInfo(String username, String password) {
		super();
		this.username = username;
		this.password = password;
	}
	public UserInfo() {
		super();
		// TODO Auto-generated constructor stub
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
		

}
```
```
package com.admin.javaBean;
import java.lang.reflect.InvocationTargetException;
import java.util.HashMap;
import java.util.Map;
import org.apache.commons.beanutils.BeanUtils;

public class Test {
	public static void main( String args[]) throws ClassNotFoundException, InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException, NoSuchMethodException, SecurityException {

		/*
		 * test2把【map封装到一个javabean中,要求map的键与 bean中的属性名称对应才可以封装】
		 */
		Map<String, String>map = new HashMap<String, String>();
		map.put("username", "李四");
		map.put("password", "123");
		UserInfo userInfo = new UserInfo();
		BeanUtils.populate(userInfo, map);
		System.err.println(userInfo);
		System.err.println(userInfo.getPassword());
	}

}
```

![image.png](https://upload-images.jianshu.io/upload_images/14935748-0a96123c7fb0ca9c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
