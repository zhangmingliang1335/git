# git
git常用的命令
MySQL:
		
SQL语言：由 
			DDL(data definition languaage)  数据定义语言 主要用于定义数据库,表  create alter  drop 数据库表的新建,修改，删除
			DML(data manipulation language) 数据操作语言 主要用于对数据增，删，改 insert update ,delete
			DQL(data query language)        数据查询语言 主要用于查询数据， select 
			DCL(data contorl language)      数据控制语言 主要是控制用户的访问权  grant(用与用户增加权限) revoke(收回权限) commit(提交事务)rollback(回滚事务)
			
命令启动Mysql：
				net start mysql
				
				net stop mysql
				
				mysql -h hostname -u username -p   -h 跟服务器地址 -u为用户名 -p 回车后位密码
				
				\s  可以查看服务器的配置信息  例如字符集.版本等。。。。。
				\G
				
数据库和表的基本操作：

					create database "bright"   创建数据库名为bright
					
					show databaes ;            显示所有的数据库；
					
					show create databae bright        显示bright数据库的信息 如character 字符集；
					
					alter database bright default character set gbk collate gbk_bin ;   设置bright的字符集为gbk;
					
					alter database bright default character set utf8 collate utf8_bin ;   设置bright的字符集为utf8;
					
					drop database brigth  删除数据库；
					
表的数据类型：
			整数型：tinyint  1个字节  smallint  2个字节    mediumint  3个字节  int  4个字节    bigint 8个字节
			浮点数与定点数(存储小数)：float 4个字节  double 8个字节      定点数 decimal(m,d)   decimal(6,2)  m：表示数据长度，d:为小数的个数
			
			日期与时间：year(年) ，data(日期) ， time(表示时间) ， datatime(日期与时间) ，timestamp(日期与时间)  
			datatime与timestamp不同处在timestamp在输入null与不输入后者是会默认存储系统当前时间
			
			字符串： char(固定长度)  varchar(不固定长度)  binary(二进制)  varbinary(不定长二进制)  text(大文本数据 ：文章内容)  常用的还有其他
			
创建与修改数据表：
				create table zml (字段名 数据类型[完整约束条件],字段名  数据类型[完整约束条件],)例如create table zml(id int(11),name varchar(20),)
				
				show tables ; 查看bright下所有的表 ；
				show create table zml 查看数据表 ； 不美观
				
				des 查看标的信息 但看不到字符集  推荐
				
				alter table zml rename to lzz   修改表名  zml-->lzz
				
				alter table lzz change garde password varchar(30);  更改lzz表中字段garde 为字段password 后面是数据类型；
				
				alter table lzz modify id char;    更改之前id 为的数据类型为int，更改之后为char类型；
				
				alter table lzz add age int(10)  first  或 after 字段名 ;    增加新的字段名并指定数据类型；frist与after可选
				
				alter table lzz drop age ;  删除字段名
				
				alter table lzz modify 字段1 数据类型 frist 或after 字段2      更改字段名的位置 ；
				
				drop  table lzz ;删除表lzz
				
表的约束：primary key    主键约束，唯一  一个表中只有一个 id primary key 或 primary key(id,student_id);
		  foreign key    外建约束
		  not null       非空约束           字段名 数据类型 null     age int(10) not null 
		  unique         唯一性约束
		  default        默认；
		  auto_increment; 自增

索引：提高查询的速度
		普通索引：
		唯一索引：是有unique定义索引
		全文索引：fulltext定义索引
		单列索引：在一个字段上创建索引，可以使所有类型的索引
		多列索引：在多个字段上创建索引，可以使所有类型的索引，并且查询只有使用了第一个索引字段该索引才会被使用；
		
创建索引：表不存在的情况下
		create table  zml(id int(10)  primary key,name  varchar(20) not null,index(id)); 创建普通索引
		create table  zml(id int(10)  primary key,name  varchar(20) not null, unique index unique_id(id asc)); 创建唯一索引
		create table  zml(id int(10)  primary key,name  varchar(20) not null, fulltext index fulltext_name(name) ); 全文索引
		create table  zml(id int(10)  primary key,name  varchar(20) not null,  index single_name(name) ); 单列索引；
		explain select * from zml where id=1 \G  可以查看索引

创建索引2：表存在的情况下：
			create index _index_id on zml (id); 普通
			create unique index uniqueidx on zml (id); 唯一
			create index singleidx on zml (id); 单列索引
			create INDEX mulitidx on zml (id ,name)  多列索引
			cretae fulltext index fulltextidx on zml (name) ; 全文索引
删除索引：drop index 索引名 on 表名	
		
创建外键：alter table student add constranint fk_id foreign key(外键字段名) references 主表表名(主键字段名)

删除外键：alter table student drop foreign key fk_id ;  

			
添加,更新,删除,查询：
					insert into 表名(字段1,字段2) values(value1,value2)   增加一条
					insert into 表名(字段1,字段2) values(value1,value2),(value1,value2)   增加多条
					update 表名 set 字段名=值,字段名=值                  更新 该字段所有的值
					update 表名 set 字段名=值,字段名=值  where 字段名=什么值  或者" > "   更新当什么=什么时才更新
					delete from 表名 表达式   delete from student where id=1  删除id=1的数据；
					
					truncate
					
					
					select distinct 字段名;   返回不重复的数据
					select * from student  查询所有字段名
					select id,name..... from student   where id >=10 and id <= 查询当id >=10 <= 一些字段数据
					select id,name..... from student   where id >=10 or id <= 查询当id >=10 <= 一些字段数据
					
					select * from student where id  not in (集合)   表示id 的值是否在集合中；
					
					select * from student where id between xx and xx ; 查询id xx 在什么之间 ； 
					
					select * from student where name is null        空值查询；
					
					select * fron student order by name  desc(升序) asc(降序)  对查询的结果name排序；
					
					select count(*) ,gender from student group by gender  对gender分组并统计分组之后的数量；
					select sum(grade) ,gender from student group by gender having sum(grade)  对gender分组并统计分组之后的数量；
					
					select * from student limit 4;  显示多少天数据  一般分页
					
					select * from student where name  not(可选) like 's%g'  '%y%' 匹配name值为s开头g结尾 ,匹配name包含y的值；'_下划线通配符'
聚合函数：

		 count() 返回某列的行数；
		 sum()   返回某列的值得和
		 avg()   返回某列的平均数
		 max()   返回某列的最大值
		 min()   返回某列的最小值
		 
		 
操作关联表：
			一对一：例如：一个人一张身份证
			多对一：例如：部门与员工的关系   一个部门有多个员工，一个员工只有一个部门；
			多对多：例如：课程与学生的关系   一个学生可以选多门课程，一门课程有多个学生；
			
连接查询：
		交叉连接：select * from  table1 corss join table2   对出现 table1*table2  对组合；
		
		内连接(inner(可省略) join)：  select * from table1 join table2 on table.id = table2.gid   或者  select * from table1 join table2 where table.id = table2.gid  (查询关联的数据)
		
		外连接：select * from  grade left join student on grade.id = student.id ;
		
		左连接：select * from  grade left join student on grade.id = student.id ;
		
		右连接：select * from  grade right join student on grade.id = student.id ;
		
		子连接：select * from grade  where id in (select gid from student where age="xxx")  其中gid与id是外键关联  年龄=的有多少个班级；
		
				带exists关键字查询 返回 boolean类型
				any
				all
事务与存储过程：
				start transaction  开启事务   (原子性)(一致性)(隔离性)(持久性)()
				
				commit  提交事务； 
				
				rollback  回滚事务；
				银行转账的例子： start transaction;                                    //开启事务
								 update accuount set money=money-100 where name='a';  a账户向b账户转100块
								 update accuount set money=money+100 where name='b';   b账户收到a转账的100块
								 commit                                                手动 提交操作，不提交退出系统重新登还是一样；
事务的隔离级别：

				read uncommitted  读未提交即读取到另一个事务未提交的数据  也可称作脏读；
				
				read committed   读提交  只能读取到已提交的事务数据   可避免脏读  但会出现重读和幻读

				repeatable read  重复读  可以避免以上两种的情况  mysql的默认事务隔离级别；
				
				serializable    可窜行化   最高的隔离事务   解决以上出现的问题    但是出现超时现象
				
				set seesion transaction isolation level read uncommitted  设置事务等级；
				
存储过程：
		creare procedure sp_name()     创建存储过程的关键字是 'procedure'   sp_name为名称；
		delimiter //        是将//作为MySQL的结束符避免与 ;冲突  注意delimiter //  之间有个空格
		
		create procedure sp_name()   创建存储过程 名为sp_name
		
		begin                        开始
		
		select * from person ;       sql语句；
		 
		end  //                        结束同时也要  //
		
		
		变量声明  declare(变量声明) variable(变量名) int(数据类型) default(默认值，没有default值为null) 100 ;      declare  var1 ,var2 int ;
		
		set variable =10 ,var1=30  为变量赋值；
		
		declare s_grade float ;
		declare s_gender varchar;
		
		select grade,gender into  s_grade,s_gender from student where name='rose'   
		
		将学生表中学生为rose 的字段grade，gender存入到变量s_grade，s_gender中；
		
		
调用存储过程  call sp_name();	


查看存储过程：show procedure status  	
				

			  
			
			
					
					
				
				
				
			
