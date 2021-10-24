# RedisNote
redis的基本操作，笔记 2021.10.23

## 1.redis的启动
* 服务端启动
     * redis-service
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
   
``·
设置用户 123abc
#添加一个键user 值为123abc
set user 1111
#修改键user     改值为123abc
```

* setex 键 过期时间
  * 过期时间单位 秒
  
 ```
 setex user 3 abc
 # 添加一个键user 值为abc 过期时间3秒
 ```
 
 
 
