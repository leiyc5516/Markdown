### 基本方法

```
	ThreadLocal<String> tl = new ThreadLocal<String>();
		tl.set("Hello");
		tl.set("world");//设置
		String s = tl.get();//获取数据
		tl.remove();//移除数据
```
#### 内部实现：
使用map<Thread,T>当前线程做key，T泛型（自己设置）做值
保证了每个线程数据独立性

#### ThreadLocal通常用在类的成员变量，多个线程访问他的时候每个线程都有自己的副本互不干扰
