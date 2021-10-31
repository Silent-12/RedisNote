# RedisNote
redis的基本操作，笔记 2021.10.23

学习环境:VM - ubuntu

## 1.redis的启动

* 服务端启动
     * redis-server
* 客户端启动
     * redis-cli        (英文)
     * redis-cli --raw （中文）
* 测试启动成功
     * ping > pong



## 2.redis基本操作

> 数据库没有名称，默认16个，通过0-15来标识 
* 选择数据库
     * select 0-15



## 3.redis数据类型

* redis保存的数据都是按照键值对来保存的
* 键(key) 值(value)
* 键的数据类型都是字符串
* 值的数据类型有五种
   *  字符串(String)
   *  哈希(hash)
   *  列表(list)
   *  无序集合(set)
   *  有序集合(zset)
   



## 4.字符串操作

* set 键 值
  * 如果键不存在，就是添加
  * 键存在，就是修改   
```cmd
set user 123abc
#添加一个键user 值为123abc
set user 1111
#修改键user     改值为1111
```

* setex 键 过期时间
  * 过期时间单位 秒  
```cmd
 setex user 3 abc
 # 添加一个键user 值为abc 过期时间3秒
```

* 添加多个键值对
  * mset 键1值1 键2值2 键3值3 
```cmd
mest user1 aaa user2 bbb user3 ccc
# 同时添加3个键值对
```

* 追加值
  * append 键 值
```cmd
append user aabbcc
# 向user键，后追加值aabbcc
```

### 1.1 获取
* get获取单个值
```cmd
get user
# 获取user对应的值
```

* mget获取多个值
```cmd
mget user1 user2 user3
# 一次获取多个值
```

### 1.2 删除
* 删除键及对应的值
```cmd
del user1 user2 user3
# 删除键值user1 user2 user3
```



## 5. 键命令

### 1.1 查找键

* keys 键名

* 可以支持 * 通配符

  ```cmd
  keys user
  # 查找键user是否存在
  keys * 
  # 显示所有键
  ```

### 1.2 查找键是否存在

* exists 键名
  * 如果键存在，返回1
  * 不存在，返回0

```cmd
exists user
# 判断user键是否存在
```

### 1.3 查看键对应的值类型

```cmd
type user
# 查看user键，对应的值类型
```

### 1.4 设已有键的过期时间

* expire 键

```cmd
expire user 100
# 设置user的过期时间为100秒
```

### 1.5 查看键的有效时间

* 以秒为单位
  * 返回大于0，代表有效时间
  * 返回 -1 永远有效
  * 返回 -2  键不存在

```cmd
ttl user
#查看键user的过期时间
```



## 6. 哈希 hash

### 1.1 添加单个值

* hset 键 字段名 值

```cmd
hset user nama abc123
# 将键user的 字段name的值设置为abc123
```

### 1.2 添加多个值

* hmset  值 字段 

* hmset  键 字段 值  字段1 值2

```cmd
hmset user a0 1 a1 2
# 添加user的键 字段分别是a0 & a1
```

### 1.3 获取

* 获取指定键所有的字段

```cmd
hkeys user
# 获取user键下的所有字段（不包含值）
```

* 获取一个字段

```cmd
hget user a0
# 获取user字段下 a0 的值
```

* 获取多个字段的值

```cmd
hmget user a0 a1
# 获取user键下 a0 a1字段的值
```

* 获取所有字段的值
  * hvals 键

 ```cmd
 hvals user
 # 获取user所有字段的值
 ```

* 一次获取所有字段名和值
* hgetall 键

```cmd
hgetall user
# 获取user键下的所有字段名和值
```

### 1.4 删除

* 删除hash中的指定字段
  * 字段对应的值会一起删除

```cmd
hdel user a
# 删除user键下的a字段
```

* 删除键

```cmd
del user
# 直接删除整个键
```



## 7. 列表 list

### 1.1 添加值

* 从左侧插入值

```cmd
lpush user value1 value2
# 从键user的列表左侧加入值
```

* 从右侧插入值

```cmd
rpush user value1 value2
# 从列表的右侧插入值
```

* 在指定位置插入值
* 前插入新值  before

```cmd
linsert user1 before value1 a1
# 在键user的列表中 的valuel值前 插入a1
```

* 后插入新值  after 

```cmd
linsert user1 after value2 a2
# 在键user的列表中 的value2值后 插入a2
```

### 1.2 获取值

* 返回列表里指定范围内的值
  * 索引从左侧开始，第一个值得索引为 0；
  * 索引可以是负数，表示从尾部开始计数，如 -1表示最后一个值
  * start，stop为要获取值的索引

```cmd
lrange user 0 -1
# 获取键user的列表所有值
```

### 1.3 修改值

* lset 键 索引 新值

```cmd
lset user 1 user1
# 修改键user 索引为1的值 改为user1
```

### 1.4 删除

* 将列表中前count次出现的值移除

* lrem 键 count 值

  * count > 0   从头往尾删

  * count < 0   从尾往头删除

  * count = 0   删除所有值

```cmd
lrem user -1 a
# 从后往前，从右往左，删除一个a
lrem user 0 a
# 删除所有的a
```



## 8. 无序集合 set

> 1.无序集合中的值，类型为String
>
> 2.集合里不允许有重复的值
>
> 3.集合里的值，只能添加与删除，不能修改



### 1.1 添加值

* sadd 键 值1 值2 值3
  * 集合添加值

```cmd
sadd user a1 a2 a3
# 添加三个值
```



### 1.2 获取

* 返回所有值

```cmd
smembers user
# 查询user键的所有值
```



### 1.3 删除

* srem 键 值

```cmd
srem user a1
# 删除键user下的a1
```



## 9. 有序集合 zset

> 1.有序集合值得类型为String
>
> 2.集合里不允许有重复的值
>
> 3.每个值都会关联一个分数（score），分数可以为负数，通过分数将值从小到大排列
>
> 4.有序集合里的值只能添加与删除，不能修改



![image-20211031171231340](C:\Users\hlh60\AppData\Roaming\Typora\typora-user-images\image-20211031171231340.png)



### 1.1 添加值

* zadd 键 分数score  值

```cmd
zadd user 1 aa 2 dd
# 添加一个user键，分数为1，值是aa
```

### 1.2 获取

* 返回指定范围内的值

  * start，stop为值得下标索引

  * 第一个值的索引为0

  * 索引可以是负数，表示从尾部计数， -1表示最后一个值

  * withscores：同时获取值对应分数（score）

    

* **zrange 键 start stop**

```cmd
zrange user 0 -1
# 查询user下的所有值
```



* **zrange 键 start stop winthscores【显示分数】**

```cmd
zrange user 0 -1 withscores
# 查询值的时候，显示分数
```



* **通过score获取值**
  * zrangebyscore 键 min max 

```cmd
zrangebyscore user 2 6
# 获取键user从2-6之间的值
```



* **通过值得到score**
  * zscore 键 值

```cmd
zscore user a
# 查询键user下a值的score
```

### 1.3 删除值

* **删除指定值**
* zrem 键 值1 值2

```cmd
zrem user aa bb
# 删除键user下的aa，bb值
```



* **删除(score)在指定范围之间的值**
* zremrangebyscore 键 min   max

```cmd
zremrangebyscore user 4 8
# 删除键user下，4-8的值
```

