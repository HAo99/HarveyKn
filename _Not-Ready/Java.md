## Java 相关面试题目

#### 1. StringBuffer 和 StringBuilder 有什么区别

首先当然 StringBuffer 和 StringBuilder 他们之间最主要的区别是一个是线程安全的一个不是，然后从细节上分析的话：

相同点：

- StringBuffer 和 StringBuilder 都继承了 AbstractStringBuilder，也就是两个类提供了基本一致的方法接口，核心的功能是提供一个字符串拼接的能力。

  如果不使用这两个类，而使用 `+` 直接进行拼接的话，由于 String 类型是不可变的，会导致常量池建立相当多无用的字符串对象。如果是静态字符串的话，使用 `+` 拼接是完全可行的，编译时候会直接将多个 `+` 拼接的字符串转换成一个拼接后的长字符串。如果字符串需要循环拼接的话，那么会有比较大的性能问题。

  而 StringBuilder 和 StringBuffer 类就是为了解决这个问题而存在的，它们内部维护了一个 byte 数组，拼接等方法只是在操作这个数组而已，只有在调用 toString 的时候，才会将 byte 数组转换成字符串。

- StringBuffer 和 StringBuilder 初始的容量和扩容的机制都是一致的，`append`、`insert`、`delete`、`indexOf` 等方法也都是调用对应的父类方法。

不同点：

- StringBuffer 是在 JDK 1.0 版本的时候就存在了，而 StringBuilder 在 JDK 1.5 版本的时候才有。

- StringBuilder 是非线程安全的，因为不需要考虑线程安全，所以不需要在操作上加锁操作，在单线程的情况下性能要比 StringBuffer 更好。而 StringBuffer 设计是用于线程安全能力的，所以在方法上都加上了 `synchronized` 关键字，来保证并发安全。

- StringBuffer 设置了一个 toString 的缓存，在多次调用 toString 方法时，可以减少 Array.copyOfRange 方法的开销。但很少有人在不修改 StringBuilder 的情况下多次调用 toString 方法，所以这个优化意义不大，之所以有这个区别也是因为历史原因。

  > StringBuffer 是在 1.0 时候存在的，而 StringBuilder 是 1.5 出现的，考虑到修改 StringBuffer 的实现还要重新测试，没有必要动已经稳定的代码。
  >
  > （参考：[Why StringBuffer has a toStringCache while StringBuilder not? -- StackOverflow](https://stackoverflow.com/questions/46294579/why-stringbuffer-has-a-tostringcache-while-stringbuilder-not)）

> 参考：[Difference between StringBuilder and StringBuffer -- StackOverflow](https://stackoverflow.com/questions/355089/difference-between-stringbuilder-and-stringbuffer)

#### 2. HashMap 的底层实现

JDK 中的 HashMap 是由数组、链表、红黑树三种数据结构组成的一个较为复杂的数据结构。在 JDK 1.8 之前，只有数组和链表。

在添加元素的时候，将元素的键（key）通过哈希函数计算出哈希值，这个哈希值确认这个元素要放入数组中的哪个位置。查找的时候也是一样，将键通过哈希函数得到哈希值，也就能够知道元素存储到在数组的哪个位置。

这是因为多次将同一个键进行哈希函数的计算结果必然是相同的，反之则不一定，多个不同的键可以得到同一个哈希值。多个键对应同一个哈希值这种情况叫做哈希冲突，桶的空间越小，哈希冲突的机率则越大。当发生哈希冲突的时候，元素会使用链表的形式存储起来。

如果查询具有哈希冲突的元素的时候，首先找到数组的位置，再依次遍历链表即可。

但是如果同一个桶挂载的链表长度很长的时候，这种情况下 HashMap 的查询效率就不太理想了。这时候可以引入二分搜索树这种数据结构，可以将链表搜素 O(n) 的时间复杂度下降至 O(logn)。但是普通的二分搜索树在插入的时候元素是有序的情况下，会退化成链表，这是我们不想要的。所以 HashMap 使用了一种叫红黑树的高级二分搜索树，能够保持左右子树的元素大致平衡。在 JDK 中，链表在长度大于 8 的时候便会转换成红黑树。

**源码分析：**

上面提到的数组（即位桶）结构在源码中是：

```java
Node<K,V> table;
```

链表结构在源码中是：

```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;       // 存储在该节点的键
    V value;           // 存储在该节点的值
    Node<K,V> next;    // 连接到下一个节点的引用
    // ...
}
```

红黑树结构在源码中是：

```java
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
    TreeNode<K,V> parent;  // red-black tree links
    TreeNode<K,V> left;    // 左节点
    TreeNode<K,V> right;   // 右节点
    TreeNode<K,V> prev;    // needed to unlink next upon deletion
    boolean red;
}
```

HashMap 默认的位桶（`Node<K, V>[] table`）大小为 16，如果需要修改可使用下面这个构造器：

```java
public HashMap(int initialCapacity) { /* ... */}
```

HashMap 默认的扩容因子为 0.75，如需调整可以使用下面这个构造器：

```java
public HashMap(int initialCapacity, float loadFactor) { /* ... */ }
```



#### 3. ConcurrentHashMap 的底层实现









