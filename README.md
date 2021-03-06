## Wikipedia 全文搜索引擎
---
小型搜索引擎实现版本，暂无进行索引压缩，主要用于展示搜索引擎的基本原理。采用XCode编写。

测试版本，需要自行编译，在项目Linking-Other Linker Flags中添加-lexpat -lsqlite3，默认对前1000个词条建立倒排索引。

####依赖：

+ expat
+ sqlite3

####基本思想：
1. 利用expat对xml文件进行解析
2. 默认使用bi-gram进行分词
3. 对每个词元建立倒排索引
4. 当词元数量达到一定数量的时候，内存缓存的词元倒排索引与数据库中存储的倒排索引进行合并，内存缓存用于减小对硬盘上的IO读写次数。
5. 搜索采用TF-IDF算法对文档进行排序

####正在添加的功能
1. 利用golomb编码对倒排索引进行压缩（还有部分bug需要修改）

####演示（内存缓存文档数阈值16384）
#####1. 对前1000个词条建立倒排索引
![](http://7fvfml.com1.z0.glb.clouddn.com/wiki.png)

分词+建立索引大概22s，把内存索引合并到数据库上大概5s

#####2. 对前10000个词条建立倒排索引
![](http://7fvfml.com1.z0.glb.clouddn.com/wiki2.png)

分词+建立索引大概119s，合并到数据库上大概17s

#####3. 搜索(样本为2建立的倒排索引数据库)
![](http://7fvfml.com1.z0.glb.clouddn.com/wiki3.png)

![](http://7fvfml.com1.z0.glb.clouddn.com/wiki4.png)

结果有665个文档，搜索时间为0.01s



