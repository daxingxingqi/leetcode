NB：所有代码仅作学习用途，如有侵权遂删。
# 1. 复杂度分析（上）-时间、空间复杂度分析

## 为什么需要复杂度分析？
- 测试结果非常依赖测试环境
  - i3cpu和i9cpu
- 测试结果受数据规模影响很大
  - 数据本来就有序，就无法测试
 ## 大O复杂度表示法
 T(n) = O(f(n))
 - 下面代码的执行时间为 2n+2,表示为T(n)=O(2n+2),这个公式的意义为T(n)与f(n)成正比，在这里表示为2n+2。其中O表示正比，大O表示时间复杂度，具体代表代码执行时间随数据规模增长的变化趋势，也叫渐进时间复杂度。
 ```
 int cal(int){
   int sum = 0;
   int i = 1;
   for (; i<= n; ++i) {
   sum = sum + i:
   }
   return sum:
 }
 ```
## 时间复杂度分析
- 1.只关心循环执行次数最多的一段代码
  - 下面代码T（n) = O(n),虽然具体复杂度为2n+2,但是O之关心执行次数最多的n
```
 int cal(int){
   int sum = 0;
   int i = 1;
   for (; i<= n; ++i) {
   sum = sum + i:
   }
   return sum:
 }
 ```
- 2.加法法则：总复杂度等于量级最大的那段代码的复杂度
- 3.乘法法则： 嵌套代码的复杂度等于嵌套内外带买复杂度的乘积
- 4.几种常见时间复杂度。
  - O(1)
  - O(logn)
 
  看下面代码，当i=2^x = n时，程序停止，那么程序运行了log(n) = x次
 ```
  i =1;
  while (i<=n){
   i = i*2;
   }
  ```
  - O(n)
  - O(nlogn)
  - O(n^2)...O(n^k)
  - O(2^n)
  - O(n!)
 
 ## 时间复杂度表示法
 比较简单，在这里不再赘述

# 2. 复杂度分析（下）：浅析最好、最坏、平均、均摊时间复杂度

## 最好、最坏情况时间复杂度
下面这段代码的含义是找到变量x的位置，找不到返回-1，这段代码的复杂度为O(n)
``` java
int find(int[] array, int n, int x){
  int i = 0；
  int pos = -1;
  for (; i < n; ++i {
    if (array[i] == x) pos = i;
   }
   return pos:
 }
```
上面的代码非常低效,下面代码做了修改。经过修改后，代码的时间复杂度还是O(n)。在不同情况下，比如第一个就是我们要找的x,那么时间复杂度就是O(1),如果碰巧是最后一个那么就是O(n)。所以为了解决这个问题，引入三个概念，最好情况时间复杂度，最坏情况时间复杂度和平均情况时间复杂度。
``` java
int find(int[] array, int n, int x){
  int i = 0；
  int pos = -1;
  for (; i < n; ++i {
    if (array[i] == x) 『
    pos = i;
    break;
    }
   }
   return pos:
 }
 ```
 ## 平均情况时间复杂度
 借助刚刚查找变量的知识，查找x有n+1种情况：在前n个中和不在数组中。加在一起，再除以情况个数：1+2+...n/(n+1) = n(n+3)/2(n+1)。这个时候时间复杂度还是O(n)。但是这里计算出了问题，因为每种情况出现的概率不一样，假设x出现在数组和不出现数组的概率为1/2,每个位置出现的概率是1/n，那么加权平均值就是1*1/2n + 2*2/2n ... n*1/2n + n*1/2 =(3n+1)/4这时候时间复杂度还是O(n)
 
 ## 均摊时间复杂度
 
``` java
int [] array  = new int[n];
int count = 0;

void insert(int val) {
  if  (count == array.length){
      int sum = 0;
      for (int i =0; i<array.length; ++i){
          sum = sum + array[i];
      }
      array[0] = sum;
      count = 1;
     }
   array[count] = val;
   ++count;
 }       
```

# 3. 数组：为什么很多编程语言中数组都从0开始编号
为什么数组要从0开始编号，而不是从1开始编号吗？
数组array-0下标表示偏移0也就是首位，k表示偏移k位。如果是以1开始则，
``` 
a[i]_address = base_address + （i-1）*data_type_size
```
## **如何实现随机访问？**

**数组：Array 是一种线性表数据结构。它用一组连续的内存空间，来存储一组具有相同类型的数据**

第一是线性表（linear list），线性表就是数据排成像一条线一样的结构。每个线性表上的数据最多只有前后两个方向。其实除了数组，链表、队列、栈等也是线性表结构。
<div align=center><img width="550" src=resource/1.jpg></div>

而与它对立的概念是**非线性列表**，比如二叉树、堆、图等。之所以叫非线性，是因为，在非线性表中，数据之间不是简单的前后关系。
<div align=center><img width="550" src=resource/2.jpg></div>

第二个是**连续的内存空间和相同类型的数据。**正是因为这两个限制限制，它才有了杀手锏的特性-随机访问。但是这也让很多操作变得非常低效，比如想要在数组中删除、插入一个数据，为了保证连续性，就需要做大量的数据搬运工作。

**数据访问**
<div align=center><img width="550" src=resource/3.jpg></div>
我们拿一个长度为10的int类型数组 int[] a = new int[10], 在这个图中，计算机给数组a[10]分配了一块连续内存空间1000~1039，其中，内存块的首地址为 base_address = 1000。

我们知道，计算机会给每个内存单元分配一个地址，计算机通过地址来访问内存中的数据。当计算机需要随时访问数组中的某个元素时，它会首先通过羡慕的寻址公式，计算出该元素的内存地址：

``` 
a[i]_address = base_address + i*data_type_size
```
NB:这里纠正一个错误，当问到数组和链表的区别，很多人都会回答说，‘链表适合插入、删除，时间复杂度是O(1)；数组适合查找，查找的时间复杂度为O(1)。实际上这种表述是不准确的。数组是适合查找，但是查找的复杂度并不是O(1),用二分法查找时间复杂度也是O(logn)。所以正确的表述应该是，数组支持随机访问，根据下标随机访问的时间复杂度为O(1)。

## 低效的插入和删除

**插入**

假设数组的长度为n,现在，如果我们要将一个数据插入到数组中第K个位置。为了把第k个位置腾出来，我们需要把k~n这部分的元素都往后面挪一位。如果在数据开始插入，时间复杂度为O（n），如果在最后为O（1）。所以平均时间复杂度为（1+2+3+4+5+6+...）/n = O（n）。

但是如果数组没有任何规律，为了避免大规模数据搬移，直接将第k位的数据放到最好，把新数据放到k。
<div align=center><img width="550" src=resource/4.jpg></div>
这样时间复杂度就会降到O（1）。

**删除**

和插入类似，也需要搬移数据，不然就会出现空洞，内存就不连续了。情况和插入类似，在这里就不赘述了，平均时间复杂度为O(n)。

为了避免多次搬移数据，在删除多个数据时，可以先把要删除的数据记录下来，当内存满了时，就义词触发删除操作，这样就大大减少了删除操作导致的数据搬移。

## 警惕数组的访问越界问题
c语言不存在访问越界，而java等其他语言有访问越界检查。

## 容器能否代替数组？

看情况
# 4. 链表（linked list)（上）：如何实现LRU缓存淘汰算法？
经典链表应用场景-LRU淘汰算法。缓存是一种提高数据读取性能的技术，在硬件设计、软件开发中都有着非常广泛的应用，比如常见的CPU缓存、数据库缓存、浏览器缓存等。缓存的大小有限，当缓存被用满时，哪些数据应该被清理出去，哪些数据应该被保留？这就需要缓存淘汰策略来决定。常见的策略有三种：先进先出策略FIFO（First In, First Out）、最少使用策略LFU（Least Frequently Used）、最近最少使用策略LRU（Least Recently Used）

## 五花八门的链表结构

**数组和链表对比**

- 底层的存储结构

  - 数组：数组需要一块连续的内存空间，对内存的要求比较高。如果我们申请一个100MB大小的数组，当内存中没有连续的、足够大的内存空间时，即便内存的剩余空间大于100MB，也会申请失败。
  
  - 链表：而链表则恰恰相反，它通过指针将一组零散的内存块串联起来，如果申请的是100MB不连续的内存则没有问题。
<div align=center><img width="550" src=resource/5.jpg></div>

**链表的类型**

-单链表：链表通过指针将一组零散的内存块连在一起。其中，内存块被称为链表的结点。为了将所有的节点串起来，每个链表的结点除了存储数据之外，还需要记录链上的下一个结点的地址。如图所示，我们把这个记录下个结点地址的指针称之为后继指针next。其中，头结点记录链表的基地址，尾结点指向一个空地址 NULL。插入和删除结点并不需要像数组那样进行数据的迁移，而只需要删除地址或者添加地址就行,在这里链表添加删除的复杂度为O（1）。但是凡事有利就有弊，链表想要访问第k个元素，就没有数组那么高效了。因为链表的数据不是连续存储的，所以无法像数组那样，根据首地址和下标，通过寻址方式就能直接计算出对应的内存。链表需要根据指针一个结点一个结点的依次遍历。所以链表的随机访问需要O（n）。
<div align=center><img width="550" src=resource/6.jpg></div>
<div align=center><img width="550" src=resource/7.jpg></div>

-循环链表：循环链表和单链表的区别在于尾结点。单链表的尾结点指向空地址，而循环链表的尾结点指向头结点。
<div align=center><img width="550" src=resource/8.jpg></div>

-双向链表：支持两个方向：每个结点不止有一个后继指针next和还有一个前趋指针prev。从图中可以看到，双向链表要比单向链表占用更多的空间。
<div align=center><img width="550" src=resource/9.jpg></div>
 - 删除操作：
   - 删除结点中“值等于某个给定值”，删除操作虽然为O（1）,但是查找的时间为O（n）。所以删除给定值，需要复杂度为O（n)的时间。
   - 删除给定指针指向的结点，我们已经找到了结点，但是要删除某个结点q，其前驱点需要知道，而单链表不支持获取前驱结点，所以单链表还是需要从头遍历整个链表，直到p->next=q，说明p是q的前驱点。所以在这种情况下，双向链表就比较有优势。

**空间换时间**：空间复杂度换取时间复杂度。

**时间换空间**：时间复杂度换取空间复杂度。
- 缓存：每次遍历硬盘查询数据会非常耗时间，如果实现把数据存在缓存中，虽然比较消耗内存，但是比较省时间。

# 链表VS数组
我举一个稍微极端的例子。如果我们用 ArrayList 存储了了 1GB 大小的数据，这个时候已经没有空闲空间了，当我们再插入数据的时候，ArrayList 会申请一个 1.5GB 大小的存储空间，并且把原来那 1GB 的数据拷贝到新申请的空间上。听起来是不是就很耗时？

除此之外，如果你的代码对内存的使用非常苛刻，那数组就更适合你。因为链表中的每个结点都需要消耗额外的存储空间去存储一份指向下一个结点的指针，所以内存消耗会翻倍。而且，对链表进行频繁的插入、删除操作，还会导致频繁的内存申请和释放，容易造成内存碎片，如果是 Java 语言，就有可能会导致频繁的 GC（Garbage Collection，垃圾回收）。



<div align=center><img width="550" src=resource/10.jpg></div>
5. 
