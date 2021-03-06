# 高效使用 Secondary Index
TiDB 支持完整的二级索引，并且是全局索引，很多查询可以通过索引来优化。如果利用好二级索引，对业务非常重要，很多 MySQL 上的经验在 TiDB 这里依然适用，不过 TiDB 还有一些自己的特点，需要注意，这一节主要讨论在 TiDB 上使用二级索引的一些最佳实践。

- 二级索引的数目
二级索引能加速查询，但是要注意新增一个索引是有副作用的，在上一节中我们介绍了索引的存储模型，那么每增加一个索引，在插入一条数据的时候，就要新增一个 Key-Value，所以索引越多，写入越慢，并且空间占用越大。另外过多的索引也会影响优化器运行时间，并且不合适的索引会误导优化器。所以索引并不是越多越好。

- 对哪些列建索引比较合适
上面提到，索引很重要但不是越多越好，我们需要根据具体的业务特点创建合适的索引。原则上我们需要对查询中需要用到的列创建索引，目的是提高性能。<br>
这里举一个例子，假设常用的查询是 select * from t where c1 = 10 and c2 = 100 and c3 > 10, 那么可以考虑建立组合索引 Index cidx (c1, c2, c3)，这样可以用查询条件构造出一个索引前缀进行 Scan。
	- 区分度比较大的列，通过索引能显著地减少过滤后的行数
	- 有多个查询条件时，可以选择组合索引，注意需要把等值条件的列放在组合索引的前面

- 通过索引查询和直接扫描 Table 的区别
TiDB 实现了全局索引，所以索引和 Table 中的数据并不一定在一个数据分片上，通过索引查询的时候，需要先扫描索引，得到对应的行 ID，然后通过行 ID 去取数据，所以可能会涉及到两次网络请求，会有一定的性能开销。<br>
如果查询涉及到大量的行，那么扫描索引是并发进行，只要第一批结果已经返回，就可以开始去取 Table 的数据，所以这里是一个并行 + Pipeline 的模式，虽然有两次访问的开销，但是延迟并不会很大。<br>
有两种情况不会涉及到两次访问的问题
	- 索引中的列已经满足了查询需求。比如 Table t 上面的列 c 有索引，查询是 select c from t where c > 10; 这个时候，只需要访问索引，就可以拿到所需要的全部数据。这种情况我们称之为覆盖索引(Covering Index)。所以如果很关注查询性能，可以将部分不需要过滤但是需要再查询结果中返回的列放入索引中，构造成组合索引，比如这个例子： select c1, c2 from t where c1 > 10; 要优化这个查询可以创建组合索引 Index c12 (c1, c2)。

	- 表的 Primary Key 是整数类型。在这种情况下，TiDB 会将 Primary Key 的值当做行 ID，所以如果查询条件是在 PK 上面，那么可以直接构造出行 ID 的范围，直接扫描 Table 数据，获取结果。

- 查询并发度
数据分散在很多 Region 上，所以 TiDB 在做查询的时候会并发进行，默认的并发度比较保守，因为过高的并发度会消耗大量的系统资源，且对于 OLTP 类型的查询，往往不会涉及到大量的数据，较低的并发度已经可以满足需求。对于 OLAP 类型的 Query，往往需要较高的并发度。所以 TiDB 支持通过 System Variable 来调整查询并发度。
	- tidb_distsql_scan_concurrency
在进行扫描数据的时候的并发度，这里包括扫描 Table 以及索引数据。

	- tidb_index_lookup_size
如果是需要访问索引获取行 ID 之后再访问 Table 数据，那么每次会把一批行 ID 作为一次请求去访问 Table 数据，这个参数可以设置 Batch 的大小，较大的 Batch 会使得延迟增加，较小的 Batch 可能会造成更多的查询次数。这个参数的合适大小与查询涉及的数据量有关。一般不需要调整。

	- tidb_index_lookup_concurrency
如果是需要访问索引获取行 ID 之后再访问 Table 数据，每次通过行 ID 获取数据时候的并发度通过这个参数调节。

- 通过索引保证结果顺序
索引除了可以用来过滤数据之外，还能用来对数据排序，首先按照索引的顺序获取行 ID，然后再按照行 ID 的返回顺序返回行的内容，这样可以保证返回结果按照索引列有序。前面提到了扫索引和获取 Row 之间是并行 + Pipeline 模式，如果要求按照索引的顺序返回 Row，那么这两次查询之间的并发度设置的太高并不会降低延迟，所以默认的并发度比较保守。可以通过 tidb_index_serial_scan_concurrency 变量进行并发度调整。

- 逆序索引
目前 TiDB 支持对索引进行逆序 Scan，但是速度要比顺序 Scan 慢 5 倍左右，所以尽量避免对索引的逆序 Scan。

