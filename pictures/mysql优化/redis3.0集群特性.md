# redis进阶

---

# Redis 简介

Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库。

Redis 与其他 key - value 缓存产品有以下三个特点：

* Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
* Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
* Redis支持数据的备份，即master-slave模式的数据备份。

# Redis 优势

* 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
* 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
* 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
* 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

# Redis与其他key-value存储有什么不同？

* Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。

* Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，
在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。


# Linux 下安装

下载地址：http://redis.io/download，下载最新文档版本。

本教程使用的最新文档版本为 2.8.17，下载并安装：
```bash
$ wget http://download.redis.io/releases/redis-2.8.17.tar.gz
$ tar xzf redis-2.8.17.tar.gz
$ cd redis-2.8.17
$ make
```
make完后 redis-2.8.17目录下会出现编译后的redis**服务程序redis-server**,还有用于测试的**客户端程序redis-cli**,两个程序位于安装目录 src 目录下：
下面启动redis服务.
```bash
$ cd src
$ ./redis-server
```

注意这种方式启动redis 使用的是默认配置。也可以通过启动参数告诉redis使用指定配置文件使用下面命令启动。
```bash
$ cd src
$ ./redis-server redis.conf
# ./redis-server 命令用来以后面所跟的redis.conf配置文件的配置方式进行启动
```
redis.conf是一个默认的配置文件。我们可以根据需要使用自己的配置文件。
启动redis服务进程后，就可以使用测试客户端程序redis-cli和redis服务交互了。 比如：
```bash
$ cd src
$ ./redis-cli
redis> set foo ba
OK
redis> get foo
"bar"

```

# Redis 配置

Redis 的配置文件位于 Redis 安装目录下，文件名为 redis.conf。

参数说明
redis.conf 配置项说明如下：
1. Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程

	    daemonize no

2. 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
 
	    pidfile /var/run/redis.pid

3. 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379在手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字
		    
		port 6379
4. 绑定的主机地址
		
		
		    bind 127.0.0.1
5.当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能

	    timeout 300

6. 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose

	    loglevel verbose

7. 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null

	    logfile stdout

8. 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
	
	    databases 16
9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合

	    save <seconds> <changes>
	    Redis默认配置文件中提供了三个条件：
	    save 900 1
	    save 300 10
	    save 60 10000
	    分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。
 
10. 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
	
	    rdbcompression yes
11. **指定本地数据库文件名，默认值为dump.rdb**
	
	    dbfilename dump.rdb

12.** 指定本地数据库存放目录**
	
	    dir ./

13. **设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步**
	
	    slaveof <masterip> <masterport>

14. 当master服务设置了密码保护时，slav服务连接master的密码
	
	    masterauth <master-password>

15. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH <password>命令提供密码，默认关闭
	
	    requirepass foobared

16. 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息
	
	    maxclients 128

17. 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
	
	    maxmemory <bytes>

18. 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
	
	    appendonly no

19. 指定更新日志文件名，默认为appendonly.aof
	
	     appendfilename appendonly.aof

20. 指定更新日志条件，共有3个可选值： 
	
	    no：表示等操作系统进行数据缓存同步到磁盘（快） 
	    always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全） 
	    everysec：表示每秒同步一次（折衷，默认值）
	    appendfsync everysec
	 

21. 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）
	
	     vm-enabled no

22. 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享
	
	     vm-swap-file /tmp/redis.swap

23. 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0
	
	     vm-max-memory 0

24. Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不 确定，就使用默认值
	
	     vm-page-size 32

25. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。
	
	     vm-pages 134217728

26. 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4
	
	     vm-max-threads 4

27. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启
	
	    glueoutputbuf yes

28. 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
	    hash-max-zipmap-entries 64
	    hash-max-zipmap-value 512
29. 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）
	
	    activerehashing yes
30. 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件
	
	    include /path/to/local.conf


# Redis 数据类型

**Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。**

详情参考 http://www.runoob.com/redis/redis-data-types.html

# Redis命令

Redis keys 命令
下表给出了与 Redis 键相关的基本命令：


	1	DEL key

该命令用于在 key 存在时删除 key。
	
	2	DUMP key 
序列化给定 key ，并返回被序列化的值。
	
	3	EXISTS key 
检查给定 key 是否存在。
	
	4	EXPIRE key seconds
为给定 key 设置过期时间。
	
	5	EXPIREAT key timestamp 
EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。
	
	6	PEXPIRE key milliseconds 
设置 key 的过期时间以毫秒计。
	
	7	PEXPIREAT key milliseconds-timestamp 
设置 key 过期时间的时间戳(unix timestamp) 以毫秒计
	
	8	KEYS pattern 
查找所有符合给定模式( pattern)的 key 。
	
	9	MOVE key db 
将当前数据库的 key 移动到给定的数据库 db 当中。
	
	10	PERSIST key 
移除 key 的过期时间，key 将持久保持。
	
	11	PTTL key 
以毫秒为单位返回 key 的剩余的过期时间。
	
	12	TTL key 
以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。
	
	13	RANDOMKEY 
从当前数据库中随机返回一个 key 。
	
	14	RENAME key newkey 
修改 key 的名称
	
	15	RENAMENX key newkey 
仅当 newkey 不存在时，将 key 改名为 newkey 。
	
	16	TYPE key 
返回 key 所储存的值的类型。


详情参考 : https://redis.io/commands

# Redis 字符串命令 
http://www.runoob.com/redis/redis-strings.html

# Redis hash 命令
http://www.runoob.com/redis/redis-hashes.html

# Redis 列表命令

http://www.runoob.com/redis/redis-lists.html


# Redis 有序集合命令
http://www.runoob.com/redis/redis-sorted-sets.html


# Redis 事务

Redis 事务可以一次执行多个命令， 并且带有以下两个重要的保证：

* 事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
* 事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。

一个事务从开始到执行会经历以下三个阶段：

* 开始事务。
* 命令入队。
* 执行事务。

http://www.runoob.com/redis/redis-transactions.html

# * Redis的持久化

Redis的强大功能很大程度上是由于其将所有数据都存储在内存中，为了使Redis在重启后仍能保证数据不丢失，需要将数据从内存中以某种形式持久化到硬盘中。

Redis支持两种方式的持久化，一种是RDB方式，一种是AOF方式。可以单独使用其中一种或将两种结合使用。

RDB方式是通过快照完成的，当符合一定条件时Redis会自动将内存中的所有数据进行快照并且存储到硬盘上。进行快照的条件在配置文件中指定，有2个参数构成：时间和改动的键的个数，当在指定时间内被更改的键的个数大于指定数值时就会进行快照。

**RDB是Redis的默认持久化方式。**

## Redis的持久化之RDB

在配置文件中已经预置了3个条件：

	save 900 1  #15分钟内有至少1个键被更改则进行快照
	save 300 10  #5分钟内至少有10个键被更改则进行快照
	save 60 10000  #1分钟内至少有100000个键被更改则进行快照

以上条件之间是“或”的关系。

默认的rdb的文件路径是在当前目录，文件名是：dump.rdb，可以在配置文件中修改路径和文件名，分别是dir和dbfilename。

Redis启动后会读取RDB快照文件，将数据从硬盘载入到内存，一般情况下1GB的快照文件载入到内存的时间约为20~30秒钟。（不同服务器会有差异）

### RDB数据恢复过程

**RDB的快照过程如下：**

Redis使用fork函数复制一份当前进程（父进程）的副本（子进程）；
父进程继续接收并处理客户端发来的命令，而子进程开始将内存中的数据写入到硬盘中的临时文件；
当子进程写入完所有数据后会用该临时文件替换旧的RDB文件。

**RDB文件是通过压缩的，可以通过配置rdbcompression参数来禁用压缩**

## Redis的持久化之AOF

**Redis的AOF持久化策略是将发送到Redis服务端的每一条命令都记录下来，并且保存到硬盘中的AOF文件，AOF文件的位置和RDB文件的位置相同，都是通过dir参数设置，默认的文件名是appendonly.aof，可以通过appendfilename参数修改。**


文件写入默认情况下会先写入到系统的缓存中，系统每30秒同步一次，才是真正的写入到硬盘，如果在这30秒服务器宕机那数据也会丢失的，Redis可以通过配置来修改同步策略：

		# appendfsync always  每次都同步 （最安全但是最慢）
		appendfsync everysec  每秒同步  （默认的同步策略）
		# appendfsync no  不主动同步，由操作系统来决定 （最快但是不安全）











