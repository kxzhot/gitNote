RDB：RDB 持久化机制，是对 Redis 中的数据执行周期性的持久化。

AOF：AOF 机制对每条写入命令作为日志，以 append-only 的模式写入一个日志文件中，因为这个模式是只追加的方式，所以没有任何磁盘寻址的开销，所以很快，有点像Mysql中的binlog。

两种方式都可以把Redis内存中的数据持久化到磁盘上，然后再将这些数据备份到别的地方去，RDB更适合做冷备，AOF更适合做热备，比如我杭州的某电商公司有这两个数据，我备份一份到我杭州的节点，再备份一个到上海的，就算发生无法避免的自然灾害，也不会两个地方都一起挂吧，这灾备也就是异地容灾，地球毁灭他没办法。

tip：两种机制全部开启的时候，Redis在重启的时候会默认使用AOF去重新构建数据，因为AOF的数据是比RDB更完整的。

我先说RDB吧

优点：
他会生成多个数据文件，每个数据文件分别都代表了某一时刻Redis里面的数据，这种方式，有没有觉得很适合做冷备，完整的数据运维设置定时任务，定时同步到远端的服务器，比如阿里的云服务，这样一旦线上挂了，你想恢复多少分钟之前的数据，就去远端拷贝一份之前的数据就好了。

RDB对Redis的性能影响非常小，是因为在同步数据的时候他只是fork了一个子进程去做持久化的，而且他在数据恢复的时候速度比AOF来的快。

缺点：
RDB都是快照文件，都是默认五分钟甚至更久的时间才会生成一次，这意味着你这次同步到下次同步这中间五分钟的数据都很可能全部丢失掉。AOF则最多丢一秒的数据，数据完整性上高下立判。

还有就是RDB在生成数据快照的时候，如果文件很大，客户端可能会暂停几毫秒甚至几秒，你公司在做秒杀的时候他刚好在这个时候fork了一个子进程去生成一个大快照，哦豁，出大问题。

我们再来说说AOF

优点：
上面提到了，RDB五分钟一次生成快照，但是AOF是一秒一次去通过一个后台的线程fsync操作，那最多丢这一秒的数据。

AOF在对日志文件进行操作的时候是以append-only的方式去写的，他只是追加的方式写数据，自然就少了很多磁盘寻址的开销了，写入性能惊人，文件也不容易破损。

AOF的日志是通过一个叫非常可读的方式记录的，这样的特性就适合做灾难性数据误删除的紧急恢复了，比如公司的实习生通过flushall清空了所有的数据，只要这个时候后台重写还没发生，你马上拷贝一份AOF日志文件，把最后一条flushall命令删了就完事了。

tip：我说的命令你们别真去线上系统操作啊，想试去自己买的服务器上装个Redis试，别到时候来说，敖丙真是个渣男，害我把服务器搞崩了，Redis官网上的命令都去看看，不要乱试！！！

缺点：
一样的数据，AOF文件比RDB还要大。

AOF开启后，Redis支持写的QPS会比RDB支持写的要低，他不是每秒都要去异步刷新一次日志嘛fsync，当然即使这样性能还是很高，我记得ElasticSearch也是这样的，异步刷新缓存区的数据去持久化，为啥这么做呢，不直接来一条怼一条呢，那我会告诉你这样性能可能低到没办法用的，大家可以思考下为啥哟。

那两者怎么选择？
AOF开启后，Redis支持写的QPS会比RDB支持写的要低，他不是每秒都要去异步刷新一次日志嘛fsync，当然即使这样性能还是很高，我记得ElasticSearch也是这样的，异步刷新缓存区的数据去持久化，为啥这么做呢，不直接来一条怼一条呢，那我会告诉你这样性能可能低到没办法用的，大家可以思考下为啥哟。
小孩子才做选择，我全都要，你单独用RDB你会丢失很多数据，你单独用AOF，你数据恢复没RDB来的快，真出什么时候第一时间用RDB恢复，然后AOF做数据补全，真香！冷备热备一起上，才是互联网时代一个高健壮性系统的王道。
