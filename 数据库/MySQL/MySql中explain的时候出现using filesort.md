# MySql中explain的时候出现using filesort

如果待排序的内容不能由所使用的索引直接完成排序的话，那么mysql有可能使用`using filesort`。

这个 filesort 并不是说通过磁盘文件进行排序，而只是告诉我们进行了一个排序操作而已。

filesort是通过相应的排序算法,将取得的数据在内存中进行排序。MySQL需要将数据在内存中进行排序，所使用的内存区域也就是我们通过sort_buffer_size 系统变量所设置的排序区。

在MySQL中filesort 的实现算法实际上是有两种：

双路排序

首先根据相应的条件**取出相应的排序字段**（也就是说并没有取出`select`后面的所有字段）和可以直接定位行数据的行指针信息，然后在sort buffer 中进行排序。

单路排序

一次性取出满足条件行的所有字段，然后在sort buffer中进行排序。

在MySQL4.1版本之前只有第一种排序算法双路排序，第二种算法是从MySQL4.1开始的改进算法，主要目的是为了**减少第一次算法中需要两次访问表数据的 IO 操作**，将两次变成了一次，但相应也**会耗用更多的sort buffer 空间**（因为需要在buffer中保存所有的字段）。当然，MySQL4.1开始的以后所有版本同时也支持第一种算法。









