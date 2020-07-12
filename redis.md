# Redis 简介
## 官网
1 redis.io https://github.com/antirez/redis
2. 作者 Salvatore Sanfilippo

## NoSQL 介绍
### 解决了什么问题
NoSQL 数据库的产生就是为了解决大规模数据集合 和多重数据种类带来的挑战，尤其是大数据应用难题
### NoSQL 分类
#### 键值(Key-Value)存储数据库
概述
常见：Redis
列存储数据库
概述
常见：Hbase
文档型数据库
概述
常见：MongoDB
图形(Graph)数据库
概述
常见：Neo4J
适用场景
1. 数据模型比较简单
2. 需要灵活性更强的 IT 系统
3. 对数据库性能要求较高
4. 不需要高度的数据一致性
5. 对于给定 Key，比较容易映射复杂的环境
关系型数据库
关系型数据库需要对：表与表之间建立关联关系
Redis 与其他 Key-Value 缓存产品对比：
1. Redis 支持数据的持久化， 可以将数内存中的数据保存在磁盘中， 重启的时候可以再次加载进行适用
2. Redis 不仅仅支持简单的 key-value 类型的数据， 同时还提供 list, set，zset, hash等数据结构的存储
3. Redis 支持数据的备份，集群等高可用功能
Redis 特点
1. 速度快
读取速度 11w/秒
写的速度是 8.1w/秒
2. 提供丰富的数据类型
String
List
Set
ZSet
Hash
3. 具备原子性
Redis 所有操作都是原子性，意思就是要么执行成功要么执行失败完全不执行。 单个操作是原子性的。多个操作也支持事务，即原子性，通过 MULTI  和 EXEC 指令包起来
4. 丰富的特征
Redis 还支持 publish/subscribe, 通知，key  过期等特征
总结
小总结
优点
缺点
Redis 的安装/配置
Mac 安装
brew安装命令： brew install redis
Linux(CentOS)
下载地址：http://download.redis.io/releases/redis-5.0.8.tar.gz
Redis安装
gcc 安装 
yum -y install gcc automake autoconf libtool make
注意：运行 yum 时出现 /var/run/yum.pid 已被锁定， PID 为 xxx 的另一个程序正在运行的问题解决
rm -rf /var/run/yum.pid
redis 安装
1. wget http://download.redis.io/releases/redis-5.0.8.tar.gz
2. 解压：tar -zxvf redis-5.0.8.tar.gz
3. 编译：make 或 make MALLOC=libc
4. 安装：make PREFIX=/usr/local/redis install
启动命令
1. 进入目录： cd /user/local/redis/bin
2. 启动服务： ./redis-server &
3. 客户端访问：./redis-cli
redis-cli -h IP 地址 -p 端口
退出客户端 control + C
4. PING 命令
配置文件详解 redis.conf
说明
Redis 的配置文件 位于Redis 的安装目录下， 文件名为 redis.conf ( Windows 名为 redis.windows.conf)
配置Redis
1. 在安装包的根目录下找到 redis.conf 文件
2. 复制配置文件安装目录
cp redis.conf /usr/local/redis
配置文件解读
查看配置文件 命令
less -mN redis.conf
1. Redis 默认不是以守护进程的方式运行， 可以通过该配置项修改，使用 yes 启用守护进程
daemonize no
2. 当 Redis 以守护进程方式运行时， Redis 默认会把 pid 写入 /var/run/redis.pid 文件，可以通过pridfile 指定
pidfile/var/run/redis.pid
3. 指定 Redis 监听端口默认为 6379， 为什么选用6379 为默认端口， 因为 6379 在手机按键上 MERZ 对应编码，而 MERZ 取子意大利歌女 Alessia Merz 的名字
port 6379
4. 绑定的主机地址
bind 127.0.0.1
5. 当客户端闲置时间多长后关闭联机，如果指定为0，表示关闭该功能
timeout 300
6. 指定日志记录级别， Redis 总共支持四个级别：debug, verbose, notice, warning，默认为verbose 
loglevel verbose
7. 日志记录方式， 默认标准输出，如果配置 Redis 为守护进程方式运行， 而这里有配置为日志记录方式为标准输出，则日志将会发送给 /dev/null
logfiel stdout
8.设置数据库的数量， 默认数据库为0， 可以使用 SELECT <dbid> 命令在链接指定数据库Id
database 16
9. 指定多长时间内，有多少个更新操作，就会将数据同步到数据文件，可以多个条件配合。
save <seconds> <changes>
Redis 提供了三个条件
save 900 1
save 300 10
save 60 10000
10. 指定存储至本地数据库时否压缩数据，默认为 yes , Redis 采用 LZF 压缩， 如果为了节省 CPU 时间，可以关闭该选项，但会导致数据库文件变得巨大
rdbcompression yes
11. 指定本地数据库文件名，默认为 dump.rdb
dbfilename dump.rdb
12. 指定本地数据库存放目录
dir ./
13. 设置本地 slav 服务时， 设置master 服务的IP地址及端口，在 Redis 启动时，它会自动从 master 进行数据同步
slaveof <masterip> <masterport>
14. 当 master 服务设置了密码保护时， slav 服务链接 master 的密码
masterauth <master-password>
15. 设置 Redis 链接密码，如果设置了链接密码，客户端在链接 Redis 时需要通过 AUTH <password> 命令提供密码，默认关闭
requirepass foobared
16. 设置同一时间最大客户端连接数，默认无限制， Redis 可以同时打开的客户端连接数为 进程可以打开的最大文件描述符数量， 如果设置为 maxclients 0,  表示不限制，当客户端连接数到达限制时，
Redis 会关闭新的链接并想客户端返回 max number of clients reached 错误信息
maxclients 128
17. 指定Redis 最大内存限制，Redis 在启动的时候会把数据加载到内存中，达到最大内存后， Redis 会先 清除已经到期或即将到期的 Key ， 在此处理方法之后，仍然到达最大的内存设置，将无法在进行写入操作，
但仍然可以进行读取操作，Redis 新的 vm 机制， 会把 Key 放入内存， Value 会放入 swap 区
maxmemory <bytes> 
18. 指定是否在每次更新操作后进行日志记录，Redis 在默认情况下是异步的把数据写入磁盘，如果不开启， 可能会在断电时导致一段时间内的数据丢失。因为 Redis 本身同步数据文件时按上面 save 条件来进行同步的，
所以一段时间内只存在内存中默认为 no
appendonly no
19. 指定更新日志文件名，默认为 appendonly.aof
appedfilename appendonly.aof
20. 指定更新日志条件，一共有3个可选值
no 表示等操作系统进行数据缓存同步到磁盘（快）
always 表示每次更新操作后手动调用 fsync() 将数据写到磁盘（慢，安全）
everyses: 表示每秒同步一次（折中，默认值）
appendfsync everysec
21. 指定是否启用虚拟机内存机制，默认为 no , 简单的介绍一下，VM 机制将数据分页存放， 由 Redis 访问量较少的页冷数据 swap 到磁盘上，访问最多的页面是由磁盘自动切换出到内存中（后面会分析到）
vm-enabled no
22. 虚拟机内存文件路径，默认值为 /tmp/redis.swap,  不可多个 Redis 实例共享
23. 将所有大于 vm-max-memory 的数据存如虚拟机内存，无论 vm-max-memory 设置多小, 所有索引数据都是内存存储的 （Redis 的索引数据就是 keys） 也就是说，
当 vm-max-memory 设置为 0 的时候其实所有 value 都存在磁盘中， 默认值为 0
vm-max-memory 0
24. Redis swap 文件分成了多个 page，一个对象可以保存在多个 page 上面，但一个 page 上不能被多个对象共享，  vm-page-size 是要根据存储的数据大小来设定的， 作则建议如果存储很多小对象，page 大小最好设置为32 或者 64 bytes; 如果存储很大对象，
则可以使用更大的 page , 如果不确定，就是用默认值
vm-page-size 32
25. 设置 swap 文件的page 数量， 由于页表（一种表示页面空闲或使用 bitmap）s是放在内存中的，在磁盘上每 8 个pages 将消耗1bytes 内存
vm-pages 134217728
26.设置访问 swap 文件的线程数， 最好不要超过机器的核数， 如果设置为0， 那么对swap 文件的操作都是串行的， 可能会造成比较长的时间的延迟，默认值为：4
vm-max-threads 4
27. 设置在向可兑换应答时，是否把较小的包含合并为一个发送，默认开启
glueoutputbuf yes
28.指定在超过一定数量或者最大元素超过某临界值时，采用一种特殊的hash 算法
hash-max-zipmap-entries 64
hash-max-zipmap-value 512
29. 指定激活重置哈希，默认开启（后面介绍 Redis 的哈希算法的时候具体介绍）
activerehashing yes
30. 指定包含其他的配置文件，可以在同一个主机上多个 Redis 实例之间使用同一份配置文件， 而同时各个实例又拥有自己的特定配置文件
include /path/to/local.conf
31.  redis 作为优秀的中间件缓存，时长会存储大量的数据，即使采取了集群部署来动态拓容， 也应该即时的整理内存，维持系统性能（如果数据一直新增，内存则很快会占满）
两种解决方案
1. 为数据设置超时时间
设定内存，建议不要超过1 G 256-512M
2. 采用 LRU算法动态不用的数据删除
1. volatile-lru
设定超时时间的数据中，删除最不常用的数据
2. allkeys-lru
查询所有的key 中最不常使用的数据进行删除，这是应用最广泛的策略
3. volatile-random
在已经设定了超时的数据中随机删除
4. allkeys-random
查询所有的 key 之后随机删除
5. volatile-ttl
查询全部设定超时时间的数据，追后马上排序，将马上将要过期的数据进行删除操作
6. noeviction (默认)
如果设置为该属性，则不会进行删除操作，如果内存溢出则报错返回
7. volatile-lfu
从所有配置了过期的时间的键中驱逐使用频率最少的键
8. allkeys-lfu
从所有键中驱逐使用频率最少的键
自定义配置
1. 进入安装目录 cd /usr/local/redis
2. 编辑 redis.conf  vim redis.conf
3. 设置默认必须修改的参数
1. daemonize no 修改为 daemonize yes
2. 注释掉 bind 127.0.0.1
3. requirepass 设置密码
4. 启动 Redis 命令
1. ./bin/redis-server redis.conf
2. 查询进程： ps -ef | grep redis
3. 客户端登录：./bin/redis-cli -h 127.0.0.1 -p 16379 -a Iredis8Pwd
Redis 的关闭
1. 断电、非正常关闭（容易丢失数据）
查询 PID  ps -ef | grep redis
kil -9 PID
2. 正常关闭，保存数据
./bin/redis-cli shutdown
如果设置了密码需要先通过客户端登录，然后执行shutdown 命令即可关闭服务
Redis 常用命令
用命令管理 Redis 的键
DEL key
该命令用于在 key  存在时删除 key
DUMP  key
序列化给定 key ，返回被序列化的值
EXISTS key
检查给定的 key 是否存在
EXPIRE key seconds
为给定的 key 设置过期时间（以毫秒计算）
PEXPIRE key  milliseconds
设置 key 的过期时间以毫秒计
TTL Key
以秒为单位，返回给定 key 的剩余的过期时间。
-1 代表永久有效
-2 代表无效的键
PTTL key
 以好面为单位返回 key 的剩余过期的时间
PERSIST  key
移除 key 的过期时间，key 将持久保持
KEYS pattern
查找符合所有给定模式（pattern）的 key.
key 通配符 获取所有与 pattern 匹配的 key, 返回所有与该匹配
通配符
 * 代表所有
？ 代表匹配一个字符
RANDOMKEY (redis 5)
从当前数据库中随机返回一个key
RENAME key newkey
修改 key 的名称
MOVE key db
将当前数据的 key 移动到指定的数据 db 当中
TYPE key 
返回 key 所存储的值的类型
命令应用场景
EXPIR key seconds
1. 限时优惠活动
2. 网站数据缓存（对于一些需要定时更新的数据，例如：积分排榜榜）
3. 手机验证码
4. 限制网站访客频率（例如：1分钟最多访问 10次）
Key 的命名建议
0. 单个key 最大存入数据 为512M 大小
1. key 不要太长，尽量不要超过 1024 字节，这不仅消耗内存，而且会降低查找的效率；
2. key 也不要太短，太短的话，key 的可读性会降低；
3. 在一些项目中，key 最好使用统一的命名模式，例如 user:123:passwd
4. Key  的名称需要注意区分大小写
Redis 数据类型和 Java 操作 Redis
String 类型
是什么 ？（简介）
string 是 redis 的最基本的类型， 一个 key 对应一个 value
string 类型是二进制安全的， 意思是 redis 的 string 可以包含任何数据， 比如 jpg 图拼啊或者序列化的对象
string 类型是 redis 最基本的数据类型，一个键最大能存储 512MB
二进制安全是指，在传输数据时，在传输二进制的信息安全， 也就是不被篡改、破译等，如果被攻击，能够即使检测出来
特点
1. 编码、解码发生在客户端完成，执行效率高
2. 不需要平凡的编解码，不会出现乱码
怎么用？（常用命令）
赋值语法
SET KEY_NAME VALUE
Redis SET 民力用于设定 key 值。 如果 key 已经存储值，SET 就覆盖旧值， 且无视类型
SETNX  KEY VALUE
只有 key 不存在设置 key 的值， setnx (SET is Not eXists) 命令在执行 key 不存在的时候，为 key 设置指定的值
具有原子性，可以解决分布式锁的问题
MSET KEY  VALUE[key vlue ...]
同时设置一个或者多个 key-value 对
取值语法
GET KEY_NAME
Redis GET 命令用于获取指定 key 的值，如果不存在，返回 nil ， 如果 key 存储的值不是一字符串类型，返回一个错误
GETRANCE key start end
用于获取存储在指定key 中字符串的子字符串。 字符串的截取范围由 start 和 end 两个偏移量决定 （包括 start 和 end 在内的）
GETBIT key offset
获取 key 所存储的字符串值， 获取指定偏移量上的位 bit
MCET  key1 [key2 ..]
获取所有 （一个或多个）指定的值
GETSET 语法： GETSET KEY_NAME VALUE
GETSET 命令用于执行 key 值， 并放回 key  的旧值，当  key 不存在时， 返回 nil
STRLEN key
返回 key 所存储的字符串的长度
删除语法
DEL KEY_NAME
删除指定的 key , 如果存在，返回数值类型。
自增/自减
INCR KEY_NAME
INCR 命令将 key 中存储的数值增 1 ， 如果 key 的值会掀背初始化为0 ，然后再执行 INCR 操作
自增：INCRBY KEY_NAME 增量值
INCRBY 命令将 key 中存储的数字加上指定的增量值
自减：DECR KEY_VALUE 或 DECYBY KEY_VALUE 减值
DECR 命令将 key 存的数字减 1 
字符串拼接： APPEND KEY_NAME VALUE
APPEND 命令用于为指定的 key 追加至末尾，如果不存在为其赋值
应用场景
1. String 通常用于保存单个字符串或 JSON 字符串数据
2. 因 String 是二进制安全的，所以完全可以把一个图片的内容作为字符串来存储
3. 计数器（常规 key-value 缓存运用， 常规计数：微博数，粉丝数）
INCR 等指令本来就具有原子性特征，所以我们完全可以利用 redis 的 INCR ， INCRBY，DECR， DECRBY 等命令来实现原子计数的效果。
假如，在某种场景下有 3 个客户同时读取了  mynum 的值（值为2），然后对其进行加1的操作，
那么， 最后mynum 的值一定是 5 。不少网站都用了redis 这个特征来实现业务上的统计需求
Hash 类型
简介
Redis hash 是一个 string 类型的 field 和 value 的映射表， hash 特别适合用于存储对象 Redis 中每个 hash 可以存储 2^32 -1 键值对（40多亿）可以看成是具有 key 和 value 的 MAP 容器，
该类型非常适合于存储对象的信息如：uname, upass, age 等， 该类型的数据仅占很少的磁盘空间 （相对于JSON）
常用命令
赋值语法
HSET  KEY FIELD VALUE
为指定KEY 设定 FIELD/VALUE
HMET KEY FIELD VLUE [FILELD1 , VLAUE1]
举例： hmset users:1 id 1 uname zhangsan age 22
取值语法
HGET KEY VALUE
获取存储在 HASH 中的值，根据 FIELD 得到 VLAUE
HMGET key field [field]
获取 KEY 所有给定字段的值
HGETALL key
返回 HASH  表中所有的字段和值
HKEYS  key
获取所有哈希表中的字段
HLEN key
获取 hash 表中的字段数量
删除语法
HDEL KEY field1 [field2]
删除一个或者多个 HASH 表字段
其他语法
HSETNX key field value 
只有在字段 field 不存在时， 设置哈希表的字段值
HINCRBY key field increment
为哈希表 key 中的指定字段的浮点数值加上增量 increment
HINCRBYFLOAT key field increment 
为哈希表 key 中国呢的指定字段的浮点数值增加上增量 increment
HEXISTS  key field 
查看哈希表 key 中，指定字段是否存在
使用场景
1. 适合存一个对象，比如存储用户信息，订单信息等
2. 为什么不用 string 存储一个对象？
hash 是最捷径关系型数据结构的数据类型，可以将一个库一条记录或存续中一个对象转换为 hashmap 存放在 redis 中
用户 id 为查找的key , 存储的 value 用户对象包含：姓名，年龄，生日等信息， 如果用普通的 key/value 结构来存储，主要有2中方式。
第一种方式将用户ID作为查找 KEY 把其他信息封装成一个对象以序列化的方式存储， 这种方式的缺点是，增加了序列化，反序列化的开销，并且在需要修改其中一项信息的时候，
需要把整个对象取出来，并且修改操作需要对并发进行保护，引入CAS等复杂问题。
第二种方式将这个用户对象有多少个成员就存成多少个 key-value 对，用户ID + 对应属性的名称作为 唯一标示来取得对应的属性值，虽然省取了序列化开销和并发问题，但是用户ID为重复存储，
如果存在大量这样的数据，内存浪费还是非常客观的。
小总结
Redis  提供的 HASH 很好的解决了这个问题， Redis 的 Hash 世纪是内部存储的 Value 为一个  HashMap ， 并且提供了直接存取这个Map 的成员接口
Redis 不会保留没有 Key 的 Hash 
Java 操作 Redis 
Jedis 基本使用
POM
测试类
Jedis 工具类
JedisPool
获取Jedis 的工具类
测试代码
Jedis 对 Hash 类型的操作
domain
测试代码
Spring-Data 整合 Redis
简介
Spring data 提供了 RedisTemplate 模版它封装了 redis 连接池管理的逻辑，业务代码无需关心获取，释放链接逻辑； spring redis 同时支持 Jedis，Jredis,  rjc 客户端操作
RedisTemplate 操作 string 使用案例
POM
配置类
domain
service
测试类
string 案例限制登录功能
案例描述
用户在2分钟内，仅输入错误密码 5 次如果，超过次数， 限制其登录1小时（要求每次登录失败，都要给出相应提示）
逻辑分析
案例实现
domain
service
单元测试
hash 类型案例
POM
配置类文件
service
单元测试
List 类型
简介
Redis 列表是简单的字符串列表，安扎插入顺序排序，你可以添加一个元素 到列表的头部（左边）或者尾部（右边）一个列表最多可以包含 2^32 - 1 个元素
（4294967295， 每个列表超过 40 亿个元素）
类似 Java 语言中的 LinkedList 类型
命令
赋值语法
LPUSH key value1 [value2]
将一个或多个值插入到列表头部（从左侧添加）
RPUSH key value1 [value2]
在列表中添加一个或多个值（从右侧添加）
LPUSHX key value
将一个值插入到已经存在的头部列表。如果类标不存在，操作无效
RPUSH  key value
一个值插入已存在的列表尾部（最右边）。如果列表不存在，操作无效。
取值语法
LLEN key
获取列表长度
LINDEX key index
通过索引获取列表中的元素
LRANDGE key start stop
获取列表指定范围内的元素
删除语法
LPOP key
移除并获取列表的第一个元素（从左侧删除）
RPOP key
移除列表的最后一个元素， 返回值为异常的元素（从右侧删除）
BLPOP key1 [key2] timeout
移除并获取列表的第一个元素，如果列表没有是元素会足赛知道等待超时或发现可以弹出的元素为止。
BRPOP key1 [key2] timeout
移除并获取列表的最后一个元素， 如果列表没有元素会阻塞列表知道等待超时或发现可弹出元素为止。
LTRIM key start stop
对一个列表进行修剪（trim）, 就是说，让列表止保留指定区间内的元素， 不在指定区间内的元素都被删除。
修改语法
LSET key index value 
通过索引设置列表元素的值
LINSERT key BEFORE | AFTER  world value
在类标的元素钱或后插入元素
高级语法
RPOPLPUSH source destination
移除列表的最后一个元素， 并将该元素添加到另一个列表并返回
RPOPLPUSH a1 a2
a1 的最后元素移到 a2 的左侧
RPOPLPUSH a1 a1
循环列表，将最后元素移动到最右侧
BRPOPLPUSH source destination timeout
从列表中弹出一的元素插入到另外一个类标中并放回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
应用场景
项目常应用于：1. 对数据量大的集合数据删减 2、任务队列
1 对数据量数集合数据删减
列表数据显示、关注列表、粉丝列表、留言评价等。。分页、热点新闻（top5）等
利用LRANGE 还可以很方便的实现分页功能， 在博客系统中， 每篇博文的评论也可以存入一个单独的list 中
List 运用案例
接口方法
实现方法
单元测试
2. 任务队列
List 通常用来实现一个消息队列， 而且可以确保先后顺序，不必像 MySQL 那样还需要通过 ORDER BY 来进行排序
任务队列介绍（生产者和消费者模式）
在处理 Web 客户端发送的命令请求时，某些操作的执行时间可能会比我们预期的更长一些 通过将待执行任务的相关信息放入队列里面，并在之后对队列进行处理，用户可以通过推迟执行那些需要一段时间
才能完成的操作，这种将工作交给任务处理器来执行的做法成为任务队列 （ task queue）
RPOPLPUSH source destination
移除列表的最后一个元素， 并将该元素添加到另一个列表中返回
常用案例
订单系统的下单流程、用户系统登录注册短信等
订单系统的下单流程
接口方法
实现方法
单元测试
Set 类型
简介
Redis 的 Set 是 String 类型的无序集合， 集合成员是唯一的，这就以为这集合中不能出现重复的数据。
Redis 中集合通过哈希表来实现的， 所以添加，删除，查找的复杂度都是 O(1)
Redis 集合中最大的成员数量是 2^(32-1) （4294967295，每个集合可以存储 40 多亿个成员）
类似 Java 中的 HashTable 集合
总结
Redis 结合对象 set 底层存储结构特别神奇， 底层采用了 intset 和 hashtable 两种数据结构存储的， instset 我们理解为数组， hashtable 就是普通的哈希表（key 为 set 的值， value 为 null）
inset 内部世纪是一个数组 (int8_t coentents[] 数组)，而且存储数据的时候是有序的， 因为在查找数据的时候是通过二分查找来实现的。
命令
赋值语法
SADD key member1 [member2]
向集合添加一个或多个成员
取值语法
SCARD key
获取集合的成员数
SMEMBERS  key
返回集合汇总的所有成员
SISMEMBER  key  member
判断 member 元素是否是集合 key 的成员（开发中：验证是否存在的判断）
SRANDMEMBER key [count]
返回集合中一个或多个随机数
删除语法
SREM key member1 [member2]
移除集合中一个或多个成员
SPOP key [count]
移除并返回集合中的一个随机元素
SMOVE source destination member
将 member 元素从 source 集合移动到 destination 集合
差集语法
SDIFF key1 [key2]
返回给所有集合的差集（左侧）
SDIFFSTORE destination key1 [key2]
返回给定所有集合的差集并存储在 destination 中
交集语法
SINTER key1 [key2]
返回给定所有结合的交集（共有数据）
SINTERSTORE destination key1 [key2]
返回给定所有集合的交集并存储在 destination  中
并集语法
SUNION key1 [key2] 
返回所有给定集合的并集
SUNIONSTORE destincation key1 [key2]
返回给定结集合的并集存储在 destination 集合中
使用场景
常应用于：两个集合之间的数据 [计算] 进行交集、并集、差集运算
1. 非常方便的实现如共同关注、共同喜好、二度好友等功能。对上面的所有集合数据操作， 你还可以使用不同的命令选择将结果返回给客户端还是存集到一个新的集合中。
2. 利用唯一性，可以统计访问网站的所有独立 IP
ZSet （sorted set） 类型
简介
1. Redis 有序集合和集合一样也是 string 类型的集合，且不允许重复的成员。
2. 不同的是每个元素都会关联一个double 类型的分数。redis 正是通过分数来为集合中的成员进行从小到大的排序。
3. 有序集合的成员是唯一的，但分数（score）却是可以重复。
4. 集合是通过哈希表实现的，所以添加，删除，查找的复杂度是O(1) . 集合中最大的成员数是 2^(32-1) (4294967295,  每个集合可以存储 40 多亿个成员)
特点
Redis 的 ZSet 是有序的、且不重复
很多时候，我们都将 redis 中的有序集合叫做 zsets , 这是因为 redis  中，有序集合相关的操作都是 z  开头的。
命令
赋值语法
zadd key score1 member1 [score2 member2]
向有序集合添加一个或多个成员，或更新已存在成员的分数
取值语法
ZCARD key
获取集合的成员个数
ZCOUNT key min max
计算在有序集合中指定成员的个数
ZRANK key member
返回有序集合中指定的成员的索引
ZRANGE key start stop [WITHSCORES]
通过索引区间返回有序集合成指定区间内的成员 （低到高的排序）
ZREVRANGE key start stop [WITHSCORES]
返回有序集中指定区间内的成员，通过索引， 分数从高到低
删除语法
del key 
移除 key
ZREM key member [member .. ]
移除集合中的一个或多个成员
ZREMRANGEBYRANK key start stop
 移除有序集合中给定的排名区间所有成员（第一名是0）（低到高的排序）
ZREMRANGEBYSCORE key min max
移除有序集合汇总给定分数区间的所有成员
使用场景
常见运用： 排行榜
1. 比如 twitter 的 public timeline 可以发表时间作为 score 来存储， 这样获取的时候就是自动按照时间排序的结果集
2. 比如一个存储全班同学的 sorted set, 其集合value 可以是同学的学好， 而 score 就可以是其考试得分， 这样在数据插入集合的时候，就已经进行了天然的排序。
3. 还可以用 sorted set 来做待权重的队列，比如消息的 score 为 1, 重要的 score 为 2 ， 然后工作线程可以选择 按 score 来倒序获取工作任务。让重要的事情优先执行。
Redis 其他功能
发布订阅
简介
Redis 发布订阅 （pub/sub）是一种消息通讯模式：发送者（pub）发送消息， 订阅者（sub）接收消息
Redis 客户端可以订阅任意数量的频道
示例
命令
订阅频道
SUBSCRIBE channel [channel ...]
订阅给定的一个或多个凭悼的信息
PSUBSCRIBE pattern [pattern ...]
订阅一个或多个富豪给定模式的频道
发布频道
PUBLISH channel message
将信息发布到指定的频道
退订频道
UNSUBSCRIBE [channel [chennel ... ]]
退订给定的频道
PUNSUBSCRIBE [pattern [pattern ... ]]
退订素有给定模式频道
运用场景
这个一个功能最明显的用法即使构建实时的消息系统，比如普通的即时聊天，群聊等功能
1. 在一个博客网站中，有100个粉丝订阅了你，当你发布新文章，就可以推送消息给粉丝们
2. 微信公众号模式
多数据库
简介
Redis 下， 数据库是由一个证书索引标示， 而不是由一个数据库名称，默认情况下，一个客户端链接到数据库 0
命令
select 数据库
数据库的切换
move key 名称 数据库
移动数据（当前key 移动到另外一个数据库中）
flushdb
清除当前数据库的所有 key
flushall
清理整个 Redis 的数据库所有 key
缓存预热
Redis 事务
简介
Redis 时许可以一次执行多条命令（允许子啊一次单独的步骤中执行一组命令）， 并且带有一下两个重要的保证
批量操作在发送 exec 命令之前被放入队列缓存。 收到 EXEC 命令 后进行入事务执行队列，事务中任意命令势必啊，其余的命令依然被执行。
在事务执行过程，其他的客户端提交的命令不会插入到事务执行命令序列中。
1. Redis 会将一个事务中的所有命令序列化，然后按照顺序执行
2.  执行中不会被其他的命令插入，不允许加塞行为
事务执行的三个阶段
开始事务
命令入队
事务执行
命令
DICARD
取消事务
EXEC
执行所有事务块呢的命令
MULTI
标记一个事务块的开始
UNWATCH
取消 WATCH 命令对所有 key 的监视
WATCH key [key ...]
监视一个或多个 key , 如果在事务执行之前（或这些）key 被其他命令锁改动那么事务将被打断
示例
1. MULTI EXEC
A 账户向 B 账户转账 50 元（A - 50， B + 50）
1.  Multi 命令开始，输入的命令都会一次进入命令队列中，但不会执行
2. 直到输入 Exec 后，Redis 会将之前队列中的命令依次执行
3. 完整命令
2. DISCARD 放弃队列
1. 输入 multi 命令开始，输入的命令都会依次进入命令队列汇总，但不会执行
2. 直到输入 Exec 后，Redis 会将之前的命令队列中的命令依次执行
3. 命令队列的过程也可以通过 discard 来放弃队列的运行
命令示例
3. 事务的错误处理
如果执行了某个命令报出了错误，则只有报错的命令不会被执行，而其他的命令都会执行，不会回滚
命令示例
4. 事务的错误处理
事务队列中的某个命令报告错误，执行了整个所有队列都会被取消。
命令示例
5. 事务的WATCH
WATCH  key
监视一个（或多个）key, 如果在事务执行这个（或这些）key 被其他命令所改动， 那么事务将被打断
需求：某一个账户在一个事务内进行操作，在提交事务之前，另一个进程对该账户进行操作
6. UNWATCH
Redis Unwatch 命令用于取消 WATCH 命令对所有 key 的监视。 如果在执行 WATCH 命令之后， EXEC 命令或 DISCARD 命令先被执行的话，那就不需要再执行UNWATCH 了
应用场景
一组命令必须同时都执行，或者都不执行。 我们想要保证一组命令在想执行过程中不被其他命令插入。
商品秒杀（活动） 转账活动
Rediscover数据淘汰策略 redis.conf
最大缓存配置
在Redis 中，允许用户设置最大的内存大小
maxmemory 512G
redis 提供的 8 种数据淘汰策略
1. volatile-lru
设定超时时间的数据中，删除最不常用的数据
2. allkeys-lru
查询所有的key 中最不常使用的数据进行删除，这是应用最广泛的策略
3. volatile-random
在已经设定了超时的数据中随机删除
4. allkeys-random
查询所有的 key 之后随机删除
5. volatile-ttl
查询全部设定超时时间的数据，追后马上排序，将马上将要过期的数据进行删除操作
6. noeviction (默认)
如果设置为该属性，则不会进行删除操作，如果内存溢出则报错返回
7. volatile-lfu
从所有配置了过期的时间的键中驱逐使用频率最少的键
8. allkeys-lfu
从所有键中驱逐使用频率最少的键
建议：了解Redis 的淘汰策略之后，在平时使用尽量主动设置/更新 key 的 expire 时间 主动提出不活跃的旧数据， 有助于提升查询性能
Redis 持久化
RDB（默认机制）
是什么
RDB相当于快照，保存的是一种状态几十G数据 -> 几KB的快照
快照是默认的持久化方式，这种方式是将内存中的数据以快照的方式 写入到二进制的文件中，默认的文件名是 dump.rdb
优缺点
优点
快照保存数据极快、还原数据极快
适用于灾备
缺点
小内存的机器不适合使用
快照的条件
1. 服务器正常关闭时
./bin/redis-cli shutdown
2. key 满足一定条件会进行快照
vim redis.conf (搜索 save)
AOF
是什么
配置的三种方式（默认每秒 sync 一次）
 启用AOF 持久化发方式
appendonly yes 
1. 收到写命令就立即写入磁盘，最慢，但是保证完全的持久化
appendfsync always
2. 每秒钟写入磁盘一次，在能能和持久化方面做了很好的折中
appendfsync everysec
3. 完全依赖OS， 性能好，持久化没有保障
appendfsync no
产生的问题
AOF 的方式也同时带了了另外一个问题。持久化文件会越来越大。 例如我们调用incr test 命令100次，文件必须保存全部命令，那么其中有99条命令都是多余的。
Redis 缓存与DB一致性
解决方案
1. 实时同步
对要求强一致性比较高的，应采用实时同步方案，即查询缓存查询不到再从DB查询，保存到缓存 更新缓存时，先更新数据库，再将缓存的设置过期（建议不要去更新缓存内容，直接设置缓存过期）
@Cacheable： 查询使用，注意Long 类型需要转换为String类型， 否者会抛异常 @CachePut: 更新使用，使用此注解，一定会从 DB上查询数据
@CacheEvict: 删除使用
@Caching: 组合用法
如果数据修改了，缓存也要修改 修改缓存数据或让缓存KEY失效
热点数据（缓存穿透）
中国奥运会金牌数量？
查询数据库，并将 KEY 设置进入 （查询数据库的时候需要加锁）
如果存在，直接查询可以
2. 异步同步
对于并发程度较高的，采用一步队列的方式同步，可采用 Kafka 等消息中间件来处理消息的生产和消费。
3. 采用阿里的同步工具canal
cannal 实现方式是模拟 mysql slave 和 master 的同步机智，监控DB bitlog 的日志来更新触发缓存的更新， 此中方法可以释放开发者的双手，但在使用时有一些局限性。
工作原理
1. canal 模拟 mysql slave 的交互模式，伪装自己是 mysql slave ， 向mysql master 发送dump 协议
2. mysql master 收到 dump 请求，开始推送 binary  log 给 salve (也是 canal)
3. canal 解析 binary log 对象（原始为 byte 流）
4.采用UDF自定义函数
面对 mysql 的 api 进行变成， 利用触发器进行缓存同步， 但是 UDF 主要是 C/C++语言实现学习成本较高
总结
1. 穿透
是什么？
缓存传统指的是查询一一定不存在的数据，由于缓存不命中时需要从数据库中查询，查询不到数据则不写入缓存， 则不写入缓存，浙江到这这个不存在的数据每次请求都要到数据库去查询，造成缓存穿透
如何处理？
持久层查询不到就缓存空结果， 查询时先判断缓存是否 exists(key) ， 如果有直接放回空，么有则查询后返回。
注意 insert 时需要清除好查询的 key , 否则即便DB中查询有值也查询不到（当让也可以设置缓存的过期时间）
2. 雪崩
是什么？
缓存大量失效的时候，引发大量查询数据库
如何处理？
1. 用锁/分布式锁或者队列串行访问
2. 缓存失效时间均匀分布
3. 热点key
是什么？
某个key 的访问频率非常频繁，当 key 失效的时候有大量线程来构建缓存，导致复杂增加，系统崩溃
如何处理？
1. 使用锁， 单机使用sychronized，lock 等，分布式系统采用分布式锁
2. 缓存过期时间不设置， 而是设置在 key 对应的 value 里。如果检测到存的时间超过过期时间则一步更新缓存。
3. 在 value 设置一个比过期时间 t0 小，过期时间 他 ， 当t1 过期时候，延长 t0 并做更新缓存操作。
4. 设置标签缓存，标签缓存设置过期时间，标签缓存过期后，需一步地更新实际缓存
Redis 高并发/高可用
1. Redis 存在的问题 ?
一般来说要将 Redis 运用到工程中， 只是用一台 Redis 是万万不能，原因如下：
1. 从结构上， 单个 Redis 服务器会发生单点故障， 并且一台服务器需要处理所有的请求负载，压力较大。
2. 从容量上， 单个 Redis 服务其内存容量有限，就算一台 Redis 服务器内容容量 256G， 也不能将所有的内容用作 Redis 存储内存， 一般来说单台的最大使用内存不应该超过 20G
2. 高并发基本概述
1. 高可用
“高可用” （High Availability）通常来面试一个系统经过专门的设计，从而减少停工时间，而保持其服务的高度可用性。
2. 高并发
高并发（High Concurrency）是互联网分布式系统架构设计中必须考虑的因素之一，它通常是指， 通过设计保证系统能够同时并行处理很多请求。
并发相关的指标有：相应时间（Response Time）、吞吐量（Troughput）、 每秒查询率 QPS （Query Per Second），并发用户等。
响应时间
系统请求作出的响应时间。例如系统处理一个 HTTP 请求需要 200ms , 这就是系统的响应时间
吞吐量
单位时间内处理的请求数量
每秒查询率 QPS
每秒响应请求数，在互联网领域，这个指标和吞吐量区分的没有这么明显
并发用户数
同时曾丹正航使用系统功能的用户数量。例如一个即时通讯系统，同时在线里那个一定程度代表了系统的并发用户数。
3. 提升系统的并发能力
提高系统并发能力的方式，方法论上主要有两种：
1. 垂直拓展（Scal UP）
1. 增强当即硬件性能。
例如：增加 CPU 核数如 32 核，升级更好的网卡如万兆， 升级更好的硬盘如 SSD，拓充硬盘容量如 2T， 拓充系统内存如 128G
2. 提升单机架构性能。
例如： 使用 Cache 来减少 IO次数，使用异步来增加单服务吞吐量，使用无锁数据结构来减少响应时间。
在互联网发展非常迅猛的早期， 如果预算不是问题，强烈建议使用  “增加单机硬件性能” 的方式提升系统并发能力，因为这个阶段，公司的战略往往是发展业务抢时间，而“增强单机硬件性能”往往是最宽的方法。
总结：不管是提升单机硬件性能，还是提升单机架构性能，都有一个致命的不足，单机性能总是有极限的。 所以互联网分布式架构高并发总计解决方案还是水平拓展。
2. 水平拓展（Scale Out）
就是增加服务器的数量，就能线性的拓充系统性能。水平拓展对系统架构设计是有要求的， 难点在于：如何架构各层进行水平拓展的设计
3. Redis 主从复制
简介
应用场景： 电子商务往回走哪上面的商品，一般都是一次上传， 无数次浏览的，说专业一些就是 “多读少写”
生么是主从复制？
一个 Redis 服务其可以有多个该服务的复制品， 这个服务器称为 Master， 其他的复制称为是 slaves
实现原理
结构图
如图所示， 我们将一台 Redis 服务其作为主库（Master）,其他的三台作为从库 （Slave）, 主库负责写数据，每次有数据更新豆浆更新的数据同步到它所有的从库，而从库只负责读数据。
这样依赖，就有2个好处：
1. 读写分离，不仅可以提高服务器的负载能力，并且可以根据读请求的规模自由增加或者减少从库的数量
2.  数据库复制成了好几份，就可算一台机器出现了故障，也可以使用其他的机器
具体配置
4 .Redis Cluster 集群
简介
为什么使用集群？
为了在大流量访问下提供稳定的业务，集群化是存储的必然形态
未来的发展趋势肯定是云计算和大数据的紧密结合
只有分布式架构才能满足需求
集群的方案
1. Twitter  开发的 twemproxy
2. 豌豆荚开发的 codis
3. redis 官方的 redis-cluster
总结：Redis 集群大家的方式有很多种， 但是从  redis 3.0 之后版本支持 redis-cluster 集群。 至少需要 3（Master） + 3 （Slave）才能建立集群。 Redis-Cluster 采用无中心结构，
每个绩点保存数据和整个集群状态，每个节点都和其他所有节点链接，其 redis-cluster 架构图如下所示。
redis-cluster 架构图
集群的特点
1. 所有的节点彼互联 PING - PONG 机制， 内部采用二进制协议优化传输速度和带宽
2. 节点的 fail 是通过集群中超过半数的节点检测失效时才失效
3. 客户端与 redis 直连，不需要中间 proxy 层， 客户端不需要连接集群所有节点， 连接集群中任何一个节点即可
4. redis-cluster  把所有的物理节点映射到 0-16383 slot 上（不一定时平均分配）cluster 负责维护
5. Redis 集群预分好 16384 个哈希槽， 当需要在 Redis 集群中放置一个 key-value 时， redis 先对 key 使用 crc16 算法酸楚一个结果，然后把结果对 16384 求余数， 这样每个 key
都会对应一个编号在 0-16383 之间的哈希槽， redis 会更具节点数量大致均等的将哈希槽映射到不同的节
redis-cluster 容错性
容错性，是指软件检测应用运行的软件或硬件中发生错误并错误中回复能力， 通常可能从系统的可靠性，可用性，可测性等几方面来衡量
redis-cluster 投票：容错
1. 投票过程是集群中所有 master 参与 ，如果半数以上 master 节点与 master 节点通讯超时 （master -node -timeout） 认为当前 master 节点挂掉
2. 什么时候整个集群不可用？（cluster_state:fail）
如果集群任意master 挂掉， 且当前 master 没有 slave .  集群进入 fail 状态， 也可以理解成集群的 slot 映射 [0 - 16383] 不完整进入 fail 状态，
如果集群超过半数以上 master 挂掉， 无论是否有 slave , 集群进入 fail 状态。
redis-cluster 节点分配
（官方推荐）三个绩点分别是 A，B，C三个阶段，他们可以是一台机器上的三个端口， 也可以是 三台不同的服务器。那么采用哈希槽（hash slot）的方式来分配 16384 个 slot 的化，
他们三个节点分别承担的 slot 区间是 ：
节点 A 覆盖 0- 5460
节点 B 覆盖 5461 - 10922
节点C 覆盖 10923 - 16383
如果有新增节点
新增节点 D redis-cluster 的这种做法是从各个节点前面各取一部分 slot 到 D上
新增节点后
节点 A 覆盖 1365 - 5460
节点 B 覆盖 6827 - 10922
节点 C 覆盖 12288 - 16383
节点D  0 - 1364， 5461 - 6826，10923 - 12287 
redis-cluster 集群搭建
简介
集群中至少应该有奇数个绩点，所以搭建集群最少需要3台主机。同时每个节点至少有一个备份节点， 所以下面最少需要创建使用 6 台机器，才能完成 Redis-Cluster 集群（主节点、备份节点由 redis-cluster 集群确定）
集群搭建流程
1. 创建集群的安装目录
mkdir -p /usr/local/redis_cluster
2. 在redis_cluster 目录下，创建 7001- 7006 个文件夹
mkdir 7001 7002 7003 7004 7005 7006
3. 并将 redis-cluster 分别拷贝到 7001-7006 文件夹下
4. 分别修改如下配置文件， 修改如下内容 同时 protected-mode 为了禁止公网访问 redis cache , 加强redis 安全性，它的启用条件有两个：
1) 没有绑定IP
2） 没有设置访问密码
由于 Linux 上的 redis 处于安全保护模式， 这就让你无法从虚拟机外部去轻松建立链接。
如果外部无法访问：redis.conf 中设置保护模式为 protected-mode no
修改的配置
bind 0.0.0.0
绑定服务器的 IP
port 7001
绑定端口号
pidfile /var/run/redis-7001.pid
修改pid 进程文件名，以端口号命名
logfile /root/application/program/redis-cluster/7001/redis.log
修改日志文件名，用端口号来区分
dir /root/appliaction/program/reid-cluster/7001/
修改数据文件存放的地址，以端口号为目录来区分
cluster-enabled yes
启用集群
cluster-config-file nodes-7001.conf
配置每个绩点的配置文件，同样端口号为文件名
cluster-node-timeout 15000
配置集群节点的超时时间，可以不改变
appendonly yes
启用AOF来增强持久化策略
appednfsync always
发生改变就持久化策略
deamonize yes
后台与性能
5. 启动各个 Redis 节点
将 redis-x.x.x/ 下的 src 为难拷贝到各个 redis 7001 - 7006 目录下
cd redis-x.x.x/
cp -r ./src /usr/local/redis_cluster/7001 (拷贝到 7001 - 7006)
启动各个节点：
cd /usr/local/redis_cluster
./7001/src/redis-server ./7001/redis.conf 依次启动 7001-7006
创建集群(4.x 安装过于复杂)
Redis 官方提供了一个 redis-trib.rb 这个工具，就在解压目录的 src 目录中 （为了方便操作）将其复制到 /usr/local/bin 目录下， 可以直接访问此命令
cd redis-x.x.x
cd src 
cp redis-trib.rb /usr/local/bin/
直接在命令中执行： ip:port 格式
redis-trib.rb create --replicas 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 127.0.0.1:7006
如果出现这个错误
/usr/bin/env: ‘ruby’: No such file or directory 需要安装 ruby
安装 ruby
yum -y install ruby ruby-devel rubygems rpm-build
gem install redis
安装 rvm
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable
source /usr/local/rvm/scripts/rvm
rvm list known
find / -name rvm -print
升级 ruby
rvm install 2.4.4
rvm use 2.4.4
rvm use 2.4.4 --default
ruby --version
gem install redis
创建集群(推荐)
./redis-cli --cluster create 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 127.0.0.1:7006 --cluster-replicas 1 -a Iredis8Pwd
说明
注：redis5版本可以直接使用redis-cli命令配置集群(内部集成ruby)
--cluster-replicas 1：表示主从比例1:1 (一台主机对应有一台从机)

--cluster-replicas 2：表示主从比例1:2 (一台主机对应有两台从机)

-a root123456：配置集群时所需的认证密码 (-a：auth简写，root123456：redis服务器认证密码）
 成功提示
[OK] All nodes agree about slots configuration. >>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
redis-cluster 测试
1. 复制 redis-cli 客户端到 /usr/local/bin 下
cp redis-cli /usr/local/bin/
2. 访问集群
redis-cli -h 127.0.0.1 -c -p 7001
加参数 -C 可连接到集群。因为上面redis 将bind 修改为了 0.0.0.0 ，所以 -h 也是可以省略的
cluster nodes
查询节点
小总结
3. 测试截图
5. 高并发常见问题？
缓存穿透
介绍
缓存穿透个是指查询一个一定不存在的数据，由于缓存是不命中时需要从数据库查询， 查不到数据则不写入缓存，这将导致这个不存在的数据每次都需要到数据库中去查询，造成缓存穿透
解决方案
持久层查询不到就缓存空结果， 查询时先判断缓存中是否 exists(key), 如果又直接放回空，没有则查询后返回
注意 insert 时需要清除查询的 key , 否则即便 DB 中有值也查询不到（当让也可以设置空缓存的过期时间）
例子
1. 数据自增ID从0 开始， 如果查询 id = -1 的数据
2. 高并发 10000 人访问
3. 由于缓存中没有数据，所以每次都会去查询数据库。
4. 如果数据库中查询出来为 null 可以缓存一个 "" 空字符串
缓存雪崩
介绍
如果缓存集中在一个时间段内失效，发生大量的缓存穿透，所有的查询都落到了数据库上，造成缓存雪崩。
这个没有完美的解决办法，但是可以分析用户的行为，尽量让失效时间均匀分布。 大多数系统设计者考虑用加锁或队列的方式来把整缓存的单线程（进程）写，
从而避免失效时大量的并发请求落到底层的存储系统上
解决方案
1. 加锁排队，限流算法。
计数
2. 滑动窗口
3. 令牌桶 Token Bucket
4. 漏桶 leaky bucket
总结
在缓存失效的时候，通过加锁或者队列来控制读数据库写缓存的线程数量。 比如对某个key只允许一个线程查询和写缓存，其他线程等待
Jedis 错误
No reachable node in cluster
1. JedisNoReachableClusterNodeException：No reachable node in cluster
2. 错误截图
截图
3. 问题分析
1. 无可用连接，可能是存在较多的连接占用，新请求无法获取连接
4. 解决方案
增大连接数解决
Redis 分布式锁
RedLock
1. 在没有 fencing栅栏机制的前提下不安全
概述： 获取锁的客户端在持有锁时可能会暂停一段较长的时间，尽管锁有一个超时时间，避免了崩溃的客户端可能永远持有锁并且永远不会释放它，但是如果客户端的暂停持续的时间长于锁的到期时间，并且客户没有意识到它已经到期，那么它可能会继续进行一些不安全的更改，换言之由于客户端阻塞导致的持有的锁到期而不自知。
问题流程图
解决方案
