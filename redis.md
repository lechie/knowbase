# 连接
```
redis-cli -h 127.0.0.1 -p 6379 -a "password"
```
>有时候会有中文乱码,可以使用 `redis-cli --raw` 避免乱码

-|-
-|-
auth "password"| 验证密码
ping | 检查是否通，pong 表示通
echo "hello" | 输出 msg
quit |关闭连接
select 0 <br> select 1 | 选择0/1号数据库

# 配置
```
config get *

config get loglevel
config set loglevel "notice"
config get loglevel
```
## 参数说明
参数|说明
-|-
daemonize no| 默认不以守护进程的方式运行，可设为 yes 启用守护进程
pidfile /var/run/redis.pid | 当以守护进程方式运行时，默认会把 pid 写入 /var/run/redis.pid
port 6379|指定 Redis 监听端口，默认 6379
bind 127.0.0.1|绑定的主机地址
timeout 300	|当客户端闲置多长秒后关闭连接，0表示关闭该功能
loglevel notice	|日志记录级别，支持：debug、verbose、notice、warning，默认为 notice
logfile stdout|日志记录方式，默认为标准输出，<br>如果为守护进程方式运行，而这里又配置为标准输出，则日志将会发送给 /dev/null
databases 16|设置数据库的数量，默认数据库为0，可以使用SELECT 命令在连接上指定数据库id
slaveof <masterip> <masterport>	|设置当本机为 slav 服务时，设置 master 服务的 IP 地址及端口，<br>在 Redis 启动时，它会自动从 master 进行数据同步
masterauth <master-password>|当 master 服务设置了密码保护时，slav 服务连接 master 的密码
requirepass foobared|设置连接密码，默认关闭<br>如果配置了密码，客户端在连接 Redis 时需要通过 AUTH <password> 提供密码，
maxclients 128	|设置同一时间最大客户端连接数，默认无限制<br>设置为 0表示不限制<br>Redis 同时打开的客户端连接数为 Redis 进程可打开的最大文件描述符数。<br>当到达限制时，Redis 会关闭新连接, <br>并向客户端返回 max number of clients reached 错误
maxmemory <bytes>|最大内存限制，<br>Redis 在启动时会把数据加载到内存中，<br>达到最大内存后，会先尝试清除已到期或即将到期的 Key，<br>之后若仍到达最大内存设置，将无法进行写操作，<br>但仍可以进行读操作。<br>Redis 新的 vm 机制，会把 Key 存放内存，Value 会存放在 swap 区

参数|说明
-|-
save <seconds> <changes>|Redis 默认配置文件中提供了三个条件：<br>save 900 1 #表示 900 秒内有 1 个更改<br>save 300 10 #300 秒内有 10 个更改<br>save 60 10000 #60 秒内有 10000 个更改。<br>指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可多个条件配合
rdbcompression yes|指定存储至本地数据库时是否压缩数据，默认为 yes<br>Redis 采用 LZF 压缩，如果节省 CPU 时间可关闭该项，但会导致数据库文件变的巨大
dbfilename dump.rdb|指定本地数据库文件名，默认 dump.rdb
dir ./|指定本地数据库存放目录
appendonly no|指定是否在每次更新操作后进行日志记录，默认为 no，默认异步把数据写入磁盘<br>如果不开启，可能会在断电时导致一段时间内的数据丢失。<br>因 redis 同步数据文件是按 save 条件同步的，<br>所以有的数据在一段时间内只存于内存中。
appendfilename appendonly.aof|指定更新日志文件名，默认 appendonly.aof
appendfsync everysec|指定更新日志条件，共有 3 个可选值：<br>no：表示等操作系统进行数据缓存同步到磁盘（快）<br>always：表示每次更新操作后手动调用 fsync() 将数据写到磁盘（慢，安全）<br>everysec：表示每秒同步一次（折中，默认值）
vm-enabled no|指定是否启用虚拟内存机制，默认值为 no，<br>VM 机制将数据分页存放，<br>由 Redis 将访问量较少的页即冷数据 swap 到磁盘上，<br>访问多的页面由磁盘自动换出到内存中
vm-swap-file /tmp/redis.swap|虚拟内存文件路径，默认值为 /tmp/redis.swap，不可多个 Redis 实例共享
vm-max-memory 0	|将所有大于 vm-max-memory 的数据存入虚拟内存，默认值为 0<br>无论 vm-max-memory 设置多小，所有索引数据(即keys)都是内存存储的，<br>即当 vm-max-memory 设置为 0 ，是所有 value 都存在于磁盘。
vm-page-size 32	|Redis swap 文件分成了很多的 page，一个对象可以保存在多个 page 上面，<br>但一个 page 上不能被多个对象共享，<br>vm-page-size 是要根据存储的 数据大小来设定的，<br>作者建议如果存储很多小对象，page 大小最好设置为 32 或者 64bytes；<br>如果存储很大大对象，则可以使用更大的 page，<br>如果不确定，就使用默认值
vm-pages 134217728|设置 swap 文件中的 page 数量，<br>由于页表（一种表示页面空闲或使用的 bitmap）是在放在内存中的，<br>在磁盘上每 8 个 pages 将消耗 1byte 的内存。
vm-max-threads 4 | 设置访问swap文件的线程数,默认值为4<br>最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。
glueoutputbuf yes | 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启
hash-max-zipmap-entries 64<br>hash-max-zipmap-value 512 |指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
activerehashing yes | 指定是否激活重置哈希，默认为开启
include /path/to/local.conf | 指定包含其它的配置文件，<br>可以在同一主机上多个Redis实例之间使用同一份配置文件，<br>而同时各个实例又拥有自己的特定配置文件


# 数据类型
支持五种数据类型：
- string（字符串）
- hash（哈希）
- list（列表）
- set（集合）
- zset(sorted set：有序集合)
 
## String
二进制安全
可以包含任何数据。比如jpg图片或者序列化的对象
最大能存储 512MB
```
>set aa "nihao"
>get aa
>del aa
```

## Hash
键值(key=>value)对集合, string 类型的 field 和 value 的映射表
特别适合用于存储对象
每个 hash 可以存储 2^32 -1 键值对（40多亿）
```
>hmset bb key1 "hi" key2 "world"
>hset bb key3 "yeah"
>hsetnx bb key4 "ok" #当不存在时设置
>hget bb key1
>hget bb key2
>hgetall bb
>hexists bb key1

>hkeys bb
>hvals bb

>hlen bb
>hdel bb key1
```
以下获取报错
```
>get abc
>hget abc 
```

## List
简单的字符串列表，按照插入顺序排序

可以添加一个元素到列表的头（左边）或者尾（右边）
```
> lpush key1 redis
> lpush key1 mysql
> lrange key1 0 10
```

命令/用法示例|说明
-|-
lpush key val1 val...n|`lpush mylist hello world`<br>将n个值插入到列表头部 **注意插入顺序**<br>如果 key 不存在，会创建空列表后再插入
rpush key val1 val...n|`lpush mylist hello world`<br>将n个值插入到列表尾部 **注意插入顺序**<br>如果 key 不存在，会创建空列表后再插入
lpushx key val1 val...n|插入头部，列表不存在无效
rpushx key val1 val...n|插入尾部，列表不存在无效
lpop key|移出一个列首元素并返回
rpop key|移出一个列尾元素并返回
blpop lst1 lst...n timeout|`blpop mylist 100`<br>移出一首元素并返回，<br> 如果没有元素会阻塞直到超时或可弹出元素为止
brpop lst1 lst...n timeout| `brpop mylist 100`<br>移出一尾元素并返回，<br> 如果没有元素会阻塞直到超时或可弹出元素为止
rpoplpush lst_from lst_to |从lst_from 弹出末尾元素,<br>并插入 lst_to 首位
brpoplpush lst_from lst_to timeout|从lst_from 弹出末尾元素,<br>并插入 lst_to 首位，<br>如果没有元素会阻塞直到超时或可弹出元素为止
lindex lst index<br>`lindex mylist 0`<br>`lindex mylist -1`|用索引形式获取元素，支持负数
lrange key start stop| `lrange mylist 0 3` | 0 为第一个元素,<br> 1 为第二个... <br>-1 为最后一个,<br>-2 为倒数第二个...
linsert key before/after pivot val<br>`linsert mylist after "hello" "world"`|在hello后面插入world
llen mylist|获取列表长度
lrem mylist count val|移除列表中count 个值=val的元素<br>count > 0 : 从表头向尾搜，移除count个<br>count < 0 : 从表尾向前搜，移除\|count\| 绝对值个<br>count = 0 : 移除所有
lset mylist index val | 通过索引设置列表元素的值
ltrim mylist 3 5<br/>`ltrim mylist 3 -2`| 只保留[3-5]区间的元素，其他都trim掉

# Set 集合
String 类型的无序集合。集合成员是唯一的

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)

最大的成员数为 2^32 - 1 (40多亿)

命令/用法示例|说明
-|-
sadd mykey val1 val2 | 插入多个元素，没有则创建
|
sismember mykey val| val是不是mykey集合成员
smembers mykey| 列出所有成员,key不存在列出空
scard mykey|返回集合元素数量
sscan mykey cursor <br> sscan mykey 0 match h* 2|迭代集合中的元素，例：从索引0开始往后，找出 h 开头的元素
|
sdiff key1 key2 key3|返回差集，差集是mykey1中与后面不同的
sdiffstore mystorekey mykey1 mykey2 mykey3| 将差集存在 mystorekey 中，弱存在则覆盖
sinter key1 key2 key3|求交集，若有一个空集则结果为空集
sinterstore mystorekey key1 key2 key3|
sunion key1 key2 key3|求并集
sunionstore mykey key1 key2 key3|求并集后存到mykey，存在则覆盖
|
smove src dst val| 把 val 从 src 移至 dst，若dst中已存在，覆盖（或者理解为只是src中删除）
srem myset val1 valn|移除集合中的元素
spop myset<br>spop myset 2|从myset 移除n个 **随机** 元素并返回
srandmember myset <br>srandmember myset 3 | 返回集合若干个随机元素，原集合不变<br>若count 为负数，那么命令返回一个数组，数组中的元素可能会重复出现多次，而数组的长度为 count 的绝对值。

# Sorted Set 有序集合
有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序

有序集合的成员是唯一的,但分数(score)却可以重复。


命令/用法示例|说明
-|-
zadd myzset score1 val1 score.n val.n | 插入，如果val已存在，则更新分数值，并重新插入来保证其在正确的顺序上
zrem myzset val val.n|移除n个成员
zremrangebyrank myzset 5 10|移除集中指定排名区间的成员（不是score）
zremrangebylex myzset [aa [ccc|移除有序集合中给定的字典区间的所有成员
zlexcount myzset min max<br>zlexcount myzset - +<br/>zlexcount myzset [a [d|指定区间内成员数量
zcard key| 返回有序集基数（是个数字）
zcount key min max|返回分数值在 [min, max] 之间成员数量
zscore key val|输出val的分数值
|
zincrby myzset number val | 对val执行 +number 操作（负值为减去），如果key 不存在，则创建，score=number
|
zrange myzset 0 -1 withscores | 带分数输出,相同分数值的成员按字典序, -1 表示最后一个成员,可以用 0 -1 显示整个有序集
zrevrange myzset 0 -1 withscores|同zrange，但按分数值递减(从大到小)来排列
zrangebylex myzset - [c <br>zrangebylex myzset - (c<br>zrangebylex myzset [a (c|通过字典区间返回有序集合的成员
zrangebyscore myzset (1 5<br>zrangebyscore myzset -inf +inf withscores|显示指定分数区间的成员，1 < score <= 5，-inf +inf 为整个区间
zrevrangebyscore 100 20<br>zrevrangebyscore +inf -inf| 同zrangebyscore按分数值递减(从大到小)的次序排列，相同分数值的成员按字典序的逆序
|
zrank myzset val|如果val是有序集成员返回排名，排名是按分数值递增(从小到大)顺序排列,最小的是0，否则返回nil
zrevrank myzset val|返回有序集中成员的排名，按分数值递减(从大到小)排序，最大的排名是0
zscan myzset 0 match a* count 2| 从位置0开始查找a开头的，最多2个，会返回成员+分数
|
zinterstore dest src_count src1 src2 src3 [weights w1 w2 w3] [aggregate sum/min/max ]| 求交集,其中结果集中某成员的分数值默认是所有给定集该成员分数之和，src_count 必须显式给指定出来
zunionstore dest src_count src1 src2 src3 [weights w1 w2 w3] [aggregate sum/min/max ]|求并集，默认结果集成员分数是集合下所有成员的分数之和



# 复杂度
在redis sorted sets里面当items内容大于64的时候同时使用了hash和skiplist两种设计实现。这也会为了排序和查找性能做的优化。所以如上可知： 

添加和删除都需要修改skiplist，所以复杂度为O(log(n))。 

但是如果仅仅是查找元素的话可以直接使用hash，其复杂度为O(1) 

其他的range操作复杂度一般为O(log(n))

当然如果是小于64的时候，因为是采用了ziplist的设计，其时间复杂度为O(n)


# HyperLogLog
Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

### 什么是基数?
比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 **基数估计**就是在误差可接受的范围内，快速计算基数。

-|-
-|-
pfadd mykey val1 val2 valn| 添加元素参数到 HyperLogLog 数据结构中
pfcount key <br>pfcount key1 key2 keyn | 返回给定 HyperLogLog 的基数估算值,如果同时统计多个 HyperLogLog ，则返回基数估值之和
pfmerge dstkey srckey1 srckey2 | 将多个 HyperLogLog 合并为一个 HyperLogLog ，合并后的 HyperLogLog 的基数估算值是通过对所有 给定 HyperLogLog 进行并集计算得出的

# 各个数据类型应用场景
类型|简介|特性|场景
-|-|-|-
String(字符串)	|二进制安全|可以包含任何数据,比如jpg图片或者序列化的对象,一个键最大能存储512M|-
Hash(字典) | 键值对集合,即编程语言中的Map类型|	适合存储对象,并且可以像数据库中update一个属性一样只修改某一项属性值(Memcached中需要取出整个字符串反序列化成对象修改完再序列化存回去)|存储、读取、修改用户属性
List(列表)|链表(双向链表)|增删快,提供了操作某一段元素的API	|1,最新消息排行等功能(比如朋友圈的时间线) <br>2,消息队列
Set(集合)|哈希表实现,元素不重复|1、添加、删除,查找的复杂度都是O(1) <br>2、为集合提供了求交集、并集、差集等操作|1、共同好友<br>2、利用唯一性,统计访问网站的所有独立ip<br>3、好友推荐时,根据tag求交集,大于某个阈值就可以推荐
Sorted Set(有序集合) | 将Set中的元素增加一个权重参数score,元素按score有序排列|	数据插入集合时,已经进行天然排序|1、排行榜<br> 2、带权重的消息队列


---
# 补充知识
## 数据库
Redis支持多个数据库，并且每个数据库的数据是隔离的不能共享，并且基于单机才有，如果是集群就没有数据库的概念。

Redis是一个字典结构的存储服务器，而实际上一个Redis实例提供了多个用来存储数据的字典，客户端可以指定将数据存储在哪个字典中。这与我们熟知的在一个关系数据库实例中可以创建多个数据库类似，所以可以将其中的每个字典都理解成一个独立的数据库。

每个数据库对外都是一个从0开始的递增数字命名，Redis默认支持16个数据库（可以通过配置文件支持更多，无上限），可以通过配置databases来修改这一数字。客户端与Redis建立连接后会自动选择0号数据库，不过可以随时使用SELECT命令更换数据库，如要选择1号数据库：
```
redis> SELECT 1
OK
redis [1] > GET foo
(nil)
```

然而这些以数字命名的数据库又与我们理解的数据库有所区别。
首先Redis不支持自定义数据库的名字，每个数据库都以编号命名，开发者必须自己记录哪些数据库存储了哪些数据。

另外Redis也不支持为每个数据库设置不同的访问密码，所以一个客户端要么可以访问全部数据库，要么连一个数据库也没有权限访问。

最重要的一点是多个数据库之间并不是完全隔离的，比如FLUSHALL命令可以清空一个Redis实例中所有数据库中的数据。

综上所述，这些数据库更像是一种命名空间，而不适宜存储不同应用程序的数据。

比如可以使用0号数据库存储某个应用生产环境中的数据，使用1号数据库存储测试环境中的数据，但不适宜使用0号数据库存储A应用的数据而使用1号数据库B应用的数据，不同的应用应该使用不同的Redis实例存储数据。

由于Redis非常轻量级，一个空Redis实例占用的内在只有1M左右，所以不用担心多个Redis实例会额外占用很多内存。

# Redis 发布订阅

订阅
```
subscribe redischat
```

发布消息
```
publish redischat "im fine"
publish redischat "oook"
```


-|-
-|-
subscribe mych1 mych2...|订阅给定的一个或多个频道的信息
publish mychannel "hello"|发布消息到指定的频道
unsubscribe mych1 mycha2|退订消息
psubscribe | 命令订阅若干个符合给定模式的频道, 模式以\* 作为匹配符，例：<br>it\* 匹配 it 开头的频道：it.news、  it.blog、 it.tweets<br>news.\* 匹配 news. 开头的频道：news.it、 news.global 
pubsub <br>pubsub channels| 查看订阅与发布系统状态，它由数个不同格式的子命令组成

# Redis 事务

**Redis 事务的执行并不是原子性的。**

> Redis 事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。

示例：
```
redis 127.0.0.1:7000> multi
OK
redis 127.0.0.1:7000> set a aaa
QUEUED
redis 127.0.0.1:7000> set b bbb
QUEUED
redis 127.0.0.1:7000> set c ccc
QUEUED
redis 127.0.0.1:7000> exec
1) OK
2) OK
3) OK
```

-|-
-|-
multi|标记一个事务块的开始。<br>事务块内的命令按照先后顺序被放进队列中，最后由 EXEC 命令原子性(atomic)地执行。
exec | 执行所有事务块内的命令
discard | 放弃执行事务
watch key1 key2|Watch 命令用于监视若干key ，如果在事务执行之前 key 被其他命令所改动，事务则被打断。<br>通常watch用来监测事务批量里的变量，也可以作为事务是否执行的flag 开关
unwatch | 取消 WATCH 命令对所有 key 的监视
