# Mysql  

```
auto_increment 自增 
unique唯一 
not null 非空
```

-u用户名  -p密码 -h要连接的mysql服务器ip地址  -P端口号

char存储十个字节

varchar 自定义

------

drop table if exists (表名)； **删除表**

**建表时确定主键**
```
id int primary key auto_increment
```

**添加数据**

1.给列加数据 insert into(列名) 表名 values()

2.给表批量添加数据  insert into 表名 values(值1，值2)，(值1，值2)，(值1，值2)，

**修改数据**

```
update 表名 set (列名1=值1，列名2=值2) where (如：name="张三")

//无where则全修改
```

**删除数据**

delete from 表名(where 条件)

## 基础查询   

```
1.查询所有数据:select 列名 from 表名

2.去除重复记录:select distinct 字段列表 from 表名

3.起别名:select math as 数学成绩 (as 可省略)
```

## **条件查询**

```
select * from 表名 where 条件列表
```

| <> 或 !=       | 不等于                                   |
| -------------- | ---------------------------------------- |
| between… and … | 在范围之间                               |
| in (…)         | 在in之后的列表中的值，多选一             |
| like 占位符    | 模糊匹配( _匹配单个字符,%匹配任意个字符) |
| is null        | 是null                                   |
| is not null    | 不是null                                 |

**聚合函数查询**

select 聚合函数(字段列表) from 表名

```
count,max,min,avg(平均值),sum
```

**分组查询**

select sex，avg(math) from stu [where math >70]  group by sex;

```
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
```

**排序查询**

```
order by age desc/asc(升序，默认的),math desc/asc   若第一个相同，则比较第二个
```

**分页查询**

```
select 字段列表 from 表名 limit 起始索引，查询条数目  
```

起始索引=(当前页码-1)*每页显示的条数

## 多表查询

**一对多关系实现：在数据库表中多的一方，添加字段，来关联一对一方的主键。**

```
单行单列：(查询工资高于张三的员工信息）
select * from emp where salary >（select salary from emp where name='张三'）;

多行单列：(查询‘财务部’和‘市场部’所有的员工信息)
select *from emp where dep_id in (select did from dept where dname='财务部' or dname=‘市场部’);

多行多列[作为虚拟表]：(查询入职日期是2011.10.1之后的员工信息和部门信息)
select *from (select * from emp where indate >'2011.10.1') t1,dept where t1.dep_id = dept.did;
```



### 外键约束

使用 foreign key 定义外键关联另外一张表

（添加约束）

[constraint]  [外键名称] foreign key (外键列名(dep_id)） references 主表(主表列名(id))

（删除约束）

alter table 表名 drop foreign key 外键名称

------

### 内连接（查询AB交集数据）

隐式内连接：select (列表名)from 表1，表2 where 条件…;

```
select 列表名 from 表1，表2 where 表1.列表名 = 表2.列表名;
```

显式内连接： select (字段列表) from 表1 [inner] (可省略) join 表2 on 连接条件…;



### 外连接查询（左右外连接）

左外连接：(包含左表的全部信息)select 字段列表 from 表1 left join 表2 on 连接条件… ；

右外连接：(包含右表的全部信息) select 字段列表 from 表1 right join 表2 on 连接条件… ;

### 子查询

**嵌套查询**

```
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2)
```

**1）标量子查询:**返回单个值(数字、字符串、日期等)，最简单的形式，常用操作符：= < >

**2）列子查询：**返回一列(可以是多行),

常用操作符: in、not in等.

**3）行子查询：**子查询的返回结果是一行(可以是多列)。

常用操作符:=、<>、 in、not in等.

**4）表子查询：**子查询返回的结果是多行多列，常把表作为临时表

常用操作符: in

![image-20230712231053578](C:\Users\86131\AppData\Roaming\Typora\typora-user-images\image-20230712231053578.png)



## 事务

概念：**是一组操作的集合**，他是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作**要么同时成功，要么同时失败。**

**(特征：A原子性 C一致性 I隔离性 D持久性)**

<u>事务操作：</u>

——开启事务： start transaction / begin;

——提交事务： commit;

——回滚事务： rollback;
