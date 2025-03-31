> [!caution] This page contained a drawing which was not converted.   

![Exported image](Exported%20image%2020250328134110-0.png)

很多操作都是先 sort 的，在执行计划中也会把 sort 计划进去

2-Way 外部归并排序，2way 的含义就是每次读两个 page 排序（还有 3way 等）

磁盘上 N 个 pages 的表

buffer 上有 B 个 page

输出的时候排好大的，输出；排好小的，输出。所以只需要一个 page 的内存大小进行输出

**数据多内存小，结果要从内存存到磁盘（分支的思想）**

普遍的外部归并排序（即 n-way）

聚簇的 B+树：提早物化，即将整行数据放在 B+树里了

非聚簇的 B+树：比如只存了个 id，等排好序再把每行内容放入 B+树

每个叶子节点和 page 里的 tuple 是一一对应的，即一行数据

排序聚集

哈西聚集

哈西表过大，分区后落盘

磁盘里的 Hash 表太大，没做完去重，且一个 Hash 函数碰撞过多，所以要 ReHash

具体用 sorting agg 还是 hashing agg，具体情况具体分析：￼1.若已经做好排序，那直接用 sorting agg 更好。  
2.如果之前已经建立 hash 了，用 hashing agg 更好。
 
取决于优化器