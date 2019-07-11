---
layout: post
title: 'Java集合结构回顾及HashMap底层原理分析'
date: 2019-01-02
categories: 技术
tags: Java
---

### <font color=DeepSkyBlue>一、集合结构回顾</font>

Java中的集合包含多种数据结构，如链表、队列、哈希表等。从类的继承结构来说，可以分为两大类，一类是继承自Collection接口，这类集合包含List、Set和Queue等集合类。另一类是继承自Map接口，这主要包含了哈希表相关的集合类。

#### <font color=Orange>1、List、Set和Queue</font>

> 注：图中的绿色的虚线代表实现，绿色实线代表接口之间的继承，蓝色实线代表类之间的继承。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225105329474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

1. <font color=Tomato>List</font>：有序可重复，平常我们使用的比较多的List有ArrayList和LinkedList

   <font color=YellowGreen>ArrayList</font>底层是数组，查询效率高，插入和删除效率低

   <font color=YellowGreen>LinkedList</font>底层是链表，查询效率低，插入和删除效率高

   至于Vector，它是ArrayList的线程安全版本

2. <font color=Tomato>Queue</font>：一般可以直接使用LinkedList完成，从上述类图也可以看出，LinkedList继承自Deque，所以LinkedList具有双端队列的功能。PriorityQueue的特点是为每个元素提供一个优先级，优先级高的元素会优先出队列。

3. <font color=Tomato>Set</font>：Set与List的主要区别是Set是不允许元素重复的，而List则可以允许元素重复的。判断元素的重复需要根据对象的hash方法和equals方法来决定。这也是我们通常要为集合中的元素类重写hashCode方法和equals方法的原因。

   对于集合中元素，hashCode值不同的元素一定不相等，但是不相等的元素，hashCode值可能相同。<font color=YellowGreen>HashSet</font>和<font color=YellowGreen>LinkedHashSet</font>的区别在于后者可以保证元素插入集合的元素顺序与输出顺序保持一致。而<font color=YellowGreen>TreeSet</font>的区别在于其排序是按照Comparator来进行排序的，默认情况下按照字符的自然顺序进行升序排列。

4. <font color=Tomato>Iterable</font>：从这个图里面可以看到Collection类继承自Iterable，该接口的作用是提供元素遍历的功能，也就是说所有的集合类（除Map相关的类）都提供元素遍历的功能。Iterable里面包含了Iterator的迭代器，其源码如下，大家如果熟悉迭代器模式的话，应该很容易理解。

   ```java
   public interface Iterator<E> {
       boolean hasNext();
       E next();
       default void remove() {
           throw new UnsupportedOperationException("remove");
       }
       default void forEachRemaining(Consumer<? super E> action) {
           Objects.requireNonNull(action);
           while (hasNext())
               action.accept(next());
       }
   }
   ```

5. <font color=Tomato>Comparable和Comparator</font>

    <font color=YellowGreen>Comparable</font>接口 -> 可比较的

	   实现该接口表示：这个类的实例可以比较大小，可以进行自然排序
	   定义了默认的比较规则
	   其实现类需要实现compareTo()方法
	   compareTo()方法返回正数表示大，负数表示小0表示相等

    <font color=YellowGreen>Comparator</font>接口 -> 比较工具接口

	   用于定义临时比较规则，而不是默认比较规则
	   其实现类需要实现compare()方法
	   Comparable和Comparator都是Java集合框架的成员

#### <font color=Orange>2、Map </font>

> 注：图中的绿色的虚线代表实现，绿色实线代表接口之间的继承，蓝色实线代表类之间的继承。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225105624577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

Map类型的集合最大的优点在于其查找效率比较高，理想情况下可以实现O(1)的时间复杂度。Map中最常用的是<font color=SandyBrown>HashMap</font>，<font color=YellowGreen>LinkedHashMap与HashMap的区别</font>在于前者能够保证插入集合的元素顺序与输出顺序一致。这两者与<font color=YellowGreen>TreeMap</font>的区别在于TreeMap是根据键值进行排序的，当然其底层的实现也有本质的区别，如HashMap底层是一个哈希表，而TreeMap的底层数据结构是一棵树。

<font color=YellowGreen>HashMap与TreeMap的区别</font>，与之前提到的HashSet与TreeSet的区别是一致的， HashSet和TreeSet本质上分别是通过HashMap和TreeMap来实现的，所以它们的区别自然也是相同的。HashTable现在已经很少使用了，与HashMap的主要区别是HashTable是线程安全的，不过由于其效率比较低，所以通常使用HashMap，在多线程环境下，通常用CurrentHashMap来代替。

   > 注：那么什么是时间复杂度呢？

#### <font color=Orange>3、算法复杂度 </font>

算法复杂度分为<font color=Tomato>时间复杂度</font><font color=YellowGreen>(度量算法执行的时间长短)</font>和<font color=Tomato>空间复杂度</font><font color=YellowGreen>(度量算法所需存储空间的大小)</font>。

**时间复杂度**

1. 时间频度
　　一个算法执行所耗费的时间，从理论上是不能算出来的，必须上机运行测试才能知道。但我们不可能也没有必要对每个算法都上机测试，只需知道哪个算法花费的时间多，哪个算法花费的时间少就可以了。并且一个算法花费的时间与算法中语句的执行次数成正比例，哪个算法中语句执行次数多，它花费时间就多。一个算法中的语句执行次数称为语句频度或时间频度。记为T(n)。

2. 分类
　　按数量级递增排列，常见的时间复杂度有：常数阶O(1),对数阶O(log2n),线性阶O(n),线性对数阶O(nlog2n),平方阶O(n2)，立方阶O(n3),...，k次方阶O(nk), 指数阶O(2n) 。随着问题规模n的不断增大，上述时间复杂度不断增大，算法的执行效率越低。

ex：

	​	要在 hash 表中找到一个元素就是 O(1)
	​	要在无序数组中找到一个元素就是 O(n)
	​	访问数组的第 n 个元素是 O(1)
	​	访问链表的第 n 个元素是 O(n)
	​	如果实现中没有循环就是 O(1)
	​	如果实现中有一个循环就是 O(n)

**空间复杂度**

　　与时间复杂度类似，空间复杂度是指算法在计算机内执行时所需存储空间的度量。记作: 　　S(n)=O(f(n)) 　　我们一般所讨论的是除正常占用内存开销外的辅助存储单元规模。

### <font color=DeepSkyBlue>二、HashMap底层原理分析</font>

>  注：以下分析仅为jdk1.8底层实现！

​       HashMap也是我们使用非常多的Collection，它是基于哈希表的 Map 接口的实现，以key-value的形式存在。在HashMap中，key-value总是会当做一个整体来处理，系统会根据hash算法来来计算key-value的存储位置，我们总是可以通过key快速地存、取value。

#### <font color=Orange>1、HashMap定义</font>

​       HashMap实现了Map接口，继承AbstractMap。其中Map接口定义了键映射到值的规则，而AbstractMap类提供 Map 接口的骨干实现，以最大限度地减少实现此接口所需的工作，其实AbstractMap类已经实现了Map。

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {
    // 序列号
    private static final long serialVersionUID = 362498820763181265L;    
    // 默认的初始容量是16
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;   
    // 最大容量
    static final int MAXIMUM_CAPACITY = 1 << 30; 
    // 默认的负载因子
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
    // 当桶(bucket)上的结点数大于这个值时会转成红黑树
    static final int TREEIFY_THRESHOLD = 8; (jdk1.8以后才有)
    // 当桶(bucket)上的结点数小于这个值时树转链表
    static final int UNTREEIFY_THRESHOLD = 6;
    // 桶中结构转化为红黑树对应的table的最小大小
    static final int MIN_TREEIFY_CAPACITY = 64;
    // 存储元素的数组，总是2的幂次倍
    transient Node<k,v>[] table; 
    // 存放具体元素的集
    transient Set<map.entry<k,v>> entrySet;
    // 存放元素的个数，注意这个不等于数组的长度。
    transient int size;
    // 每次扩容和更改map结构的计数器
    transient int modCount;   
    // 临界值 当实际大小(容量*填充因子)超过临界值时，会进行扩容
    int threshold;
    // 负载因子
    final float loadFactor;
}
```

> 注：transient是java语言的关键字，变量修饰符，如果用transient声明一个实例变量，当对象存储时，它的值不需要维持。换句话来说就是，用transient关键字标记的成员变量不参与序列化过程。

#### <font color=Orange>2、HashMap底层结构示意图</font>

<font color=Tomato>Jdk1.7</font>中，HashMap底层基于<font color=Tomato>数组+链表</font>实现，如果entry放的位置在数组中都是同一个位置，链表就会变得很长，链表深度过深将导致效率低下！

因此在<font color=Tomato>Jdk1.8</font>中，为了解决链表深度过深导致的效率低下问题，HashMap底层结构变为<font color=Tomato>数组+链表+红黑树</font>实现。示意图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225105848818.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225105911650.png)

**<font color=YellowGreen>红黑树：</font>**

​      红黑树（Red Black Tree） 是一种自平衡二叉查找树，是在计算机科学中用到的一种数据结构，典型的用途是实现关联数组。

**特点：**

```java
1. 每个节点要么是红色，要么是黑色。
2. 根节点必须是黑色
3. 红色节点不能连续（也即是，红色节点的孩子和父亲都不能是红色）。
4. 对于每个节点，从该点至null（树尾端）的任何路径，都含有相同个数的黑色节点。
```
**注：** 在树的结构发生改变时（插入或者删除操作），往往会破坏上述条件3或条件4，需要通过调整使得查找树重新满足红黑树的条件。如果有删除或者插入节点,使用左旋和右旋;

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225113755888.gif)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225113804643.gif)


> 具体红黑树特征可查看：<https://mp.weixin.qq.com/s/oNmUW-rUbTPPgUbFn9oddQ>

#### <font color=Orange>3、HashMap底层代码解析</font>

HashMap提供了4个构造函数：

 ```java
 HashMap()：构造一个具有默认初始容量 (16) 和默认加载因子 (0.75) 的空 HashMap。

 HashMap(int initialCapacity)：构造一个带指定初始容量和默认加载因子 (0.75) 的空 HashMap。

 HashMap(int initialCapacity, float loadFactor)：构造一个带指定初始容量和加载因子的空 HashMap。

 HashMap(Map<? extends K, ? extends V> m)：传入一个map以构造一个新的map，使用默认加载因子（0.75）。
 ```

&emsp;&emsp;在这里提到了两个参数：初始容量，加载因子。这两个参数是影响HashMap性能的重要参数，其中容量表示哈希表中桶的数量，初始容量是创建哈希表时的容量，加载因子是哈希表在其容量自动增加之前可以达到多满的一种尺度，它衡量的是一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高，反之愈小。对于使用链表法的散列表来说，查找一个元素的平均时间是O(1+a)，因此如果负载因子越大，对空间的利用更充分，然而后果是查找效率的降低；如果负载因子太小，那么散列表的数据将过于稀疏，对空间造成严重浪费。系统默认负载因子为0.75，一般情况下我们是无需修改的。

**<font color=VioletRed>3.1 存值put(K key, V value)</font>**

```java
public V put(K key, V value) {  
        return putVal(hash(key), key, value, false, true);  
    }  
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
		//检测table是否为空，如果为空，则使用扩容函数进行初始化
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
		//如果通过hash值取模得到的桶为空，则直接把新生成的节点放入该桶
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {//以下为该桶不为空的逻辑
            Node<K,V> e; K k;
			//判断桶的第一个元素的key值是否相同（hash值相同，且能equals）
			//如果相同，则返回当前元素（函数末尾进行统一处理）
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)//桶元素采用的是红黑树结构
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {//桶元素采用的是链表结构
                for (int binCount = 0; ; ++binCount) {
					//如果遍历到了链表末端，则直接在链表末端插入新元素
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
						//插入之后，检查是否达到了转成红黑树结构的标准
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
					//如果在遍历过程中，发现了key值相同，则返回当前元素（函数末尾进行统一处理）
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
			//处理相同元素的情况
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
				//如果onlyIfAbsent为ture，则在oldValue为空时才替换
				//否则直接替换
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;//修改次数+1
		//map的size加1，然后判断是否达到了threshold，否则进行扩容
		//threshold由Node[] table的长度及loadFactor控制
        if (++size > threshold)
            resize();
		//执行回调函数
        afterNodeInsertion(evict);
        return null;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225110539276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

① 判断当前桶是否为空，空的就需要初始化（resize 中会判断是否进行初始化）。

② 根据当前 key 的 hashcode 定位到具体的桶中并判断是否为空，为空表明没有 Hash 冲突就直接在当前位置创建一个新桶即可。

③ 如果当前桶有值（ Hash 冲突），那么就要比较当前桶中的 key、key 的 hashcode 与写入的 key 是否相等，相等就赋值给 e,在第 8 步的时候会统一进行赋值及返回。

④ 如果当前桶为红黑树，那就要按照红黑树的方式写入数据。

⑤ 如果是个链表，就需要将当前的 key、value 封装成一个新节点写入到当前桶的后面（形成链表）。

⑥ 接着判断当前链表的大小是否大于预设的阈值，大于时就要转换为红黑树。

⑦ 如果在遍历过程中找到 key 相同时直接退出遍历。

⑧ 如果 e != null 就相当于存在相同的 key,那就需要将值覆盖。

⑨ 最后判断是否需要进行扩容。

**<font color=VioletRed>3.2 取值 get(Object key)</font>**

```java
public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
}
final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
		//如果table不为空，则再进行查询操作
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
			//先检查第一个元素是否key相同
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
				//如果为红黑树结构，则走红黑树的查询逻辑
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {//否则遍历链表
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
}
```

① 首先将 key hash 之后取得所定位的桶。

② 如果桶为空则直接返回 null 。

③ 否则判断桶的第一个位置(有可能是链表、红黑树)的 key 是否为查询的 key，是就直接返回 value。

④ 如果第一个不匹配，则判断它的下一个是红黑树还是链表。

⑤ 红黑树就按照树的查找方式返回值。

⑥ 不然就按照链表的方式遍历匹配返回值。

从这两个核心方法（get/put）可以看出 1.8 中对大链表做了优化，修改为红黑树之后查询效率直接提高到了 O(logn)。

**<font color=VioletRed>3.3 遍历方式</font>**

HashMap 的遍历方式，通常有以下几种：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225110702458.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

强烈建议使用第一种 EntrySet 进行遍历。

第一种可以把 key value 同时取出，第二种还得需要通过 key 取一次 value，效率较低。

**涉及到的问题：**

&emsp;&emsp;put过程是先计算hash然后通过hash与table.length取摸计算index值，然后将key放到table[index]位置，当table[index]已存在其它元素时，会在table[index]位置形成一个链表，将新添加的元素放在table[index]，原来的元素通过Entry的next进行链接，这样以链表形式解决hash冲突问题，当元素数量达到临界值(capactiy*factor)时，则进行扩容，是table数组长度变为table.length*2;

&emsp;&emsp;如果通过hash运算计算出来的index值与数组中某个index值相同，说明此存储地址已经被其他元素所占有，这种现象称之为hash碰撞或hash冲突。 

&emsp;&emsp;HashMap底层采用数组加链表加红黑树的方式，当存储地址index相同的时候，

&emsp;&emsp;判断当前确定的索引位置是否存在相同hashcode和相同key的元素，如果存在相同的hashcode和相同的key的元素，那么新值覆盖原来的旧值，并返回旧值。  

​ &emsp;&emsp;如果存在相同的hashcode，那么他们确定的索引位置就相同，这时判断他们的key是否相同，如果相同，这时就是产生了hash冲突。  

&emsp;&emsp;Hash冲突后，那么HashMap的单个bucket里存储的不是一个 Entry，而是一个 Entry 链。  

&emsp;&emsp;系统只能必须按顺序遍历每个 Entry，直到找到想搜索的 Entry 为止——如果恰好要搜索的 Entry 位于该 Entry 链的最末端（该 Entry 是最早放入该 bucket 中），那系统必须循环到最后才能找到该元素。
 
<hr/>

**<font color=red>总结：</font>**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225112529852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
