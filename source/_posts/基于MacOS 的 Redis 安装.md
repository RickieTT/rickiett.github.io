---
title: 基于MacOS 的 Redis 安装
date: 2022-11-26 23:51:12
---

1. 下载Redis

https://redis.io/

 

2.安装

 

# 解压
tar -zxvf redis-6.2.7.tar
# 拷贝的local目录下
sudo cp -rf redis-6.2.7 /usr/local/
# 进入相应目录下
cd /usr/local/redis-*
# 编译
sudo make test
# 安装
sudo make install
# 建立相应目录
sudo mkdir bin etc db
# 拷贝启动文件
sudo cp src/mkreleasehdr.sh src/redis-benchmark src/redis-check-rdb src/redis-cli src/redis-server bin/
 

3. 配置

# 拷贝配置文件（在redis目录下）
sudo cp redis.conf etc/
sudo vi etc/redis.conf

####内容参照如下：(选择性修改就好了，不用全都照着改)

#修改为守护模式（后台启动）
daemonize yes
#设置进程锁文件
pidfile /usr/local/redis-4.0.10/redis.pid
#端口
port 6379
#客户端超时时间
timeout 300
#日志级别
loglevel debug
#日志文件位置
logfile /usr/local/redis-4.0.10/log-redis.log
#设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
databases 16
##指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
#save <seconds> <changes>
#Redis默认配置文件中提供了三个条件：
save 900 1
save 300 10
save 60 10000
#指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，
#可以关闭该#选项，但会导致数据库文件变的巨大
rdbcompression yes
#指定本地数据库文件名
dbfilename dump.rdb
#指定本地数据库路径
dir /usr/local/redis-4.0.10/db/
#指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能
#会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有
#的数据会在一段时间内只存在于内存中
appendonly no
#指定更新日志条件，共有3个可选值：
#no：表示等操作系统进行数据缓存同步到磁盘（快）
#always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
#everysec：表示每秒同步一次（折衷，默认值）
appendfsync everysec
# 设置连接密码
requirepass yourpassword
 

4. 启动服务

sudo /usr/local/redis-6.2.7/src/redis-server /usr/local/redis-6.2.7/etc/redis.conf
如果觉得命令太长了，建议使用别名（alias）
结束redis服务：（当然可以直接结束进程）
在客户端执行 SHUTDOWN 或 SHUTDOWN NOSAVE 可关闭 redis 服务
 

5. 客户端

# 进入相应目录
cd /usr/local/redis-4.0.10/src/
# 启动客户端连接
sudo ./redis-cli 
# 如果有密码
sudo ./redis-cli auth yourpassword
 

 

根据https://blog.csdn.net/shuux666/article/details/124295096 进行安装
