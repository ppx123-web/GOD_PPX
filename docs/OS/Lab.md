# Lab

## Lab 1

### 关于数据结构

关于使用何种数据结构管理的问题，目前主要有两个想法：

```c
typedef union page {
  struct {
    spinlock_t lock; // 锁，用于串行化分配和并发的 free
    int obj_cnt;     // 页面中已分配的对象数，减少到 0 时回收页面
    int cpu_id;
    page_node list_head; //连接不同page间的节点
    list_head list;  // 属于同一个线程的页面的链表
  };
  struct {
    uint8_t header[HDR_SIZE];
    uint8_t data[PAGE_SIZE - HDR_SIZE];
  } __attribute__((packed));
} page_t;
```

fast path:在每一个page中可以任意分配大小（但是并不好管理，因为数据对齐）
slow path:设计一个大的内存链表，管理所有大内存的分配

可以优化的部分： 将链表的数据存储到后半部分，这样可以在一个page上存最多12KiB的内存

量一种想法：初始化为每一个cpu做一个bitmaps，好处是从16，32，64，128，256，512，1024，2048，4096，8192，分配会有更加的性能，也更加方便对齐，

缺点是链表信息不好存储（其实可以一样放在每一个块的最后面），初始化的工作量比较大,后面比较大的slab需要更加重新定义一个更大的page的结构。

```c
typedef struct page {
  uint8_t data[PAGE_SIZE - HDR_SIZE];
  union {
    struct {
      spinlock_t lock; // 锁，用于串行化分配和并发的 free
      int obj_cnt;     // 页面中已分配的对象数，减少到 0 时回收页面
      int cpu_id;
      int slab_size;
      page_node list_head; //连接不同page间的节点
      list_head list;  // 属于同一个线程的页面的链表
    }
    uint8_t header[HDR_SIZE];
  }
} page_t;
```

暂时还是考虑采用第一种方案

### 方案

#### kmalloc

1. Fast path
  找到当前线程的头页面，开始扫描，寻找是否有空的空间，如果有检查对齐，分配，维护链表及找到的当前页面的链表，其中注意在分配，维护链表对当前链表上锁。
2. Slow path 1
  找到空的页面，注意对齐，要对page上锁，分配新的页面（上锁），然后fast path
3. Slow path 2
  在全局的管理大内存的链表，找位置，注意对齐，维护链表（上锁），分配

#### kfree

1. Fast path
  查找，确认是fast path，上锁，free
  
  如果需要回收页面，对page的链表整个上锁，回收
2. Slow path
  上锁，在全局的管理大内存的链表，找位置，删除。


第一种方法有些麻烦，暂时算是写的差不多了，第二种明显要简单一些，效率也显然要更高一些。考虑重写中。

事实证明，slab要简单的多简单非常多，第一种方法是第二种的1.5倍工作量。

第二种方法重写到AC大概花了12个小时，太难了，中间还git reset了很多次。

我主要卡在了1Mop/s上，通过将一个大锁拆成小锁加各个cpu预留空间，平分内存后则各自有一把锁可以加速来解决。

其实对于锁的问题我没有遇到特别多，测试也很容易找到问题。

然后就是小错误，这个也花了好几个小时的时间，写代码还是要多想，不能光写。
