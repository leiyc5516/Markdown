>文件如何表示：Java 使用File类进行文件对象的封装。要注意的是，File类只是数据的封装，不具备文件中数据的读写操作，只有流才具备这个功能。他本身是comparebale接口的子类，所以File类是可以排序的。
![Q3.png](https://upload-images.jianshu.io/upload_images/14935748-bfb800b185c4f47c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
File file = new File("./111.txt");
file.createNewFile();
System.out.println(file.isFile());
System.out.println(file.length());
System.out.println(file.canRead());
System.out.println(file.exists());
System.out.println(new Date(file.lastModified()));
System.out.println(file.getPath());
System.out.println(file.getAbsolutePath());
System.out.println(file.delete());
```
![8H.png](https://upload-images.jianshu.io/upload_images/14935748-08a416ef22318c29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
File file = new File("./files");
System.out.println(file.isDirectory());

File files = new File("./abc/def/111");
System.out.println(file.mkdirs());

File file = new File("./doc");
File[] files = file.listFiles();
for(File f : files) {
	System.out.println(f.getName());
}

```

```
//递归删除文件夹里的文件及文件夹本身
package com.langsin.io;

import java.io.File;
import java.io.IOException;

public class Test {
	
	public static void deleteFile(File file) {
		if(file.isFile()) {
			file.delete();
		} else {
			File[] files = file.listFiles();
			for(File f : files) {
				deleteFile(f);
			}
			file.delete();
		}
	}
	
	public static void main(String[] args) throws IOException {
		File file = new File("./abc");
		deleteFile(file);
	}
}
```
[具体参见](https://www.cnblogs.com/cainiao-chuanqi/p/11326624.html)


>建议：在书写文件路径时我们使用File.separator,确保在不同操作系统都可以使用。`
```
File file = new File("d:"+File.separator+"mldn.txt");//表示分割符File.separator
```
>File 文件类的处理流程：程序 → JVM → 操作系统函数 → 文件处理
文件反复的删除和创建有延迟所以最好是别重名。文件创建的前提是父路径首先存在。

>通常文件创建都是先获取父路径，判断父路径存在与否。一般文件父路径判断有：
public String getParent（）返回父路径的String值
另外常用的：public File getParentFile（）判断文件存在与否，再执行创建删除。
案例：
```
package com.study.threeday;

import java.io.File;
import java.io.IOException;

public class FileDemo1 {

	public static void main(String[] args) throws IOException {
		File file = new File("F:"+File.separator+"Demo"+File.separator+"test"+File.separator+"mldn.txt");
		if(!file.getParentFile().exists())
			file.getParentFile().mkdirs();
		if(!file.exists())
			file.createNewFile();
		else {
			System.out.println("【文件已经存在】");
		}
		// TODO Auto-generated method stub

	}

}
```
>文件信息：
获取可读，可写等信息：canRead(),与canWrite()方法
返回文件长度：length()方法，返回字节长度
判断是否是文件：public boolean isFile​()
判断是否是目录：public boolean isDirectory​()
文件最后一次修改时间：public long lastModified​()
列出目录全部文件：public File[] listFiles​()


>层次化列出目录：
案例：
```
package com.study.threeday;

import java.io.File;

public class FileDirDemo {
	public static void main(String args[]) {
		File file = new File("F:"+File.separator+"Demo");
		listDir(file);
		
	}
	public static void listDir(File file) {
		if (file.isDirectory()) {//判断改路径下是否为目录
			File results[] = file.listFiles();//获取文件
			if (results != null) {
				for (int i = 0; i < results.length; i++) {
					listDir(results[i]);
				}
				
			}
			
		}
			System.out.println(file);
	}

}

```

>**文件批量更名**：public boolean renameTo​(File dest)
案例：
package com.study.threeday;

import java.io.File;

public class FileRenameDemo {
	public static void main(String args[]) {
		File file = new File("F:"+File.separator+"Demo");
		long start = System.currentTimeMillis();//开始时间
		renameDir(file);
		long end = System.currentTimeMillis();//结束时间
		System.out.println("本次操作时间" +(end - start));//
	}
	public static void renameDir(File file) {
		if (file.isDirectory()) {//判断改路径下是否为目录
			File results[] = file.listFiles();//获取文件夹
			if (results!= null) {//递归访问
				for (int i = 0; i < results.length; i++) {
					renameDir(results[i]);
				}
			}
		}
		else {
			if (file.isFile()) {
				String filename =null;
				if(file.getName().contains("."))
					filename = file.getName().substring(0,file.getName().lastIndexOf("."))+".txt";
				//截取最后一个点前面的名字，直接加上.txt
				else {
					filename = file.getName();
				}
				File newfile =new File(file.getParent(),filename);//产生新文件
				file.renameTo(newfile);//覆盖原文件
			}
		}
	}
}
```


