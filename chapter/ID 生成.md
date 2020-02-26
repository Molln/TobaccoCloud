# ID 生成

## 一 概述

分布式系统中，有一些需要使用全局唯一 ID 的场景，这种时候为了防止 ID 冲突可以使用32位的 UUID ，但是 UUID 有一些缺点，首先他相对比较长，另外 UUID 一般是无序的。

有些时候我们希望能使用一种简单一些的 ID ，并且希望 ID 能够按照时间有序生成。

而 Twitter 的 Snowflake 解决了这种需求，最初 Twitter 把存储系统从 MySQL 迁移到 Cassandra，因为 Cassandra 没有顺序 ID 生成机制，所以开发了这样一套全局唯一ID生成服务。

## 二 Snowflake

<https://github.com/twitter/snowflake>

Snowflake 是 Twitter 开源的分布式 ID 生成算法，结果是一个 long 型的ID。

这种方案大致来说是一种以划分命名空间（ UUID 也算，由于比较常见，所以单独分析）来生成 ID 的一种算法，这种方案把 64-bit 分别划分成多段，分开来标示机器、时间等。

其核心思想是：使用 41-bit 作为毫秒数，10-bit 作为机器的ID（5个bit 是数据中心，5个bit 的机器ID），12-bit 作为毫秒内的流水号，最后还有一个符号位，永远是0。

比如在 Snowflake 中的 64-bit 分别表示如下图（图片来自网络）所示：

![Snowflake](img/Snowflake.png)

* 1位标识

* 由于 long 基本类型在Java中是带符号的，最高位是符号位，正数是0，负数是1，所以id一般是正数，最高位是0

* 41位时间截(毫秒级)

* 注意，41位时间截不是存储当前时间的时间截，而是存储时间截的差值（当前时间截 - 开始时间截)

  得到的值），这里的的开始时间截，一般是我们的id生成器开始使用的时间，由我们程序来指定的（如下下面程序IdWorker类的startTime属性）。41位的时间截，可以使用69年，年T = (1L << 41) /

  (1000L * 60 * 60 * 24 * 365) = 69

* 10位的数据机器位
* 可以部署在1024个节点，包括5位 datacenterId 和5位 workerId
* 12位序列
* 毫秒内的计数，12位的计数顺序号支持每个节点每毫秒(同一机器，同一时间截)产生4096个ID序号

 加起来刚好64位，为一个Long型。

### 优点

SnowFlake 的优点是，整体上按照时间自增排序，并且整个分布式系统内不会产生 ID 碰撞(由数据中心 ID 和 机器ID 作区分)，并且效率较高，经测试，SnowFlake 每秒能够产生26万 ID 左右。

- 毫秒数在高位，自增序列在低位，整个 ID 都是趋势递增的。
- 不依赖数据库等第三方系统，以服务的方式部署，稳定性更高，生成ID的性能也是非常高的。
- 可以根据自身业务特性分配 bit 位，非常灵活。

### 缺点

- **强依赖机器时钟，如果机器上时钟回拨**，会导致发号重复或者服务会处于不可用状态。
- 针对此，美团做出了改进：<https://github.com/Meituan-Dianping/Leaf>

## 三 实现类

`com.neusoft.tobaccocloud.framework.base.utils.IdWorker`

## 四 使用

框架中在执行保存方法时，使用此类进行ID 生成，如果在其他地方需要生成全局唯一 ID，可使用如下代码

````java
IdWorker idWorker=IdWorker.getInstance();
Long id=idWorker.nextId();
````