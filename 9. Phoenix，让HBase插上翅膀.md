你是否还在为HBase shell操作而苦恼，为HBase的Java API为烦心？想不想为HBase插上一双SQL的翅膀，带你飞给你希望。

目前对于HBase的操作方式主要分为四种：

基于HBase提供的shell命令行和Java API进行原始操作
以MapReduce为核心，单个任务使用HBase client原始接口访问。比如Hive和Impala
以Google Dremel为核心，单个任务使用HBase client原始接口访问。比如Drill
以HBase Coprocessor为核心，结合Google Dremel思想，客户端合并多个节点的处理结果。比如Phoenix

注：Google Dremel是"交互式"数据分析系统，对PB级别数据处理时间缩短到秒级，是对MapReduce交互式查询能力不足的补充（非替代品），具体实现有兴趣可以参考Google论文：[Dremel: Interactive Analysis of Web-Scale Datasets](https://ai.google/research/pubs/pub36632)

Phoenix是一个HBase的开源SQL引擎，号称We put the SQL back in NoSQL。通过Phoenix你可以使用标准的JDBC API代替HBase客户端API来创建表，插入数据，查询你的HBase数据。

Phoenix将你输入的SQL语句编译成原生的HBase scan语句，检测到scan语句最佳开始和结束的key，精心编排你的scan语句，依托定制的hbase协处理器，在hbase的服务端进行操作，从而最大限度的减少客户端和服务器的数据传输，进而提高效率，除此之外，Phoenix还有一些特性来增强HBase的功能，它实现了二级索引来提升非主键字段查询的性能，统计相关数据提高并行化水平，并帮助选择最佳优化方案，跳过扫描过滤器来优化IN，LIKE，OR查询以及优化主键的来均匀分布写压力等等。



## 安装Phoenix

Phoenix的安装非常简单