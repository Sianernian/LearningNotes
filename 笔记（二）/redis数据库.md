redis 类型 string  list hash   set   zset



set   key  value  设置key value    设置相同key会覆盖

get key     根据key  查看value

del   key    删除

keys *  查看所有key

expire key  时间      为给定的key设置过期时间

ttl  key  查看还有多少时间    -1 永不过期    -2已过期

exists key  查看是否存在   1存在   0不存在

move  key   数据库下标    将key移植到另一个数据库中

​        默认7个数据库   用 select  0-6 选择数据库



type key 查看 查看类型

dbsize   数据库大小

save 即刻保存数据



**AOF文件可与 RDB文件共存    且先执行AOF文件**

**AOF文件修复    redis-check-aof  --fix  XXXX。**



### 事务

命令：  discard     取消事务               exec   执行所有命令           multi   标记一个事务的开始                               unwatch   取消 watch命令对所有key的监视                   watch key[key ...]  监视一个（多个）key

**主从复制/读写分离（master/slave）**

1、配从（库）不配主（库）

2、slaveof  主库ip  主库端口

   停止为slave    slave no one

​     一主多仆      薪火相传   反客为主

172.17.0.3   6381

172.17.0.4   6380 

172.17.0.2   6379   查看信息  info replication





### string

#####       append      key   value   添加

#####      strlen  key   获取长度

#####       incr  key    数值    增加一	            incrby  key    数值       增加x 

#####    decr  key    数值    减一                    decrby   key    数值    减x

##### getrange    key   x  y    得到下标为x到y的值         setrange    key   x  y    范围设值

##### setex key  时间  value    设置留存时间          setnx   key  value    不存在设值  已存在则不设值

##### mget   key1   key2 ..   获取多个           msetnx key value key2 value2 ... 设置多个不存在的   



### List  （r 开头时顺序  其他时逆反）

#####     lpush kye vlaue value ... 从左放置一个list     rpush kye vlaue value ... 从右 放置一个list 

#####     larnge  key   index  index（0 -1   查看全部内容）

#####     lpop  key      把最上面的拿出    （栈顶移出）   rpop  key      把最上面的拿出    （栈尾移出）

##### lindex  key   index    下表索引	

#####  lrem  key    n   value  删除n个重复的值 

#####    ltrim key   开始index    结束index    截取指定范围的值后赋值给key

##### rpoplpush  源列表   目标列表   

lrange list1 0 -1       127.0.0.1:6379> lrange list2 0 -1

1) "3"                       1) "2"
2) "2"                   2) "3"    3) "4"     4) "5"

RPOPLPUSH list1 list2         "2"

lrange list2 0 -1
1) "2"  2) "2"    3) "3"    4) "4"  4) "4"  5) "5"



##### lset    key   index  value  : 把index的值 改变为 设定的value

##### linsert  key before/after value（list中的数据） value（要设定的值）: 把value（list中的数据）改为要设定的值



### Set(没有重复  无序)

   sadd  key  value1  value2 ....   设置set 值

   smembers  key    查看set

   sismember	 key  判断是否时set   是返回1   不是返回0

  scard  key  查看 set（集合）有多少个元素

 srem  key value   删除

  srandmember  key   x    某个随机数（随机几个数）

spop   key  随机出栈

smove  key1  key2  value   在key1里某个值   将key1中的某个值赋值给key2

sdiff  key1  key2  展示key1中没有而key2中有的数据   差集

sunion  key1  key2   并集 



### Hash（hashset）(类似map)   kv模式不变  但v是一个键值对

hset  key   value[key  value]    设置hash

hget  key  value[key]   得到值

hmset  key  value[key1 value1  key2 value2  ....] 

hmget key  value[key1  key2 ....]     /   hgetall  key

hdel  key 删除 hash    hlen key  查看长度      hexists key  valu[key]  是否存在      hkeys key  查看所有hashmap中所有key                hvals  key  查看所有value

hincrby  key value[key  数值整数X]   增加x   hincrbyfloatey value[key  数值小数X]   增加x

  hsetnx key value[ key1 value2] ... 设置多个不存在的  

### Zset（有序集合 sirted set

zadd  key  [key value  , key1 value1,]   

zrange key  0 -1  获取全部  值

zrange key 0 -1 withscores  获取全部key和value 

zrangescore  key  x--x（范围）    zrangescore  key  x--（x（范围 不包含后一个x）

  zrangescore  key  x--x（范围 ） limit  index  x （大小） 在范围内再截取

​      zrem key  value[key]  删除

zcard  key 查看有多少个

