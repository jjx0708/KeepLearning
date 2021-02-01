## What‘s MyBatis ？

MyBatis 本是 Apache 的一个开源项目 iBatis ,  2010年这个项目由 Apache Software Foundation 迁移到了Google Code，随着开发团队转投 Google Code旗下， iBatis3.x正式更名为 MyBatis ，代码于2013年11月迁移到 Github 是一个基于 Java 的持久层框架。



## Why MyBatis ？

在传统意义上的 JDBC 代码中，我们除了需要自己写出 sql 语句之外，还要获取 Connection 对象，操作 Statement ，然后再解析 ResulteSet 结果集；

![image-20210112223312824](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210112223312824.png)

除此之外，一个更致命的就是 sql 语句夹在 Java 代码里，耦合度高！非常不易于维护，因为在实际开发中会频繁的改变一些需求甚至修改 sql 语句；

MyBatis 提供了一种简洁的操作，开发者只需要提供相应的 sql 语句，其他的配置都交给框架进行处理，使得开发者能够更加专注于 sql 代码本身，写出更简洁高效的 sql 代码 ；

为什么我们要学 MyBatis ，不是已经有了 Hibernate 吗 ？

这里我们简单分析一下这两者的差异：

- Hibernate 是一个 **全自动的 ORM ** 框架，旨在消除 sql ，在整个数据操作过程中，开发人员只需要写好相应的 JavaBean 以及配置好对应的数据源，这整个执行过程都是一个整体封装好的，甚至于 sql 语句都不需要开发人员编写，但是定制化的查询需要额外学习 HQL 语言；

  ![image-20210112222324920](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210112222324920.png)

- MyBatis 是一个 **半自动的 ORM (持久化层)** 框架，支持使用简单的 XML 或者注解来配置数据库和 Java POJO 对象的映射关系，并且将编写 sql 语句的自由交还到开发人员手中，能够做到 sql 和 Java 分开，开发人员只需要掌握 sql 语句编写，并且能够充分发挥 sql 的灵活性；

  ![image-20210112223207505](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210112223207505.png)

针对两者的区别，可以知道 Hibernate 具有一定的弊端：

1. 长难复杂的 sql ，不易处理；
2. 内部自动生成 sql ，有好也有坏，坏处就是不容易做特殊优化；
3. 基于全映射的全自动框架，大量字段的 POJO 进行部分映射时比较困难，导致数据库性能下降；

而 MyBatis 却恰恰弥补了 Hibernate 的这些不完美的地方：

1. MyBatis 是支持定制 sql ，存储过程以及高级映射的优秀的持久层框架；
2. MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集；
3. MyBatis 可以使用简单的 XML 或者注解用于配置和原始映射，将接口和 POJO(Plain Old Java Object，普通的 Java 对象) 映射成数据库中的记录； 

尤其是在当今的实际需求中，我们发现 Hibernate 确实厉害，但 MyBatis 学习成本低，使用简单，能达到目的，不增加额外工作量（或很少增加），从架构来说 MyBatis 功能单一，场景明确，耦合度低，无状态，便于(放在业务代码里)分布式部署，所以在大多数场景里，MyBatis 更适用。