# 常用的ORM框架
  ORM就是对象关系匹配，是为了解决面向对象与关系数据库存在的互不匹配的问题。简单来说，就是把关系数据库中的数据转换成面向对象程序中的对象。

常用的ORM框架有Hibernate和MyBatis，也就是ssh组合和ssm组合中的h与m。

**它们的特点和区别如下：**
  Hibernate对数据库结构提供了完整的封装，实现了POJO对象与数据库表之间的映射，能够自动生成并执行SQL语句。只要定义了POJO 到数据库表的映射关系，就可以通过Hibernate提供的方法完成数据库操作。Hibernate符合JPA规范，就是Java持久层API。
  MyBatis通过映射配置文件，将SQL所需的参数和返回的结果字段映射到指定对象，MyBatis不会自动生成SQL，需要自己定义SQL语句，不过更方便对SQL语句进行优化。

**总结起来：**
1. Hibernate配置要比mybatis复杂的多，学习成本也比MyBatis高。MyBatis，简单、高效、灵活，但是需要自己维护SQL；
2. Hibernate功能强大、全自动、适配不同数据库，但是非常复杂，灵活性稍差。

小孩子才做选择，我全都要，你单独用RDB你会丢失很多数据，你单独用AOF，你数据恢复没RDB来的快，真出什么时候第一时间用RDB恢复，然后AOF做数据补全，真香！冷备热备一起上，才是互联网时代一个高健壮性系统的王道。


git remote add origin https://github.com/kxzhot/gitNote.git
git push -u origin master