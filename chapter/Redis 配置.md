# Redis 配置

## 一 工具类

`com.neusoft.tobaccocloud.framework.base.utils.RedisUtil`

## 二 配置文件

````properties
# Redis 集群配置
spring.redis.cluster.nodes=192.168.159.129:7001,192.168.159.129:7002,192.168.159.129:7003,192.168.159.129:7004,192.168.159.129:7005,192.168.159.129:7006

# Redis 单节点——主机地址  
spring.redis.host=192.168.0.24  
# Redis 单节点——端口  
spring.redis.port=6379

# Redis服务器连接密码（默认为空）  
spring.redis.password=  
# Redis数据库索引(默认为0) 
spring.redis.database=0
# 连接池最大连接数（使用负值表示没有限制） 
spring.redis.pool.max-active=200
# 连接池最大阻塞等待时间（使用负值表示没有限制） 
spring.redis.pool.max-wait=-1
# 连接池中的最大空闲连接 
spring.redis.pool.max-idle=10
# 连接池中的最小空闲连接 
spring.redis.pool.min-idle=0
# 连接超时时间（毫秒） 
spring.redis.timeout=60000
````

