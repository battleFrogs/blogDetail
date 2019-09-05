---
title: mysql常用命令
layout: post
tags:   
  - 常用命令  
---

MySQL 是最流行的关系型数据库管理系统，在 WEB 应用方面 MySQL 是最好的 RDBMS(Relational Database Management System：关系数据库管理系统)应用软件之一。

<!--more-->


## 内部命令   

    SELECT version(); 数据库版本查询方法    
    show engines; 查询引擎
    show create table xx; 查询xx表引擎
    select @@tx_isolation;//在MySQL数据库中查看当前事务的隔离级别：
    set tx_isolation=’隔离级别名称;’ //设置隔离级别   
    SELECT * FROM information_schema.INNODB_TRX //查询正在执行的事务
    SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS; //查询正在锁的事务
    SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;  //查看等待锁的事务  
    kill  线程id       杀掉线程   （根据这个事务的线程ID（trx_mysql_thread_id）
    select ... lock in share mode语句//共享锁   
    select ...for update //排他锁       
    BEGIN 开启一个事务   
    ROLLBACK 事务回滚    
    COMMIT 事务确认
    show variables like "innodb_locks_unsafe_for_binlog" （默认是OFF，间隙锁是开启的） 
    set @@autocommit = 0;想开启事务，设置为0，不然1时立即提交  
    select @@autocommit;  查询事务是否立即提交的状态
    show engine innodb status 获取死锁等情况说明
    
    
    SELECT distinct TABLE_NAME FROM INFORMATION_SCHEMA.COLUMNS 
    WHERE COLUMN_NAME = '字段名'  查询数据库字段名称对应的表
    

    
## 操作符    

    不等于： <> 或者 !=   
    不为null：is not null
    为null ： is null   
    条件范围 ： in   
    模糊查询  ： like "%id%"   
    正则表达式  ： regexp 


### 数据库和表操作    

    create database name; 创建数据库   
    
    use databasename; 选择数据库   
    
    drop database name 直接删除数据库，不提醒    
    
    show tables; 显示表


---


### 最基本语句   

    如果存在表删除：drop table if exists tb_area;

    创建表：CREATE TABLE `tb_area`(
            	`area_id` INT(2) NOT NULL AUTO_INCREMENT,
            	`area_name` VARCHAR(200) NOT NULL,
            	`priority` INT(2) NOT NULL DEFAULT '0',
            	`create_time` datetime DEFAULT NULL,
            	`last_edit_time` datetime DEFAULT NULL,
            	PRIMARY key(`area_id`),
            	UNIQUE KEY `UK_AREA`(`area_name`)
            )ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET='utf8';

    选择：select * from table1 where 范围

    插入：insert into table1(field1,field2) values(value1,value2)
    
    删除：delete from table1 where 范围
    
    更新：update table1 set field1=value1 where 范围
    
    
### 关键词的sql使用    

    （顺序）where -> group by -> having -> order by

    总数(count)：select count as totalcount from table1

    分组条件查询(group by和having)： select COUNT(*)  from table t group by t.field1 Having count(*)>2  

    排序(order by)：select * from table1 order by field1,field2 [desc]   （asc是升序，desc是降序）
    
    分页(limit)：select from user u limit start,pageSize(0索引值第一个开始，5取5条，start = （curpage - 1）* pageSize)
    
    查找(like)：select * from table1 where field1 like ’%value1%’   
    
    查询出并集所有结果不去除重复 （union）：select * from Table1 union select * from Table2   
    
    
    并集不去重复（union all） ： select * from Table1 union all select * from Table2  

    去重字段（distinct）：select distinct vent_id,price from customers//除非两个列都不同，这将检索出所有的列（不能针对所有单个字段不同情况）
    
    条件查询（in）：select s.name from  stu s where s.id in (1,2,3,4)

    条件查询(exists)：select s1.class from stu s1 where exists (select 1 from stu s2 where s2.name ='gjc' and s1.id = s2.id)      
    
    内连接（inner join）两表完全匹配会返回：select t1.*,t2.* from table1 t1 inner join table2 t2 on t1.id = t2.id   
    
    左连接（left join） 左表完全返回： select t1.*,t2.* from table1 t1 left join table2 t2 on t1.id = t2.id    
    
    右连接（right join） 右表完全返回：select t1.*,t2.* from table t1 right join table2 t2 on t1.id = t2.id  
    
    不同条件选择判断(case..when..end) :SELECT nickname,user_name,CASE WHEN user_rank = '5' THEN '经销商'  WHEN user_rank = '6' THEN '代理商'
    WHEN user_rank = '7' THEN 'VIP' ELSE '注册用户' END AS user_rank FROM at_users
    
    条件判断(IF(expr1,expr2,expr3)):select *,if(sva=1,"男","女") as ssva from taname where sva != ""   
    (如果 expr1 是TRUE (expr1 <> 0 and expr1 <> NULL)，则 IF()的返回值为expr2; 否则返回值则为 expr3。IF() 的返回值为数字值或字符串值，具体情况视其所在语境而定)

    

## 函数使用    
    
    求和[sum()]：select sum(field1) as sumvalue from table1
    
    平均[avg()]：select avg(field1) as avgvalue from table1
    
    最大[max()：select max(field1) as maxvalue from table1
    
    最小[min()]：select min(field1) as minvalue from table1    
    
    绝对值[abs()]: select abs(field1) as minvalue from table1    
    
    分组查询然后组合(group_concat()): select GROUP_CONCAT(score),stuName from grade1 GROUP BY stuName;[以stuName分组，把score字段的值打印在一行，逗号分隔(默认)]        
    
    查询某字段中以逗号分隔的字符串(find_in_set):SELECT * FROM test WHERE find_in_set('3',pnum) OR find_in_set('9',pnum);   
    
    自定义排序规则(field(value,str1,str2)):select * from test order by field(status,"FINISHED","DO")  //str1 ->1,str2->2,null或者不存在对应0
    
    
    
    
### 文本函数处理

    连接字符串[concat()]：select concat (id, name, score) as info from tt2;//结果字段拼接在一起 0gjc100
    
    返回从左边取指定长度的子串(LEFT)：select LEFT('abc123', 3)   ----> abc   
    
    返回从右边取指定长度的子串(Right): select RIGHT('abc123', 3)	----> 123   
    
    返回字符串长度(Length): select LENGTH('abc')	---->3    
    
    返回小写格式字符串(LOWER): select LOWER('ABC')	---->abc    
    
    返回大写格式字符串(Upper):select UPPER('abc')	---->ABC   
    
    将字符串左边空格去除后返回(Ltrim): select LTRIM('  abc')	---->abc    
    
    将字符串右边空格去除后返回(RTRIM):select RTRIM('abc  ')	---->abc   
    
    从字符串第2位开始截取3位字符(SUBSTRING) select SUBSTRING('abc123', 2, 3)---> bc1   

### 日期和时间处理    

    返回当前日期和时间[Now()]:select Now() --->2018-05-05 23:31:46    
    
    返回当前日期[CurDate()]:select CurDate()---->2018-05-05    
    
    日期运算函数[Date_Add()]:select Date_Add(CurDate(),INTERVAL 31 DAY)-->2018-06-05     
    日期格式化[Date_Format()]：select Date_Format(CurDate(),'%Y-%m-%d %H:%i:%S')   
    
    日期时间的日期部分[Date()]:select Date("2018-05-05 23:31:46")---->2018-05-05    
    
    日期时间的时间部分[Time()]:select Time("2018-05-05 23:31:46") --->23:31:46   
    
    日期时间比较相差[TIMESTAMPDIFF(type,pretime,latertime)] : SELECT * from table where TIMESTAMPDIFF(type,pretime,latertime)>100     
    (TIMESTAMPDIFF函数，需要三个参数，type是比较的类型，可以比较FRAC_SECOND、SECOND、 MINUTE、 HOUR、 DAY、 WEEK、 MONTH、 QUARTER或 YEAR几种类型，pretime是前一个时间，比较时用后一个时间减前一个时间)   
    
    日期范围[between 'dateTime1' and 'dateTime2']: select * from trade where  time1 between '2011-03-03 17:39:05' and '2011-03-03 17:39:52';   

---


## 刷数据sql集合       

**update或delete语句里含有子查询时，子查询里的表不能在update或是delete语句中**
### 批量update语句    


    UPDATE `core_user` a,
    (SELECT message_push_conf.user_id,message_push_conf.`open_push` 
    FROM `message_push_conf`) b 
    SET a.`switch_push`=b.open_push
    WHERE a.id=b.user_id     
    
    
    #子查询（不支持）
    //update question q set q.`level`=2 where q.id in( select id from  question where id>=2111 limit 165,165);
    
    
    #改写
    update question q inner join 
    ( select id from  question where id>=2111 limit 165,165) t 
    on q.id=t.id set q.`level`=2;    
    
    #另一种方式
    UPDATE s SET s.ClassName=c.ClassName FROM [dbo].[StudentDemo] s
    INNER JOIN [dbo].[ClassDemo] c ON s.ClassID=c.ClassID
    

### insert刷数据     

    1.两张表字段一致，并插入全部数据    
    INSERT INTO 目标表 SELECT  * FROM 来源表 ;

    2.要将 articles 表插入到 newArticles 表中，  
    INSERT INTO  newArticles SELECT  * FROM articles ;

    3.只希望导入指定字段   
    INSERT INTO 目标表 (字段1, 字段2, ...) SELECT 字段1, 字段2, ...FROM 来源表 ;

    
### delete 刷数据   

    delete from t1 where 条件 
    delete t1 from t1 where 条件 
    delete t1 from t1,t2 where 条件 （从数据表t1中把那些id值在数据表t2里有匹配的记录全删除 掉1）
    delete t1 from t1 left join t2 on t1.t_id = t2.t_id where t1.t_id >1;

    #子查询（不支持）
    //delete from `user` where id in (
       select min(id) as id from `user` group by wx_open_id having count(1)>1
    );
    
    #改写
    delete from `user` where id in (
        select id from(    
           select min(id) as id from `user` group by wx_open_id having count(1)>1
        ) t
    );



## 额外述说    

### STR_TO_DATE字符串转日期类型    

    SELECT STR_TO_DATE('2017-09-20 08:30:45',   '%Y-%m-%d %H:%i:%S');--->2017-09-20 08:30:45（日期Date格式）


### Date_FORMAT日期类型转字符串    

    SELECT DATE_FORMAT(NOW(),   '%Y-%m-%d %H:%i:%S'); --->2017-04-05 16:53:59（String型格式）

### 分组取最值   

> group by 然后取最大max或最小min 函数时，并不能按照分组的对应的查询出来    
  
解决方法：

1.先将sql排序，正序或者倒序，然后再groupby分组取值    

    select * from (select * from member_payment   
                order by id desc limit 10000000 ) t group by member_id limit 10  
    
2.联合查询，将最大值取出来，然后关联条件查询出    

    select * from `member_payment` where EXISTS (  
        select `id` from (  
            SELECT max(`id`) as id FROM `member_payment` group by `member_id` limit 10) t   
        where t.`id`=`member_payment`.`id`  
)  


### 主表信息加从表信息查询     

外面主表，查询显示字段有从表关联外表

    select 
    u.user_id userId,
    u.user_name userName,
    (select r.role_name from role r where r.user_id = u.user_id ) roleName,
    from user u
    where u.user_id = 1

### count(*)

    count(*)效率优于count(1)和count(key)   
    count(column)：表示结果集中有多少个column字段不为空的记录     
    count(*):表示整个结果集有多少条记录  
    
    多个查询count结合   
    Select A,
    count(B) as total,
    sum(case when B > 30 then 1 else 0 end) as total1,
    sum(case when B > 20 then 1 else 0 end) as total2 
    from ABC group by A
    
    

### 清空数据表    


    TRUNCATE TABLE  `mytable`   
    
    
### union的排序     

#### 1.使用了union All的不能进行各自排序（会报错）    

    SELECT `id`,`username`,`mobile`,`time`,id AS leader 
    FROM `grouporder_leader`
    WHERE `courseid` = 21 AND `merchid` = 23 AND `status` = 1 
    ORDER BY time DESC 
    UNION ALL 
    SELECT leadorderid,username,mobile,time,null 
    FROM `grouporder_partner`
    WHERE courseid=21 and status=1 and merchid=23 
    ORDER BY time DESC   
    
    
    
#### 2.可以通过起别名查询 起到先合并再排序的作用（正确）  

    # union all 的用法
    SELECT a.id,a.username,a.mobile,a.time,a.leader 
    FROM (SELECT `id`,`username`,`mobile`,`time`,id AS leader 
    	FROM `grouporder_leader` WHERE `courseid` = 21 AND `merchid` = 23 AND `status` = 1 
    	UNION ALL 
    	SELECT leadorderid,username,mobile,time,null 
    	FROM `grouporder_partner` WHERE courseid=21 and status=1 and merchid=23
    ) AS a 
    ORDER BY time DESC
    
    # union用法
    select supplier_id as id, supplier_name as name  
    from suppliers  
    UNION  
    select company_id as id, company_name as name  
    from companies  
    ORDER BY name;  
    

#### 3.先排序再合并的作用(正确)    
    
    语句1：select s1.id,s1.name from stu1 s1 order by stu2.score limit 9999999
    语句2: select s2.id,s2.name from stu2 s2 order by stu2.score  limit 9999999
    将语句1和语句2在包上一层select,例如  select t.* from ( 语句1) t union all select m.* from (语句2) m;