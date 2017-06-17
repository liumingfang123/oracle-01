--1.2子查询分类 单行子查询、多行子查询
--子查询中的HAVING子句
SELECT deptno,MIN(sal)
FROM emp GROUP BY deptno
HAVING MIN(sal) >    (SELECT MIN(sal)  FROM emp  WHERE deptno=10);
/*类型	                                                                    操作
  DML   [data maniplulation language 数据操作语言]（表记录）          	    insert update delete select(DQL)
  DDL   [data definition language数据定义语言]（表结构）	                  create table,alter table,drop table,truncate table,crate/drop 
                                                                         view,sequence(序列),index(索引),synonym(同义词)
  DCL   [data control language 数据控制语言]                             	grant(授权)，revoke（撤销权限）*/
  
                    
--地址符&
Insert into emp(empno,ename,sal,deptno) values(&empno,&ename,&sal,&deptno);
--批处理
-- 一次性将emp中所有10号部门的员工插入到emp3中
insert into emp3 select * from emp where deptno=10;
/*   delete 和 truncate的区别
	1.delete 逐条删除：truncate先销毁表，再重建表
	2.delete语句是DML[可以rollback],truncate是DDL[不可以rollback]
	3.delete不会释放空间，truncate会释放空间
	4.delete可以闪回(flashback)，truncate不可以
  5.delete会产生碎片, truncate不会产生碎片*/
  
/*     事务的隔离级别
对于同时运行的多个事务，当这些事务访问数据库中相同的数据时,如果没有采取必要的隔离机制，就会导致各种并发问题：
   1 脏读：对于两个事务T1,T2,T1读取了已经被T2更新但还没有被提交的字段之后,若T2回滚，T1读取的内容就是临时且无效的。
   2 不可重复读（针对记录）：对于两个事务T1，T2，T1读取了一个字段,然后T2更新了该字段之后，T1再此读取同一个字段，值就不同了。
   3 幻读（针对表）：对于两个事务T1，T2，T1从一个表中读取了一个字段,然后T2在该表中插入了一些新的行之后，如果T1再次读取同一个表,就会多出几行。
     数据库事务的隔离性：数据库系统必须具有隔离并发运行各个事务的能力，使他们不会相互影响，避免各种并发问题。
    一个事务与其他事务隔离的程度称为隔离级别，数据库规定了多种事务隔离级别,不同隔离级别对应不同的干扰程度,隔离级别越高，数据一致性越好,但并发性越弱。
数据库提供的四种隔离级别  
隔离级别                             描述
Read uncommitted(读未提交的数据)   允许事务读取未被其他事务提交的变更，脏读，不可重复读和幻读的问题都会出现
Read commited(读已提交的数据)      只允许读取已经被其他事务提交的变更，可以避免脏读，但是不可重复读和幻读问题任然可能出现；
Repeatable read (可重复读)        确保事务可以从一个字段中读取相同的值，在这个事务持续期间，禁止其他事务对这个字段进行更新，可以避免脏读和不可重复                                  读，但是幻读的问题任然存在。
Serlalizable(串行化)             确保事务可以从一个表中读取相同的行，在这个事务持续期间，禁止其他事务对该表执行插入，更新和删除操作所有并发问题都可                                 以避免，但性能十分低下
                                
*/
