全相联高速缓存是由一个包含所有高速缓存行的组组成的。在高速缓存中查找所需字w的副本。如果命中，立即返回字w给CPU。不命中，从存储器层次结构中较低层取出包含字w的块。

更新w在层次结构中，紧接着低一层中的副本：
直写（write-through），立即将w的高速缓存块写回到紧接着的第一层中。缺点为每次写都会引起总线流量。
写回（write-back），尽可能推迟更新，只有当替换算法要驱逐这个更新过的块时，才把它写到紧接着的第一层中。写回能显著减少总线流量，但缺点是增加了复杂性。

处理写不命中。写分配，加载相应的低一层中的块到高速缓存中，然后更新高速缓存块。缺点是每次不命中都会导致一个块从低一层传送到高速缓存。非写分配，避开高速缓存，直接把这个字写到低一层中。

存储器层次结构中较低层的缓存更可能使用写回，而不是直写。
高速缓存既保存数据，也保存指令。只保存指令的高速缓存称为i-cache。只保存程序数据的高速缓存称为d-cache。
