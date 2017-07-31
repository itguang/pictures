> **看到现在好多Web项目用了MyBatis，没感觉MyBatis好到哪里了，从开发效率上讲，不管是Hibernate的反向工程，还是自动建表，关联映射都比MyBatis方便得多。难道仅仅是因为运行速度，Hibernate也有缓存啊。**
**或者，二者适用场合不同，再或者，MyBatis就是比Hibernate出现晚，晚的就是好么。**



# 观点一

## **1、开发对比开发速度**

Hibernate的真正掌握要比Mybatis来得难些。Mybatis框架相对简单很容易上手，但也相对简陋些。个人觉得要用好Mybatis还是首先要先理解好Hibernate。

**开发社区**

Hibernate 与Mybatis都是流行的持久层开发框架，但Hibernate开发社区相对多热闹些，支持的工具也多，更新也快，当前最高版本4.1.8。而Mybatis相对平静，工具较少，当前最高版本3.2。

**开发工作量**

Hibernate和MyBatis都有相应的代码生成工具。可以生成简单基本的DAO层方法。

针对高级查询，Mybatis需要手动编写SQL语句，以及ResultMap。而Hibernate有良好的映射机制，开发者无需关心SQL的生成与结果映射，可以更专注于业务流程。

## **2、系统调优对比Hibernate的调优方案**

制定合理的缓存策略；

尽量使用延迟加载特性；

采用合理的Session管理机制；

使用批量抓取，设定合理的批处理参数（batch_size）;

进行合理的O/R映射设计

**Mybatis调优方案**

MyBatis在Session方面和Hibernate的Session生命周期是一致的，同样需要合理的Session管理机制。MyBatis同样具有二级缓存机制。 MyBatis可以进行详细的SQL优化设计。

**SQL优化方面**

Hibernate的查询会将表中的所有字段查询出来，这一点会有性能消耗。Hibernate也可以自己写SQL来指定需要查询的字段，但这样就破坏了Hibernate开发的简洁性。而Mybatis的SQL是手动编写的，所以可以按需求指定查询的字段。

Hibernate HQL语句的调优需要将SQL打印出来，而Hibernate的SQL被很多人嫌弃因为太丑了。MyBatis的SQL是自己手动写的所以调整方便。但Hibernate具有自己的日志统计。Mybatis本身不带日志统计，使用Log4j进行日志记录。

**扩展性方面**

Hibernate与具体数据库的关联只需在XML文件中配置即可，所有的HQL语句与具体使用的数据库无关，移植性很好。MyBatis项目中所有的SQL语句都是依赖所用的数据库的，所以不同数据库类型的支持不好。

## **3、对象管理与抓取策略对象管理**

Hibernate 是完整的对象/关系映射解决方案，它提供了对象状态管理（state management）的功能，使开发者不再需要理会底层数据库系统的细节。也就是说，相对于常见的 JDBC/SQL 持久层方案中需要管理 SQL 语句，Hibernate采用了更自然的面向对象的视角来持久化 Java 应用中的数据。

换句话说，使用 Hibernate 的开发者应该总是关注对象的状态（state），不必考虑 SQL 语句的执行。这部分细节已经由 Hibernate 掌管妥当，只有开发者在进行系统性能调优的时候才需要进行了解。

而MyBatis在这一块没有文档说明，用户需要对对象自己进行详细的管理。

**抓取策略**

Hibernate对实体关联对象的抓取有着良好的机制。对于每一个关联关系都可以详细地设置是否延迟加载，并且提供关联抓取、查询抓取、子查询抓取、批量抓取四种模式。 它是详细配置和处理的。

而Mybatis的延迟加载是全局配置的。

## **4、缓存机制对比Hibernate缓存**

Hibernate一级缓存是Session缓存，利用好一级缓存就需要对Session的生命周期进行管理好。建议在一个Action操作中使用一个Session。一级缓存需要对Session进行严格管理。

Hibernate二级缓存是SessionFactory级的缓存。 SessionFactory的缓存分为内置缓存和外置缓存。内置缓存中存放的是SessionFactory对象的一些集合属性包含的数据(映射元素据及预定SQL语句等),对于应用程序来说,它是只读的。

外置缓存中存放的是数据库数据的副本,其作用和一级缓存类似.二级缓存除了以内存作为存储介质外,还可以选用硬盘等外部存储设备。

二级缓存称为进程级缓存或SessionFactory级缓存，它可以被所有session共享，它的生命周期伴随着SessionFactory的生命周期存在和消亡。

## **5、优势对比**

**Mybatis优势**

* MyBatis可以进行更为细致的SQL优化，可以减少查询字段。
* MyBatis容易掌握，而Hibernate门槛较高。

**Hibernate优势**

* Hibernate的DAO层开发比MyBatis简单，Mybatis需要维护SQL和结果映射。
* Hibernate对对象的维护和缓存要比MyBatis好，对增删改查的对象的维护要方便。
* Hibernate数据库移植性很好，MyBatis的数据库移植性不好，不同的数据库需要写不同SQL。
* Hibernate有更好的二级缓存机制，可以使用第三方缓存。MyBatis本身提供的缓存机制不佳。



# 观点二

作为mybatis支持者我来写几句。

首先是运行速度，hibernate是在jdbc上进行了一次封装，而mybatis基于原生的jdbc，因此mybatis天生就有运行速度上的优势。

然后mybatis开放了插件接口。也许mybatis团队知道自己人少力单，索性把很多功能接口都开放了。不好分页？网上大神写的分页插件多得很；需要手写sql？按注解生成自动生成sql的插件早就有了；还有缓存的插件等等。

可以说，只要肯在mybatis上花时间，你会发现orm这一块的所有问题它都有解决方案。

这方面不是说hibernate不好，但是我还真没听说过hibernate有插件了。还有就是对遗留系统的支持。很多系统在设计之初还没有orm思想，现在想“抢救”一下，用mybatis就比hibernate更合适。

因为mybatis可以很容易做到不规范的映射对象和规范的映射对象共存，如果这种系统中再需要增加个需要复杂sql的功能，mybatis只需要把sql手写出来，先把功能运行起来后再看看能不能变成自动生成的sql，而对hibernate来说就很困难了

# 观点三:

作者：王晨

链接：https://www.zhihu.com/question/21104468/answer/134854807

来源：知乎

说Hibernate性能问题的，没有用Mybatis手写sql来的快的都是Hibernate的菜鸟。你自己不会弄不能说框架不行。在单数据库下会用Hibernate的都知道，Hibernate性能远远高于手动sql。

这个结论是Hibernate之父Gavin King也是在jpa规范的制定者很早拿出来宣传的重点。在国外的java圈，这个观点大家都是默认。否则当年Gavin King也进不了sun。也不可能在java世界有如今的地位。

Mybatis只是一个半orm产品，除了灵活好学外各方面都不如Hibernate。用Mybatis之所以能流行起来有两个原因。适合初学者意淫是基础。第二。是bat喜欢使用。之于为为什么bat喜欢用是因为，以bat的流量已经到了要自己开发自己数据库以适应大规模集群的阶段。

比如：阿里的核心数据库就是在开源的mysql上改过的。这种改过的数据库，Mybatis手写sql为核心的框架有天生的优势。Hibernate全自动orm的优点在于会用的人拿它做项目，快速，高效，高度解耦。

在项目开发，数据库结构不断变幻阶段，不知甩Mybatis几条街。但是，也是因为它全自动的优点，在计算机集群需要大量跨数据库事务的环境下是不如Mybatis灵活。Mybatis只是一个小玩具，它有的功能，Hibernate也有。Hibernate也有自己拦截器，甚至可以在它生成的sql里像Mybatis加入自己的东西的。

不过，一般没人，国内也没这类的教程。因为，这么用就失去了用Hibernate提高开发进度的本意。单数据库下Hibernate无敌的存在，有怀疑的都是外行。集群下优点变缺点，但是，不是不能解决google有一个开源项目可缓解这个问题。只是比Mybatis复杂很多而已。

所以，Hibernate特别适合那些有技术实力，项目处于开发期，数据层级不稳定的时期。之于，数据量大了以后，真正上开始上集群。

Mybatis这个小玩具开始上台的时候，其实，数据库技术其实已经不是核心，大数据跟no-sql会更好地解决问题。这一时候的db层，所有人的精力基本上也在hadoop跟spark上了。只会Mybatis在有点规模的互联公司，永远走不出非主流，小三的命运


# 观点四

hibernate在实际项目开发中，前期很顺利
后期每个人都恨不得杀了hi

# 观点五

hibernate的门槛比较高.老实说,真的会用hibernate,并且能在产品里驾驭的人很少很少.

___________

补充一下，hibernate的sql是可控的。调优也和sql没有什么大区别。觉得关联查询控制不住的，是因为不太会用。


# 观点六

hibernate的门槛比较高.老实说,真的会用hibernate,并且能在产品里驾驭的人很少很少.

___________

补充一下，hibernate的sql是可控的。调优也和sql没有什么大区别。觉得关联查询控制不住的，是因为不太会用。