

----------------------------------------------------------------------------------------------------------------------------------------

数据库范式？

粗略的理解是：一张数据表结构所符合的某种设计标准的级别。
常见的范式有:1NF  2NF  3NF 

1、1NF(第一范式)

第一范式是指数据库表中的每一列都是不可分割的基本数据项，同一列中不能有多个值，即实体中的某个属性不能有多个值或者不能有重复的属性。
简而言之，第一范式就是无重复的列。
例如，由“职工号”“姓名”“电话号码”组成的表(一个人可能有一部办公电话和一部移动电话)，这时将其规范化为1NF可以将电话号码分为“办公电话”和“移动电话”两个属性，即职工(职工号，姓名，办公电话，移动电话)。

2、2NF(第二范式)

第二范式(2NF)是在第一范式(1NF)的基础上建立起来的，即满足第二范式(2NF)必须先满足第一范式(1NF)。第二范式(2NF)要求数据库表中的每个实例或行必须可以被唯一地区分。为实现区分通常需要为表加上一个列，以存储各个实例的唯一标识。

3、3NF（第三范式）

函数依赖我们可以这么理解（但并不是特别严格的定义）：若在一张表中，在属性（或属性组）X的值确定的情况下，必定能确定属性Y的值，那么就可以说Y函数依赖于X，写作 X → Y。也就是说，在数据表中，不存在任意两条记录，它们在X属性（或属性组）上的值相同，而在Y属性上的值不同。这也就是“函数依赖”名字的由来，类似于函数关系 y = f(x)，在x的值确定的情况下，y的值一定是确定的。

满足2NF,非主键外的所有字段必须互不依赖。

如：学号，课程号，系别，系主任。系别和系主任是非主属性，学号和课程号是主属性，但是系别和系主任这两个非主属性之间有函数依赖关系：系别→系主任。比造成函数依赖：学号→系别一系主任。
解决方案：将这个非主属性与其依赖的码拿出来单独建表，并设置依赖的属性为主键，在原表中使用外键表示。

----------------------------------------------------------------------------------------------------------------------------------------
数据库的事务的四大特性：

⑴ 原子性（Atomicity）
　　原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚。
  
  ⑵ 一致性（Consistency）
　　一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。

　　拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性。
  
  ⑶ 隔离性（Isolation）
　　隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。

　　即要达到这么一种效果：对于任意两个并发的事务T1和T2，在事务T1看来，T2要么在T1开始之前就已经结束，要么在T1结束之后才开始，这样每个事务都感觉不到有其他事务在并发地执行。
  
  ⑷ 持久性（Durability）
　　持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。

　　例如我们在使用JDBC操作数据库时，在提交事务方法后，提示用户事务操作完成，当我们程序执行完成直到看到提示后，就可以认定事务以及正确提交，即使这时候数据库出现了问题，也必须要将我们的事务完全执行完成，否则就会造成我们看到提示事务处理完毕，但是数据库因为故障而没有执行事务的重大错误。
  
  
  
--------------------------------------------------------------------------------------------------------------------------------------
 
 数据库事务的隔离级别
 
 当多个线程都开启事务操作数据库中的数据时，数据库系统要能进行隔离操作，以保证各个线程获取数据的准确性，在介绍数据库提供的各种隔离级别之前，我们先看看如果不考虑事务的隔离性，会发生的几种问题：
  1，脏读
　　脏读是指在一个事务处理过程里读取了另一个未提交的事务中的数据。

　　当一个事务正在多次修改某个数据，而在这个事务中这多次的修改都还未提交，这时一个并发的事务来访问该数据，就会造成两个事务得到的数据不一致。例如：用户A向用户B转账100元，对应SQL命令如下

    update account set money=money+100 where name=’B’;  (此时A通知B)

    update account set money=money - 100 where name=’A’;
　　当只执行第一条SQL时，A通知B查看账户，B发现确实钱已到账（此时即发生了脏读），而之后无论第二条SQL是否执行，只要该事务不提交，则所有操作都将回滚，那么当B以后再次查看账户时就会发现钱其实并没有转。
  
  2，不可重复读
　　不可重复读是指在对于数据库中的某个数据，一个事务范围内多次查询却返回了不同的数据值，这是由于在查询间隔，被另一个事务修改并提交了。

　　例如事务T1在读取某一数据，而事务T2立马修改了这个数据并且提交事务给数据库，事务T1再次读取该数据就得到了不同的结果，发送了不可重复读。

　　不可重复读和脏读的区别是，脏读是某一事务读取了另一个事务未提交的脏数据，而不可重复读则是读取了前一事务提交的数据。

　　在某些情况下，不可重复读并不是问题，比如我们多次查询某个数据当然以最后查询得到的结果为主。但在另一些情况下就有可能发生问题，例如对于同一个数据A和B依次查询就可能不同，A和B就可能打起来了……
  
  3，虚读(幻读)
　　幻读是事务非独立执行时发生的一种现象。例如事务T1对一个表中所有的行的某个数据项做了从“1”修改为“2”的操作，这时事务T2又对这个表中插入了一行数据项，而这个数据项的数值还是为“1”并且提交给数据库。而操作事务T1的用户如果再查看刚刚修改的数据，会发现还有一行没有修改，其实这行是从事务T2中添加的，就好像产生幻觉一样，这就是发生了幻读。

　　幻读和不可重复读都是读取了另一条已经提交的事务（这点就脏读不同），所不同的是不可重复读查询的都是同一个数据项，而幻读针对的是一批数据整体（比如数据的个数）。
  
  
  现在来看看MySQL数据库为我们提供的四种隔离级别：

　　① Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。

　　② Repeatable read (可重复读)：可避免脏读、不可重复读的发生。

　　③ Read committed (读已提交)：可避免脏读的发生。

　　④ Read uncommitted (读未提交)：最低级别，任何情况都无法保证。
  
  以上四种隔离级别最高的是Serializable级别，最低的是Read uncommitted级别，当然级别越高，执行效率就越低。像Serializable这样的级别，就是以锁表的方式(类似于Java多线程中的锁)使得其他的线程只能在锁外等待，所以平时选用何种隔离级别应该根据实际情况。在MySQL数据库中默认的隔离级别为Repeatable read (可重复读)。
  
  在MySQL数据库中，支持上面四种隔离级别，默认的为Repeatable read (可重复读)；而在Oracle数据库中，只支持Serializable (串行化)级别和Read committed (读已提交)这两种级别，其中默认的为Read committed级别。

　　在MySQL数据库中查看当前事务的隔离级别：

    select @@tx_isolation;
　　在MySQL数据库中设置事务的隔离 级别：

    set  [glogal | session]  transaction isolation level 隔离级别名称;

    set tx_isolation=’隔离级别名称;’
    
--------------------------------------------------------------------------------------------------------------------------------------
 
 左连接和右连接区别

1、内联接（典型的联接运算，使用像 =  或 <> 之类的比较运算符）。包括相等联接和自然联接。     
内联接使用比较运算符根据每个表共有的列的值匹配两个表中的行。例如，检索 students和courses表中学生标识号相同的所有行。   


2、外连接。外联接可以是左向外联接、右向外联接或完整外部联接。     
在 FROM子句中指定外联接时，可以由下列几组关键字中的一组指定：     
1）LEFT  JOIN或LEFT OUTER JOIN     
left join 是left outer join的简写，它的全称是左外连接，是外连接中的一种。

左(外)连接，左表(a_table)的记录将会全部表示出来，而右表(b_table)只会显示符合搜索条件的记录。右表记录不足的地方均为NULL。      
2）RIGHT  JOIN 或 RIGHT  OUTER  JOIN     
right join是right outer join的简写，它的全称是右外连接，是外连接中的一种。

与左(外)连接相反，右(外)连接，左表(a_table)只会显示符合搜索条件的记录，而右表(b_table)的记录将会全部表示出来。左表记录不足的地方均为NULL。     
3）FULL  JOIN 或 FULL OUTER JOIN
完整外部联接返回左表和右表中的所有行。当某行在另一个表中没有匹配行时，则另一个表的选择列表列包含空值。如果表之间有匹配行，则整个结果集行包含基表的数据值。  

3、交叉联接   
交叉联接返回左表中的所有行，左表中的每一行与右表中的所有行组合。交叉联接也称作笛卡尔积。    
FROM 子句中的表或视图可通过内联接或完整外部联接按任意顺序指定；但是，用左或右向外联接指定表或视图时，表或视图的顺序很重要。有关使用左或右向外联接排列表的更多信息，请参见使用外联接。
 
     
--------------------------------------------------------------------------------------------------------------------------------------
     
 union和union all的区别
 
 如果我们需要将两个select语句的结果作为一个整体显示出来，我们就需要用到union或者union all关键字。

union(或称为联合)的作用是将多个结果合并在一起显示出来。

union和union all的区别是,union会自动压缩多个结果集合中的重复结果，而union all则将所有的结果全部显示出来，不管是不是重复。

Union：对两个结果集进行并集操作，不包括重复行，同时进行默认规则的排序；

Union All：对两个结果集进行并集操作，包括重复行，不进行排序；

Intersect：对两个结果集进行交集操作，不包括重复行，同时进行默认规则的排序；

Minus：对两个结果集进行差操作，不包括重复行，同时进行默认规则的排序。

可以在最后一个结果集中指定Order by子句改变排序方式。
     
     
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
     
--------------------------------------------------------------------------------------------------------------------------------------
 
  