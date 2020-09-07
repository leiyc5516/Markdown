```
public class Linked <T>{	
	private class Node{
		private T t;
		private Node next;
		public Node(T t,Node next){
			this.t = t;
			this.next = next;
		}
		public Node(T t){
			this(t,null);
		}
	}
	private Node head;    		//头结点
	private int size;			//链表元素个数
	//构造函数
	public Linked(){
		this.head = null;
		this.size = 0;
	}
}
```
[参考](https://blog.csdn.net/weixin_36605200/article/details/88804537)

>待完善
