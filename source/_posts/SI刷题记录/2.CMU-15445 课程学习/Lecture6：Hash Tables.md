![Exported image](Exported%20image%2020250328133952-0.png)  
![CMU 15-445/645 DATABASE SYSTEMS Hash Tables Query Planning Operator Execution . Access Methods— Buffer Pool Manager Disk Manager DBMS pages DBMS — DBMS • Hash Tables • Trees DBMS , • Internal Meta-data • Core Data Storage • Temporary Data Structures • Table Indexes • Data Organization: memory/pages 9, • Concurrency: Hash Tables Hash Table associative array/dictionary ADT MIT 6.006 Dictionary Problem & Implementation. Hash Table • Hash Function: How to map a large key space into a smaller domain Trade-off between being fast vs. collision rate • Hashing Scheme: How to handle key collisions after hashing Trade-off between allocating a large hash table vs. additional instructions to find/insert keys Hash Functions DBMS Hash Function (cryptographic) n, collision DBMS Hash Functions = B. id MurmurHash (2008) • Google CityHash (2011) • Google FarmHash (2014) • CLHash (2016) Hashing Scheme Linear Probe Hashing Linear Probe Hashing MIT 6.006 keys value öIäJßRj, 1. Separate Linked List 2. Redundant Keys XYZ ABC Value Lists valuel value2 value3 valuel value2 XYZ I valuel ABC I valuel XYZ I value2 XYZ I value3 ABC I value2 Hash Table table collision comparisons, Robin Hood Hashing Robin Hood Hashing Linear Probe Hashing 7ßhlE Linear Probe Hashing slot, Cuckoo Hashing Hash Tables Hash Join SELECT A. id FROM A, WHERE A. id A. id ORDER BY A. id A. id A. id= A, B Hash Table Dynamic Hash Tables . id 5 Static Hash Tables Dynamic Hash Tables Chained Hashing, Extendible Hashing Linear Hashingo Chained Hashing Chained Hashing Dynamic Hash Tables fi HelloWorId Ä!, key bucket, Buckets hash(key) Hash Table O(n) Extendible Hashing Extendible Hashing Ole. 10... 11... global øø„. 11 m global 11m global øoø.„ 010.„ 100.„ 110.„ 001.„ 011m 101.„ 111.„ GIE MELLON IASE GROUP øøø.„ 010.„ 100.„ 110.„ 001.„ 011.„ 111.„ IEGIE MELLON GROUP Linear Hashing , —it rehash, 0001 0... 01 1 0... 10101... 10011... 10111. 0001 0... 01110... 10101... 10011m 10111... 11010m 0001 0... 01110... 10011... 10101... 11010... 0001 0... 01 1 10... 1 0011.„ 101 1 0100... 101 11... 11010... — I bucket, 0001 0... 01 10... 10101 ... 1001 1 1010... local local local local local local Find A hash(A) Insert B hash(B) = Find A hash(A) Insert B hash(B) Insert C hash(C) - Find A hash(A) Insert B hash(B) Insert C hash(C) Find A hash(A) Insert B hash(B) Insert C hash(C) - local local local 0111 0... -10111... 01 øø„. = 01110... -10111... 10100... CMU 15445/645 (Fall 201 -10111... CMU 15-445/645 (Fall — I bucket Bij, bucket Hash Tables 0(1) DBMS table index fiü}EÆtö, table index B+ Treeo slides, video Previous Buffer Pools Next Tree Indexes ](Exported%20image%2020250328133954-1.png)
  2. 查询规划
3. 作符执行
4. 访问方法
5. 缓冲池管理器
6. 盘管理器

内部元数据

核心数据

临时的数据结构

表索引（很少用到 Hash，大都是用 B-tree，下节介绍）

### Hash Table 简介
 
**Hash Table（哈希表）** 是一种高效的数据结构，用于存储键值对。它通过哈希函数将键映射到数组的特定位置，从而实现快速查找、插入和删除操作。
 
### 核心概念
 
1. **哈希函数**：将键转换为数组索引的函数。  
2. **数组**：存储键值对的数据结构。  
3. **冲突处理**：当不同键映射到同一索引时，常用的解决方法有链地址法和开放寻址法。
 
### 举例说明
 
假设我们有一个哈希表，用于存储学生姓名和成绩：
 
1. **插入数据**：  
- 插入 ("Alice", 90)  
- 插入 ("Bob", 85)  
- 插入 ("Charlie", 95)
 
2. **哈希函数**：  
- 假设哈希函数将姓名转换为索引：  
- "Alice" → 1  
- "Bob" → 2  
- "Charlie" → 3
 
3. **存储结果**：  
- 数组内容：  
- 索引 1: ("Alice", 90)  
- 索引 2: ("Bob", 85)  
- 索引 3: ("Charlie", 95)
 
4. **查找数据**：  
- 查找 "Bob" 的成绩：  
- 哈希函数计算 "Bob" → 2  
- 直接访问索引 2，得到 85
 
5. **冲突处理**：  
- 如果插入 ("David", 88) 且哈希函数计算 "David" → 2：  
- 使用链地址法，索引 2 存储链表：[("Bob", 85), ("David", 88)]
 
### 总结
 
哈希表通过哈希函数快速定位数据，平均时间复杂度为 O(1)。冲突处理机制确保数据存储的完整性。

取模是最简单的一个 Hash 函数

速度和碰撞率之间的权衡

哈西的方案

将大的键空间映射到较小的域中。

- 处理哈希后的键冲突。
- **举例**：链地址法（Chaining）和开放寻址法（Open Addressing）。

需要在**分配大的哈希表**和**额外的查找/插入指令**之间进行权衡。哈希表越大，冲突越少，但占用内存更多；哈希表较小，冲突较多，但节省内存。

1.送进来一个数据，出来一个数字。因为哈希表往往是一个数组然后哈希函数计算出一个数字指向哈西表的数组位置。
 
2.不要用加密用的 Hash 函数（如 sha256），因为其只起验证作用，而反着算推不出原来数据。

Hash 函数的性能对比

![HASH FUNCTION BENCHMARK Intel Core i7-8700K @ 3.70GHz —crc64 28000 21000 7000 Source: FredriKWidtunO *CMU-DB —MurmurHash3 32 51 101 —CityHash 128 151 —FarmHash 192 201 —XXHash3 251 Key Size (bytes) ](Exported%20image%2020250328134003-2.png)

**开放地址 Hash****：**（如果碰撞的就存到下一个槽里，对所有的 key 都是开放的）

**Static hash table：静态哈西表**

**动态哈西表**

冗余键，将键和值拼接，以键-值的方式放入哈希表中

链地址法：在哈希表的每个位置存储一个链表（或类似的结构）来解决冲突。

罗宾汉 Hash：劫富济贫，冲突时避免距离本位太远。

杜鹃鸟 Hash：（总把蛋放别人窝里）（极少使用）  
1.多个 Hash 表情况下，一旦冲突就找其他 Hash 表，直到插入无冲突的表里。  
2.若都冲突，则把原主人挤掉，然后给原主人再进行杜鹃鸟 Hash，循环往复。

**拉链式哈西：**追加桶子，动态扩容（很常用，如 java 的 HashMap（链表太长转红黑树降低层级））

这个是最常用的算法，面试常问

可扩展哈西：看二进制前几位，若冲突则改变 global 的查看位数，然后 reHash。  
拉链式哈西在太拥挤的情况也会扩容槽和桶然后 reHash。

槽

桶