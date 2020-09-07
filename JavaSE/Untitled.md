# [关于GC(下)：CMS和G1GC的比较](https://www.cnblogs.com/wuyuegb2312/p/11839295.html)

# 简称

- STW —— Stop the World，暂停所有在执行的线程

# 简史

- 2004年Sun实验室第一次发表G1论文
- JDK6U14中第一次作为实验选项引入
- JDK7中开始作为替换CMS的方案
- JDK9中成为默认的垃圾回收器
- JDK10优化，将其fullGC改为并行： [JEP307](http://openjdk.java.net/jeps/307)
- JDK11引入了更新的ZGC，可能会成为G1的潜在替代者

# G1特有数据结构和算法

## Region

堆仍然有新生代(eden、survivor)、老年代的划分，但是不再要求它们是内存连续的。每个区都由多个Region组成。
部分老年代Region存储Humongous对象(即下图的H)，这种对象大小大于等于Region的一半。
![img](https://img2018.cnblogs.com/blog/228024/201911/228024-20191126113635033-542381041.png)

([图片来源-Java Hotspot G1 GC的一些关键技术](https://tech.meituan.com/2016/09/23/g1.html))

## SATB算法

全称Snapshot-At-The-Beginning，起始时活对象的快照。在理解SATB前需要先了解以下知识。

### 三色标记法

CMS和G1的算法都是通过对gc root 进行遍历，并进行三色标记。标记规则为

- 黑色(black): 节点被遍历完成，而且子节点都遍历完成。
- 灰色(gray): 当前正在遍历的节点，而且子节点（即对象的域）还没有遍历。遍历完所有子节点后，将成为黑色
- 白色(white): 还没有遍历到的节点，即灰色节点的子节点。扫描结束仍是白色时会被回收。

并发扫描时，对于白色有两种情况同时发生时，可能会漏标导致被误回收：

- 增加了被黑色引用的关系。
- 被灰色下应用，删除了到它的引用

具体执行过程：https://www.cnblogs.com/javaadu/p/10713956.html

按照R大的说法：CMS的incremental update设计使得它在remark阶段必须重新扫描所有线程栈和整个young gen作为root；G1的SATB设计在remark阶段则只需要扫描剩下的satb_mark_queue。

## RSet

全称Remember Set，记录一个Region里的对象被哪些其他Region引用。
相对应地，有另一种辅助数据结构Collection Set（CSet），它记录了GC要收集的Region集合。GC时只需扫描CSet中各个Rset即可。

![img](https://img2018.cnblogs.com/blog/228024/201911/228024-20191127012638454-1842691132.png)
([Tips for Tuning the Garbage First Garbage Collector](https://www.infoq.com/articles/tuning-tips-G1-GC/))

更详细的访问机制和回收过程这里不再展开，有兴趣可以参考后文引用文献。

## Pause Prediction Model

暂停预测模型，G1根据它计算出的历史数据来预测本次收集需要选择的Region数量，从而尽量满足用户设定的目标停顿时间。
具体算法和公式略，可见[Java Hotspot G1 GC的一些关键技术](https://tech.meituan.com/2016/09/23/g1.html)

# 垃圾回收过程

![img](https://img2018.cnblogs.com/blog/228024/201911/228024-20191126112024509-1350936490.png)
分为以下几步：

- 初始标记（Initial Mark）—— 标记GC root能直接关联的对象（短暂STW）
- 并发标记（Concurrent mark）—— GCRootsTracing，从并发标记中的root遍历，对不可达的对象进行标记，耗时长但可并行
- 最终标记（Final Remark）—— 收集并发标记期间产生的新垃圾（短暂STW）,采用了SATB算法比CMS更快
- 筛选回收（Live Data Counting and Evacuation）—— 对各个Region的回收性价比排序，在保证时间可控的情况下清除失活对象，清除Remember Sets

作为对比，CMS的回收过程
![img](https://img2018.cnblogs.com/blog/228024/201911/228024-20191126113556089-601648311.png)

- 初始标记（CMS Initial Mark）—— 标记GC root能直接关联的对象（短暂STW）
- 并发标记（CMS Concurrent Mark）—— GCRootsTracing，从并发标记中的root遍历，对不可达的对象进行标记
- 重新标记（CMS Remark）—— 修正并发标记期间因为用户操作导致标记发生变更的对象，有STW
- 并发清除（CMS Concurrent Sweep）

# 与CMS相比的优势

1. 并发度更高，充分利用CPU多线程 —— CMS对CPU资源敏感，需要占用25%的线程，如果核数小于4更会占用一半的资源。
2. 整体上是标记-整理(分代)，局部是复制(分Region)，运行期不产生碎片 —— CMS是标记-清除，会产生空间碎片和本次回收期间产生导致本次无法回收的浮动垃圾
3. 可预测的停顿(基于Region)