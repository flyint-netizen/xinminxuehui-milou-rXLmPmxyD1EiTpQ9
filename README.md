
MySQL索引是对数据库表中一列或多列的值进行排序的一种结构，使用索引可提高数据库中特定数据的查询效率。**本节将介绍索引的含义、分类和设计原则。**


**7\.1\.1 索引的含义和特点**索引是一个单独的、存储在磁盘上的数据库结构，包含了对数据表里所有记录的引用指针。使用索引可以快速找出在某个或多个列中有一特定值的行，所有MySQL列类型都可以被索引。对相关列使用索引是加快查询操作速度的最佳途径。


例如，数据库中有2万条记录，现在要执行一个查询“SELECT \* FROM table where num\=10000”，如果没有索引，就必须遍历整张表，直到num等于10000的这一行被找到为止；如果在num列上创建索引，MySQL不需要任何扫描，直接在索引里面找10000，就可以得知这一行的位置。可见，索引的建立可以加快数据库的查询速度。


索引是在存储引擎中实现的，每种存储引擎的索引都不一定完全相同，并且每种存储引擎也不一定支持所有索引类型。根据存储引擎来定义每张表的最大索引数和最大索引长度。所有存储引擎支持每张表至少16个索引，总索引长度至少为256字节。大多数存储引擎有更高的限制。MySQL中索引的存储类型有两种，即BTREE和HASH，具体和表的存储引擎相关：MyISAM和InnoDB存储引擎只支持BTREE索引；MEMORY/HEAP存储引擎可以支持HASH和BTREE索引。


索引的优点主要有以下4点：


（1）通过创建唯一索引，可以保证数据库表中每一行数据的唯一性。


（2）可以大大加快数据的查询速度，这也是创建索引的主要原因。


（3）在实现数据的参考完整性方面，可以加速表和表之间的连接。


（4）在使用分组和排序子句进行数据查询时，可以显著减少查询中分组和排序的时间。


增加索引也有许多不利的方面，主要表现在如下3个方面：


（1）创建索引和维护索引要耗费时间，并且随着数据量的增加，所耗费的时间也会增加。


（2）索引需要占磁盘空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间。如果有大量的索引，则索引文件可能比数据文件更快达到最大文件尺寸。


（3）当对表中的数据进行增加、删除和修改时，索引也要动态地维护，这样就降低了数据的维护速度。


**7\.1\.2 索引的分类**MySQL的索引可以分为以下几类。


1\. 普通索引和唯一索引


普通索引是MySQL中的基本索引类型，允许在定义索引的列中插入重复值和空值。


唯一索引要求索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。主键索引是一种特殊的唯一索引，不允许有空值。


2\. 单列索引和组合索引


单列索引即一个索引只包含单个列，一张表可以有多个单列索引。


组合索引是指在表的多个字段组合上创建的索引，只有在查询条件中使用了这些字段的左边字段时，索引才会被使用。使用组合索引时遵循“最左前缀”原则。


3\. 全文索引


全文索引类型为FULLTEXT，在定义索引的列上支持值的全文查找，允许在这些索引列中插入重复值和空值。全文索引可以在CHAR、VARCHAR或者TEXT类型的列上创建。MySQL中只有MyISAM存储引擎支持全文索引。


4\. 空间索引


空间索引是对空间数据类型的字段建立的索引，MySQL中的空间数据类型有4种，分别是GEOMETRY、POINT、LINESTRING和POLYGON。MySQL使用SPATIAL关键字进行扩展，使得能够用与创建正规索引类似的语法创建空间索引。创建空间索引的列，必须声明为NOT NULL，空间索引只能在存储引擎为MyISAM的表中创建。


**7\.1\.3 索引的设计原则**索引设计不合理或者缺少索引都会对数据库和应用程序的性能造成障碍。高效的索引对于获得良好的性能非常重要。设计索引时，应该考虑以下准则：


（1）索引并非越多越好，一张表中如有大量的索引，不仅占用磁盘空间，还会影响INSERT、DELETE、UPDATE等语句的性能，因为表中的数据更改时，索引也会进行调整和更新。


（2）避免对经常更新的表进行过多索引，并且索引中的列要尽可能少。应该为经常用于查询的字段创建索引，但要避免添加不必要的字段。


（3）数据量小的表最好不要使用索引，当数据较少时，查询花费的时间可能比遍历索引的时间还要短，因此索引可能不会产生优化效果。


（4）在条件表达式中经常用到的不同值较多的列上建立索引，在不同值很少的列上不要建立索引。比如学生表的“性别”字段上只有“男”与“女”两个不同值，因此无须建立索引，如果建立索引，不但不会提高查询效率，反而会严重降低数据更新速度。


（5）当唯一性是某种数据本身的特征时，指定唯一索引。使用唯一索引时要确保定义的列的数据完整性，以提高查询速度。


（6）在频繁进行排序或分组（即进行GROUP BY或ORDER BY操作）的列上建立索引，如果待排序的列有多个，可以在这些列上建立组合索引。————————————————


![](https://img2024.cnblogs.com/blog/270128/202410/270128-20241009094617125-865673461.jpg)


 


 本博客参考[楚门加速器p](https://tianchuang88.com)。转载请注明出处！
