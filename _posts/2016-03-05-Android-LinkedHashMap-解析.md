---
author: zhangman523
comments: true
date: 2016-03-05 08:32:31+00:00
layout: post
slug: Android LinkedHashMap 解析
title: Android LinkedHashMap 解析
tags:
- Android
categories:
- Android 笔记
---

### LinkedHashMap 概述

`LinkedHashMap` 是哈希表和链表的Map实现，具有可预测的迭代次序。`LinkedHashMap` 允许 key和value 为null。`LinkedHashMap` 和`HashMap`的不同之处在于 `LinkedHashMap` 维护了一个双向链表，运行于所有的条目。该链表定义了迭代排序，通常是定义了插入顺序。

>注意: 如果将key 重新插入map中 则插入顺序不受影响（如果`m.containsKey（k)`在调用之前立即返回true，则调用`m.put（k，v）`时，将键k重新插入到映射m中）

### LinkedHashMap 的实现

对于LinkedHashMap 继承了HashMap 底层使用哈希表与双向链表来保存所有元素。 操作基本和HashMap 一样。
{% highlight java %}
public class LinkedHashMap<K,V>
    extends HashMap<K,V>
    implements Map<K,V>
{
	....
}
{% endhighlight %}

#### 构造方法

`LinkedHashMap` 有五个构造方法

- `LinkedHashMap()` 默认构造方法 初始化一个空的插入顺序的LinkedHashMap `capacity (16)`和 `load factor (0.75)`
{% highlight java linenos %}
/**
 * Constructs an empty insertion-ordered <tt>LinkedHashMap</tt> instance
 * with the default initial capacity (16) and load factor (0.75).
 */
public LinkedHashMap() {
    super();
    accessOrder = false;
}
{% endhighlight %}
- `LinkedHashMap(int initialCapacity)` 根据初始容量`initialCapacity` 和默认0.75 `load factor(0.75)`的负载系数初始化一个空的插入顺序的LinkedHashMap
{% highlight java linenos %}
/**
 * Constructs an empty insertion-ordered <tt>LinkedHashMap</tt> instance
 * with the specified initial capacity and a default load factor (0.75).
 *
 * @param  initialCapacity the initial capacity
 * @throws IllegalArgumentException if the initial capacity is negative
 */
public LinkedHashMap(int initialCapacity) {
    super(initialCapacity);
    accessOrder = false;
}
{% endhighlight %}
- `LinkedHashMap(int initialCapacity, float loadFactor)` 根据初始容量`initialCapacity`和负载系数`loadFactor`初始化一个空的插入顺序的LinkedHashMap
{% highlight java linenos %}
/**
 * Constructs an empty insertion-ordered <tt>LinkedHashMap</tt> instance
 * with the specified initial capacity and load factor.
 *
 * @param  initialCapacity the initial capacity
 * @param  loadFactor      the load factor
 * @throws IllegalArgumentException if the initial capacity is negative
 *         or the load factor is nonpositive
 */
public LinkedHashMap(int initialCapacity, float loadFactor) {
    super(initialCapacity, loadFactor);
    accessOrder = false;
}
{% endhighlight %}
- `LinkedHashMap(Map<? extends K, ? extends V> m)` 根据传入的map 初始化一个和map 一样插入顺序的LinkedHashMap
{% highlight java linenos %}
/**
 * Constructs an insertion-ordered <tt>LinkedHashMap</tt> instance with
 * the same mappings as the specified map.  The <tt>LinkedHashMap</tt>
 * instance is created with a default load factor (0.75) and an initial
 * capacity sufficient to hold the mappings in the specified map.
 *
 * @param  m the map whose mappings are to be placed in this map
 * @throws NullPointerException if the specified map is null
 */
public LinkedHashMap(Map<? extends K, ? extends V> m) {
    super();
    accessOrder = false;
    putMapEntries(m, false);
}
{% endhighlight %}
- `LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder)` 根据初始容量`initialCapacity`、负载系数`loadFactor` 和 排序方式`accessOrder`(`true` 为 访问顺序 `false`为插入顺序) 初始化一个空的LinkedHashMap
{% highlight java linenos %}
/**
 * Constructs an empty <tt>LinkedHashMap</tt> instance with the
 * specified initial capacity, load factor and ordering mode.
 *
 * @param  initialCapacity the initial capacity
 * @param  loadFactor      the load factor
 * @param  accessOrder     the ordering mode - <tt>true</tt> for
 *         access-order, <tt>false</tt> for insertion-order
 * @throws IllegalArgumentException if the initial capacity is negative
 *         or the load factor is nonpositive
 */
public LinkedHashMap(int initialCapacity,
                     float loadFactor,
                     boolean accessOrder) {
    super(initialCapacity, loadFactor);
    this.accessOrder = accessOrder;
}
{% endhighlight %}


#### 存取
- `LinkedHashMap` 重写了`HashMap` 的 `public V get(Object key)` 方法 当排序方式为 访问顺序时（`accessOrder 为 true`）会把访问的节点放到队尾
{% highlight java linenos %}
public V get(Object key) {
  	Node<K,V> e;
  	if ((e = getNode(hash(key), key)) == null)
  	    return null;
  	if (accessOrder)//如果是访问顺序是 则进行排序
  	    afterNodeAccess(e);
  	return e.value;
}
{% endhighlight %}
`afterNodeAccess(e)` 将访问的节点放到队尾 方法具体实现
{% highlight java linenos %}
void afterNodeAccess(Node<K,V> e) { // move node to last
    LinkedHashMap.Entry<K,V> last;
    if (accessOrder && (last = tail) != e) {
        LinkedHashMap.Entry<K,V> p =
            (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
        p.after = null;
        if (b == null)
            head = a;
        else
            b.after = a;
        if (a != null)
            a.before = b;
        else
            last = b;
        if (last == null)
            head = p;
        else {
            p.before = last;
            last.after = p;
        }
        tail = p;
        ++modCount;
    }
}
{% endhighlight %}
- `HashMap` 的put 方法
{% highlight java linenos %}
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
{% endhighlight %}
然后我们看看`putVal`方法
{% highlight java linenos %}
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
            boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
                }
            }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);//这个方法在LinkedHashMap中实现
    return null;
}
{% endhighlight %}
`afterNodeInsertion `的实现
{% highlight java linenos %}
void afterNodeInsertion(boolean evict) { // possibly remove eldest
    LinkedHashMap.Entry<K,V> first;
    if (evict && (first = head) != null && removeEldestEntry(first)) {
        K key = first.key;
        removeNode(hash(key), key, null, false, true);
    }
}
{% endhighlight %}
这个方法可能删除最少使用的元素

### 具体使用实例
- 排序方法为 访问顺序即
{% highlight java linenos %}
public static void main(String[] args) {
    LinkedHashMap<Integer, String> linkedHashMap = new LinkedHashMap<Integer, String>(0, 0.75f, true);
    linkedHashMap.put(0, "张三");
    linkedHashMap.put(1, "李四");
    linkedHashMap.put(2, "王二");
    linkedHashMap.put(3, "麻子");
    linkedHashMap.get(1);//访问之前顺序是插入顺序
    for (Map.Entry<Integer, String> entry : linkedHashMap.entrySet()) {
        System.out.println(" key: " + entry.getKey() + " value:" + entry.getValue());
    }
}
{% endhighlight %}
输出结果
{% highlight shell %}
 key: 0 value:张三
 key: 2 value:王二
 key: 3 value:麻子
 key: 1 value:李四
{% endhighlight %}
可以看到 输出的顺序 key=1 的在队尾
{% highlight java linenos %}
 LinkedHashMap<Integer, String> insertLinkedMap = new LinkedHashMap<Integer, String>(0, 0.75f, false);
    insertLinkedMap.put(1, "李四");
    insertLinkedMap.put(0, "张三");
    insertLinkedMap.put(2, "王二");
    insertLinkedMap.put(3, "麻子");
    insertLinkedMap.get(1);
    for (Map.Entry<Integer, String> entry : insertLinkedMap.entrySet()) {
        System.out.println(" key: " + entry.getKey() + " value:" + entry.getValue());
    }
{% endhighlight %}
输出结果
{% highlight shell %}
 key: 1 value:李四
 key: 0 value:张三
 key: 2 value:王二
 key: 3 value:麻子
{% endhighlight %}
输出顺序不会变化。

暂时分析到这，以后继续补充。