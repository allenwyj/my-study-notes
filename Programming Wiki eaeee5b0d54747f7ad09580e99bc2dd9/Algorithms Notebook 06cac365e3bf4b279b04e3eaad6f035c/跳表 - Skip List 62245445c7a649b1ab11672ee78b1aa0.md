# 跳表 - Skip List

![%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled.png](%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled.png)

- 跳表是基于链表的衍生物。
- 跳表只能用于链表中的元素有序的情况
- 跳表对标的是平衡树 - 即二叉搜索树中的平衡树二分查找
- 现实中，跳表中的跳过的元素个数可能会不工整（由于增加/删除）。
- 维护成本相对较高，增加/删除一个元素的时候，需要更新所有索引
- **跳表是用空间来换时间。**

## 如何给有序的链表加速

![%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%201.png](%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%201.png)

- 一维的数据结构想要加速一半采用升维 - 即二维

### 添加第一级索引

![%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%202.png](%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%202.png)

- 此处采用的是添加多一个索引，指针指向node.next.next
- 如果在第一级索引中找到需要的node，就停止搜索。如果在第一级索引中没找到，但是发现在两个索引之间，就从第一级索引下降到原始链表中两个索引对应的node的之间，一个一个开始搜索。

### 添加第二级索引

- 进一步提高链表查找效率

![%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%203.png](%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%203.png)

### 添加多级索引

![%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%204.png](%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%204.png)

### 跳表的时间复杂度

![%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%205.png](%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%205.png)

### 跳表的空间复杂度

![%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%206.png](%E8%B7%B3%E8%A1%A8%20-%20Skip%20List%2062245445c7a649b1ab11672ee78b1aa0/Untitled%206.png)

[力扣](https://leetcode-cn.com/problems/lru-cache/)

[为啥 redis 使用跳表(skiplist)而不是使用 red-black？](https://www.zhihu.com/question/20202931)

[跳跃表 - Redis 设计与实现](https://redisbook.readthedocs.io/en/latest/internal-datastruct/skiplist.html)