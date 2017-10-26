---
layout:     post
title:      "redis的安装，类库及demo "
date:       2016-5-17 00:00:00
author:     "wblearn"
header-img: "img/contact-bg.jpg"
tags:
    - redis
    - 
    - 
---


<p><span style="font-size:14px"><strong><span style="color:red">一、</span><span style="color:red"><span style="font-family:Calibri">redis</span></span><span style="color:red">的安装</span></strong></span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">a.</span>系统环境和说明</strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">Linux</span>操作系统选用<span style="font-family:Calibri">Ubuntu</span>，<span style="font-family:Calibri"> Redis</span>的版本选取目前的最新稳定版本<span style="font-family:Calibri">2.8.9</span>。客户端选用了<span style="font-family:Calibri">Redis</span>的<span style="font-family:Calibri">Java</span>版本<span style="font-family:Calibri">jedis
</span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">b.</span>安装步骤</strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">wgethttp://download.redis.io/releases/redis-2.8.9.tar.gz</span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">c. </span>
在目录下，解压按照包，生成新的目录<span style="font-family:Calibri">redis-2.8.9</span></strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">tar xvfz redis-2.8.9.tar.gz</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">d.&nbsp; </span>
进入解压之后的目录<span style="font-family:Calibri">,</span>进行编译</strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">cd &nbsp;redis-2.8.9</span></p>
<p><span style="font-family:Calibri; font-size:14px">sudo make</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px">说明：如果没有明显的错误，则表示编译成功</span></p>
<p><span style="font-size:14px">编译前请确认是否安装了<span style="font-family:Calibri">gcc</span>，若报<span style="font-family:Calibri">command not found,</span>解决：<span style="font-family:Calibri">sudo apt_get install make gcc</span></span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">e.&nbsp; </span>
安装</strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-family:Calibri; font-size:14px">sudo make install</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px">说明：一般情况下，在<span style="font-family:Calibri">Ubuntu</span>系统中，都是需要使用<span style="font-family:Calibri">sudo</span>提升权限的</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">f.&nbsp; </span>
在安装成功之后，可以运行测试，确认<span style="font-family:Calibri">Redis</span>的功能是否正常</strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-family:Calibri; font-size:14px">sudo make test</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">g. </span>
启动<span style="font-family:Calibri">Redis</span>服务</strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px">查找<span style="font-family:Calibri">Redis</span>安装的目录：<span style="font-family:Calibri"> find / -name 'redis*' ------
</span>在根目录下查找名称中含有<span style="font-family:Calibri">redis</span>的文件</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px">经过查找，发现<span style="font-family:Calibri">Redis</span>被安装到了<span style="font-family:Calibri">/usr/local/bin/</span>目录下。</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px">接下来，启动<span style="font-family:Calibri">Redis</span>服务：</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-family:Calibri; font-size:14px">/usr/local/bin/redis-server</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px"><strong><span style="font-family:Calibri">h. </span>
查看<span style="font-family:Calibri">Redis</span>进程</strong></span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-family:Calibri; font-size:14px">ps -ef | grep redis</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-family:Calibri; font-size:14px">&nbsp;</span></p>
<p><span style="font-size:14px"><strong><span style="color:red">二、</span><span style="color:red"><span style="font-family:Calibri">redis</span></span><span style="color:red">的类库</span></strong></span></p>
<p align="left" style="background:white"><strong><span style="color:black">1. keys</span></strong><span style="color:black"><br>
redis</span><span style="color:black">本质上一个key-value db</span>，所以我们首先来看看他的key.首先key也是字符串类型，但是key中不能包括边界字符<br>
由于key不是binary safe的字符串，所以像&quot;my key&quot;和&quot;mykey\n&quot;这样包含空&#26684;和换行的key是不允许的<br>
顺便说一下在redis内部并不限制使用binary字符，这是redis协议限制的。&quot;\r\n&quot;在协议&#26684;式中会作为特殊字符。<br>
redis 1.2以后的协议中部分命令已经开始使用新的协议&#26684;式了(比如MSET)。总之目前还是把包含边界字符当成非法的key吧，<br>
免得被bug纠缠。<br>
另外关于key的一个&#26684;式约定介绍下，object-type:id:field。比如user:1000:password，blog:xxidxx:title<br>
还有key的长度最好不要太长。道理很明显占内存啊，而且查找时候相对短key也更慢。不过也推荐过短的key，<br>
比如u:1000:pwd,这样的。显然没上面的user:1000:password可读性好。<br>
下面介绍下key相关的命令<br>
exits key 测试指定key是否存在，返回1表示存在，0不存在<br>
del key1 key2 ....keyN 删除给定key,返回删除key的数目，0表示给定key都不存在<br>
type key 返回给定key的value类型。返回 none 表示不存在key,string字符类型，list 链表类型 set 无序集合类型...<br>
keys pattern 返回匹配指定模式的所有key,下面给个例子<br>
randomkey 返回从当前数据库中随机选择的一个key,如果当前数据库是空的，返回空串<br>
rename oldkey newkey 原子的重命名一个key,如果newkey存在，将会被覆盖，返回1表示成功，0失败。可能是oldkey不存在或者和newkey相同<br>
renamenx oldkey newkey 同上，但是如果newkey存在返回失败<br>
dbsize 返回当前数据库的key数量<br>
expire key seconds 为key指定过期时间，单位是秒。返回1成功，0表示key已经设置过过期时间或者不存在<br>
ttl key 返回设置过过期时间的key的剩余过期秒数 -1表示key不存在或者没有设置过过期时间<br>
select db-index 通过索引选择数据库，默认连接的数据库所有是0,默认数据库数是16个。返回1表示成功，0失败<br>
move key db-index 将key从当前数据库移动到指定数据库。返回1成功。0 如果key不存在，或者已经在指定数据库中<br>
flushdb 删除当前数据库中所有key,此方法不会失败。慎用<br>
flushall 删除所有数据库中的所有key，此方法不会失败。更加慎用<br>
<strong>2. string 类型</strong><br>
string是redis最基本的类型，而且string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象<br>
。从内部实现来看其实string可以看作byte数组，最大上限是1G字节。下面是string类型的定义。</p>
<p align="left" style="background:rgb(244,244,244)"><span style="color:black">struct&nbsp;sdshdr {<br>
&nbsp;&nbsp;&nbsp;&nbsp;long&nbsp;len;<br>
&nbsp;&nbsp;&nbsp;&nbsp;long&nbsp;free;<br>
&nbsp;&nbsp;&nbsp;&nbsp;char&nbsp;buf[];<br>
};</span></p>
<p align="left" style="background:white"><span style="color:black">buf</span><span style="color:black">是个char</span>数组用于存贮实际的字符串内容。其实char和c#中的byte是等价的，都是一个字节<br>
len是buf数组的长度，free是数组中剩余可用字节数。由此可以理解为什么string类型是二进制安全的了。因为它本质上就是个byte数组。<br>
当然可以包含任何数据了。另外string类型可以被部分命令按int处理.比如incr等命令，下面详细介绍。还有redis的其他类型像list,set,sorted set ，hash<br>
它们包含的元素与都只能是string类型。<br>
如果只用string类型，redis就可以被看作加上持久化特性的memcached.当然redis对string类型的操作比memcached多很多啊。如下：<br>
set key value 设置key对应的&#20540;为string类型的value,返回1表示成功，0失败<br>
setnx key value 同上，如果key已经存在，返回0。nx 是not exist的意思<br>
get key 获取key对应的string&#20540;,如果key不存在返回nil<br>
getset key value 原子的设置key的&#20540;，并返回key的旧&#20540;。如果key不存在返回nil<br>
mget key1 key2 ... keyN 一次获取多个key的&#20540;，如果对应key不存在，则对应返回nil。下面是个实验,首先清空当前数据库，然后<br>
设置k1,k2.获取时k3对应返回nil</p>
<p align="left" style="background:rgb(244,244,244)"><span style="color:black">redis&gt;&nbsp;flushdb<br>
OK<br>
redis&gt;&nbsp;dbsize<br>
(integer) 0<br>
redis&gt;&nbsp;set k1 a<br>
OK<br>
redis&gt;&nbsp;set k2&nbsp;b<br>
OK<br>
redis&gt;&nbsp;mget k1 k2 k3<br>
1. &quot;a&quot;<br>
2. &quot;b&quot;<br>
3. (nil)</span></p>
<p align="left" style="background:white"><span style="color:black">mset key1 value1 ...keyN valueN
</span><span style="color:black">一次设置多个key</span>的&#20540;，成功返回1表示所有的&#20540;都设置了，失败返回0表示没有任何&#20540;被设置<br>
msetnx key1 value1 ... keyN valueN 同上，但是不会覆盖已经存在的key<br>
incr key 对key的&#20540;做加加操作,并返回新的&#20540;。注意incr一个不是int的value会返回错误，incr一个不存在的key，则设置key为1<br>
decr key 同上，但是做的是减减操作，decr一个不存在key，则设置key为-1<br>
incrby key integer 同incr，加指定&#20540; ，key不存在时候会设置key，并认为原来的value是 0<br>
decrby key integer 同decr，减指定&#20540;。decrby完全是为了可读性，我们完全可以通过incrby一个负&#20540;来实现同样效果，反之一样。</p>
<p align="left" style="background:white"><span style="color:black">substr </span>
<span style="color:black">返回截取过的key</span>的字符串&#20540;,注意并不修改key的&#20540;。下标是从0开始的.(redis在2.0版本以后不包括2.0,使用的方法是getrange参数相同。)<br>
append key value 给指定key的字符串&#20540;追加value,返回新字符串&#20540;的长度。下面给个例子</p>
<p align="left" style="background:rgb(244,244,244)"><span style="color:black">redis&gt;&nbsp;set k hello<br>
OK<br>
redis&gt;&nbsp;append k ,world<br>
(integer) 11<br>
redis&gt;&nbsp;get k<br>
&quot;hello,world&quot;<br>
substr key start end<br>
redis&gt;&nbsp;substr k 0 8<br>
&quot;hello,wor&quot;<br>
redis&gt;&nbsp;get k<br>
&quot;hello,world&quot;</span></p>
<p align="left" style="background:white"><strong><span style="color:black">3. list</span></strong><span style="color:black"><br>
<br>
redis</span><span style="color:black">的list</span>类型其实就是一个每个子元素都是string类型的双向链表。所以[lr]push和[lr]pop命令的算法时间复杂度都是O(1)<br>
另外list会记录链表的长度。所以llen操作也是O(1).链表的最大长度是(2的32次方-1)。我们可以通过push,pop操作从链表的头部<br>
或者尾部添加删除元素。这使得list既可以用作栈，也可以用作队列。有意思的是list的pop操作还有阻塞版本的。当我们[lr]pop一个<br>
list对象是，如果list是空，或者不存在，会立即返回nil。但是阻塞版本的b[lr]pop可以则可以阻塞，当然可以加超时时间，超时后也会返回nil<br>
。为什么要阻塞版本的pop呢，主要是为了避免轮询。举个简单的例子如果我们用list来实现一个工作队列。执行任务的thread可以调用阻塞版本的pop去<br>
获取任务这样就可以避免轮询去检查是否有任务存在。当任务来时候工作线程可以立即返回，也可以避免轮询带来的延迟。ok下面介绍list相关命令<br>
lpush key string 在key对应list的头部添加字符串元素，返回1表示成功，0表示key存在且不是list类型<br>
rpush key string 同上，在尾部添加<br>
llen key 返回key对应list的长度，key不存在返回0,如果key对应类型不是list返回错误<br>
lrange key start end 返回指定区间内的元素，下标从0开始，负&#20540;表示从后面计算，-1表示倒数第一个元素 ，key不存在返回空列表<br>
ltrim key start end 截取list，保留指定区间内元素，成功返回1，key不存在返回错误<br>
lset key index value 设置list中指定下标的元素&#20540;，成功返回1，key或者下标不存在返回错误<br>
lrem key count value 从key对应list中删除count个和value相同的元素。count为0时候删除全部<br>
lpop key 从list的头部删除元素，并返回删除元素。如果key对应list不存在或者是空返回nil，如果key对应&#20540;不是list返回错误<br>
rpop 同上，但是从尾部删除<br>
blpop key1...keyN timeout 从左到右扫描返回对第一个非空list进行lpop操作并返回，比如blpop list1 list2 list3 0 ,如果list不存在<br>
list2,list3都是非空则对list2做lpop并返回从list2中删除的元素。如果所有的list都是空或不存在，则会阻塞timeout秒，timeout为0表示一直阻塞。<br>
当阻塞时，如果有client对key1...keyN中的任意key进行push操作，则第一在这个key上被阻塞的client会立即返回。如果超时发生，则返回nil。有点像unix的select或者poll<br>
brpop 同blpop，一个是从头部删除一个是从尾部删除<br>
rpoplpush srckey destkey 从srckey对应list的尾部移除元素并添加到destkey对应list的头部,最后返回被移除的元素&#20540;，整个操作是原子的.如果srckey是空<br>
或者不存在返回nil<br>
<strong>4. set</strong><br>
redis的set是string类型的无序集合。set元素最大可以包含(2的32次方-1)个元素。set的是通过hashtable实现的，所以添加，删除，查找的复杂度都是O(1)。hashtable会随着添加或者删除自动的调整大小。需要注意的是调整hash table大小时候需要同步（获取写锁）会阻塞其他读写操作。可能不久后就会改用跳表（skip list）来实现<br>
跳表已经在sorted set中使用了。关于set集合类型除了基本的添加删除操作，其他有用的操作还包含集合的取并集(union)，交集(intersection)，<br>
差集(difference)。通过这些操作可以很容易的实现sns中的好友推荐和blog的tag功能。下面详细介绍set相关命令<br>
sadd key member 添加一个string元素到,key对应的set集合中，成功返回1,如果元素以及在集合中返回0,key对应的set不存在返回错误<br>
srem key member 从key对应set中移除给定元素，成功返回1，如果member在集合中不存在或者key不存在返回0，如果key对应的不是set类型的&#20540;返回错误<br>
spop key 删除并返回key对应set中随机的一个元素,如果set是空或者key不存在返回nil<br>
srandmember key 同spop，随机取set中的一个元素，但是不删除元素<br>
smove srckey dstkey member 从srckey对应set中移除member并添加到dstkey对应set中，整个操作是原子的。成功返回1,如果member在srckey中不存在返回0，如果<br>
key不是set类型返回错误<br>
scard key 返回set的元素个数，如果set是空或者key不存在返回0<br>
sismember key member 判断member是否在set中，存在返回1，0表示不存在或者key不存在<br>
sinter key1 key2...keyN 返回所有给定key的交集<br>
sinterstore dstkey key1...keyN 同sinter，但是会同时将交集存到dstkey下<br>
sunion key1 key2...keyN 返回所有给定key的并集<br>
sunionstore dstkey key1...keyN 同sunion，并同时保存并集到dstkey下<br>
sdiff key1 key2...keyN 返回所有给定key的差集<br>
sdiffstore dstkey key1...keyN 同sdiff，并同时保存差集到dstkey下<br>
smembers key 返回key对应set的所有元素，结果是无序的<br>
<strong>5 sorted set</strong><br>
和set一样sorted set也是string类型元素的集合，不同的是每个元素都会关联一个double类型的score。sorted set的实现是skiplist和hash table的混合体<br>
当元素被添加到集合中时，一个元素到score的映射被添加到hashtable中，所以给定一个元素获取score的开销是O(1),另一个score到元素的映射被添加到skip list<br>
并按照score排序，所以就可以有序的获取集合中的元素。添加，删除操作开销都是O(log(N))和skip list的开销一致,redis的skip list实现用的是双向链表,这样就<br>
可以逆序从尾部取元素。sorted set最经常的使用方式应该是作为索引来使用.我们可以把要排序的字段作为score存储，对象的id当元素存储。下面是sorted set相关命令<br>
zadd key score member 添加元素到集合，元素在集合中存在则更新对应score<br>
zrem key member 删除指定元素，1表示成功，如果元素不存在返回0<br>
zincrby key incr member 增加对应member的score&#20540;，然后移动元素并保持skip list保持有序。返回更新后的score&#20540;<br>
zrank key member 返回指定元素在集合中的排名（下标）,集合中元素是按score从小到大排序的<br>
zrevrank key member 同上,但是集合中元素是按score从大到小排序<br>
zrange key start end 类&#20284;lrange操作从集合中去指定区间的元素。返回的是有序结果<br>
zrevrange key start end 同上，返回结果是按score逆序的<br>
zrangebyscore key min max 返回集合中score在给定区间的元素<br>
zcount key min max 返回集合中score在给定区间的数量<br>
zcard key 返回集合中元素个数<br>
zscore key element 返回给定元素对应的score<br>
zremrangebyrank key min max 删除集合中排名在给定区间的元素<br>
zremrangebyscore key min max 删除集合中score在给定区间的元素<br>
<strong>6. hash</strong>&nbsp;<br>
redis hash是一个string类型的field和value的映射表.它的添加，删除操作都是O(1)（平均）.hash特别适合用于存储对象。相较于将对象的每个字段存成<br>
单个string类型。将一个对象存储在hash类型中会占用更少的内存，并且可以更方便的存取整个对象。省内存的原因是新建一个hash对象时开始是用zipmap（又称为small hash）来存储的。这个zipmap其实并不是hash table，但是zipmap相比正常的hash实现可以节省不少hash本身需要的一些元数据存储开销。尽管zipmap的添加，删除，查找都是O(n)，但是由于一般对象的field数量都不太多。所以使用zipmap也是很快的,也就是说添加删除平均还是O(1)。如果field或者value的大小超出一定限制后，redis会在内部自动将zipmap替换成正常的hash实现.
 这个限制可以在配置文件中指定</p>
<p align="left" style="background:rgb(244,244,244)"><span style="color:black">redis&gt;&nbsp;set test dsf<br>
OK<br>
redis&gt;&nbsp;set tast dsaf<br>
OK<br>
redis&gt;&nbsp;set tist adff<br>
OK<br>
redis&gt;&nbsp;keys t*<br>
1. &quot;tist&quot;<br>
2. &quot;tast&quot;<br>
3. &quot;test&quot;<br>
redis&gt;&nbsp;keys t[ia]st<br>
1. &quot;tist&quot;<br>
2. &quot;tast&quot;<br>
redis&gt;&nbsp;keys t?st<br>
1. &quot;tist&quot;<br>
2. &quot;tast&quot;<br>
3. &quot;test&quot;</span></p>
<p align="left" style="background:white"><span style="color:black">hash-max-zipmap-entries64 #</span><span style="color:black">配置字段最多64</span>个<br>
hash-max-zipmap-value 512 #配置value最大为512字节<br>
下面介绍hash相关命令<br>
hset key field value 设置hash field为指定&#20540;，如果key不存在，则先创建<br>
hget key field 获取指定的hash field<br>
hmget key filed1....fieldN 获取全部指定的hash filed<br>
hmset key filed1 value1 ... filedN valueN 同时设置hash的多个field<br>
hincrby key field integer 将指定的hash filed 加上给定&#20540;<br>
hexists key field 测试指定field是否存在<br>
hdel key field 删除指定的hash field<br>
hlen key 返回指定hash的field数量<br>
hkeys key 返回hash的所有field<br>
hvals key 返回hash的所有value<br>
hgetall 返回hash的所有filed和value</p>
<p><strong><span style="font-size:14px"><span style="color:red">三、</span><span style="color:red"><span style="font-family:Calibri">demo</span></span></span></strong></p>
<p><span style="font-size:14px"><strong><span style="color:red">准备工作：引入</span><span style="color:red"><span style="font-family:Calibri">redis</span></span><span style="color:red">包</span></strong></span></p>
<p><strong><img src="http://img.blog.csdn.net/20160517220203714?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></strong></p>
<p><strong><span style="color:red"><span style="font-family:Calibri; font-size:14px">&nbsp;</span></span></strong></p>
<p><strong><span style="font-family:Calibri; font-size:14px">String</span></strong></p>
<p><strong></strong></p>
<p><strong><span style="font-family:Calibri; font-size:14px">&nbsp;<img src="http://img.blog.csdn.net/20160517220219645?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></span></strong></p>
<p><strong><span style="font-family:Calibri; font-size:14px">&nbsp;</span></strong></p>
<p><strong><span style="font-family:Calibri; font-size:14px">list</span></strong></p>
<p><strong><img src="http://img.blog.csdn.net/20160517220236817?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></strong></p>
<p><strong><span style="font-family:Calibri; font-size:14px"></span></strong>&nbsp;</p>
<p><strong><span style="font-family:Calibri; font-size:14px">set</span></strong></p>
<p><strong><img src="http://img.blog.csdn.net/20160517220248292?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></strong></p>
<p><strong><span style="font-family:Calibri; font-size:14px">map</span></strong></p>
<p><strong></strong>&nbsp;</p>
<p><img src="http://img.blog.csdn.net/20160517220302739?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
   



