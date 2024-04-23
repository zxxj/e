# MySQL

## 1.DDL(数据库设计)

```sql
-- 查询所有数据库
show databases;

-- 创建数据库
create database db02;

-- 创建表
create table tb_user
(
    id       int comment 'ID, 唯一标识',
    username varchar(20) comment '用户名',
    name     varchar(10) comment '姓名',
    age      int comment '年龄',
    gender   char(1) comment '性别'
) comment '用户表';

-- 表约束
create table tb_user
(
    id       int primary key auto_increment comment 'ID, 唯一标识',
    username varchar(20) unique not null comment '用户名',
    name     varchar(10)        not null comment '姓名',
    age      int comment '年龄',
    gender   char(1) default '男' comment '性别'
) comment '用户表';

-- DDL: 查看表结构
-- 查看当前数据库下所有的表
show tables;

-- 查看指定的表结构
desc tb_emp;

-- 查看某个数据库的建表语句
show create table tb_emp;

-- DDL: 修改表结构
-- 修改(添加字段): 为tb_emp表中添加字段 "qq" 数据类型为varchar(11)
alter table tb_emp add qq varchar(11) comment 'QQ号';

-- 修改varchar长度: 修改tb_emp表中字段名为qq的varchar长度调整为13
alter table tb_emp modify qq varchar(13) comment 'QQ号';

-- 修改字段名: 将tb_emp表中字段名为qq的字段修改为qq_num
alter table tb_emp change qq qq_number varchar(13) comment 'QQ号';

-- 删除表中的某个字段: 将tb_emp表中的qq字段删除
alter table tb_emp drop column qq_number;

-- 修改表名称: 将tb_emp表名修改为emp
rename table tb_emp to emp;
rename table emp to tb_emp;

-- 删除表: 删除tb_emp表
drop table if exists tb_emp;
```



## 2.DML(数据库操作-单表)

- **向表中插入数据**

  1. 指定字段添加数据: 

     ```sql
     insert into 表名 (字段名1, 字段名2) values (对应字段名1的值, 对应字段名2的值)
     ```

  2. 全部字段添加数据:

     ```sql
     insert into 表名 values (表中字段1的值, 表中字段2的值...)
     ```

  3. 批量添加数据(指定字段):

     ```sql
     insert into 表名 (字段名1, 字段名2) values (对应字段名1的值,对应字段名2的值), 
     																				(对应字段名1的值,对应字段名2的值)
     ```

  4. 批量添加数据(全部字段):

     ```sql
     insert into 表名 values (表中字段1的值, 表中字段2的值, 表中字段3的值,...),
     											(表中字段1的值, 表中字段2的值, 表中字段3的值,...)
     ```

     

  ```sql
  -- DML: 数据库操作语言
  -- DML: 插入数据 - insert
  
  -- 1.为tb_emp表的username,name,gender字段插入值
  insert into tb_emp(username, name, gender, create_time, update_time)
  values ('xinxin', '张鑫鑫', 1, now(), now());
  
  -- 2.为tb_emp表的所有字段插入值
  insert into tb_emp(id, username, password, name, gender, avatar, job, entrydate, create_time, update_time)
  values (null, 'kobe', '123', '科比', 1, '1.jpg', 2, '2024-04-23', now(), now())
  
  -- 3.批量为tb_emp表的username,name,gender字段插入数据
  insert into tb_emp(username, name, gender, create_time, update_time)
  values ('guoyiyuan', '郭益源', 2, now(), now()),
         ('tangqing', '唐清', 2, now(), now())
  ```

- 注意: 

  1. 插入数据时,指定的字段顺序需要与值的顺序是一一对应的
  2. 字符串和日期型数据应该包含在引号中
  3. 插入的数据大小,应该在字段的规定范围内

- **更新表中的数据**

  ```sql
  -- DML: 更新数据 - update
  -- 1.将tb_emp表的id为1的员工姓名更新为'张三'
  update tb_emp set name = '张三', update_time = now() where id = 1;
  
  -- 2.将tb_emp表中的所有员工的入职日期更新为'2024-10-01
  -- 注意: 修改语句的条件可以有,也可以没有,如果没有条件,则会修改整张表中的所有数据
  update tb_emp set entrydate = '2024-10-01', update_time = now();
  ```

- 删除表中的数据

  delete语法:

  ```sql
  delete from 表名 [where 条件];
  ```

  ```sql
  -- DML: 删除数据 - delete
  
  -- 1.删除tb_emp表中id为1的员工
  delete from tb_emp where id = 1;
  
  -- 2.删除tb_emp表中所有的员工
  delete from tb_emp;
  ```

  - 注意: 
    1. DELETE语句的条件可以有,也可以没有,如果没有条件,则会删除整张表中的所有数据
    2. DELETE语句不能删除某一个字段的值,如果有这个想法,可以使用UPDATE,将该字段的值重置为NULL

## 3.DQL(数据库查询)

- **语法**:

```sql
基本查询: {
	select => 字段列表
	from => 表名列表
}

条件查询: {
	where => 条件列表
}

分组查询: {
	group by => 分组字段列表
	having => 分组后条件列表
}

排序查询: {
	order by => 排序字段列表
}

分页查询: {
	limit => 分页参数
}
```

- **基本查询**

  语法: 

  ```sql
  查询多个字段: select 字段1, 字段2, 字段3 from 表名;
  查询所有字段(通配符): select * from 表名;
  设置别名: select 字段1 as 别名1, 字段2 as 别名2 from 表名;
  去除重复记录: select distinct 字段列表 from 表名;
  
  -- *号代表查询所有字段,在实际开发中尽量少用(不直观,影响效率)
  ```

  

  ```sql
  -- 创建表结构
  create table tb_emp
  (
      id          int unsigned primary key auto_increment comment 'ID',
      username    varchar(20)      not null unique comment '用户名',
      password    varchar(32) default '123456' comment '密码',
      name        varchar(10)      not null comment '姓名',
      gender      tinyint unsigned not null comment '性别, 说明: 1 男, 2 女',
      image       varchar(300) comment '图像',
      job         tinyint unsigned comment '职位, 说明: 1 班主任,2 讲师, 3 学工主管, 4 教研主管',
      entrydate   date comment '入职时间',
      dept_id     int unsigned comment '归属的部门ID',
      create_time datetime         not null comment '创建时间',
      update_time datetime         not null comment '修改时间'
  ) comment '员工表';
  
  -- 向表中插入数据
  INSERT INTO tb_emp
  (id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time)
  VALUES (1, 'jinyong', '123456', '金庸', 1, '1.jpg', 4, '2000-01-01', 2, now(), now()),
         (2, 'zhangwuji', '123456', '张无忌', 1, '2.jpg', 2, '2015-01-01', 2, now(), now()),
         (3, 'yangxiao', '123456', '杨逍', 1, '3.jpg', 2, '2008-05-01', 2, now(), now()),
         (4, 'weiyixiao', '123456', '韦一笑', 1, '4.jpg', 2, '2007-01-01', 2, now(), now()),
         (5, 'changyuchun', '123456', '常遇春', 1, '5.jpg', 2, '2012-12-05', 2, now(), now()),
         (6, 'xiaozhao', '123456', '小昭', 2, '6.jpg', 3, '2013-09-05', 1, now(), now()),
         (7, 'jixiaofu', '123456', '纪晓芙', 2, '7.jpg', 1, '2005-08-01', 1, now(), now()),
         (8, 'zhouzhiruo', '123456', '周芷若', 2, '8.jpg', 1, '2014-11-09', 1, now(), now()),
         (9, 'dingminjun', '123456', '丁敏君', 2, '9.jpg', 1, '2011-03-11', 1, now(), now()),
         (10, 'zhaomin', '123456', '赵敏', 2, '10.jpg', 1, '2013-09-05', 1, now(), now()),
         (11, 'luzhangke', '123456', '鹿杖客', 1, '11.jpg', 1, '2007-02-01', 1, now(), now()),
         (12, 'hebiweng', '123456', '鹤笔翁', 1, '12.jpg', 1, '2008-08-18', 1, now(), now()),
         (13, 'fangdongbai', '123456', '方东白', 1, '13.jpg', 2, '2012-11-01', 2, now(), now()),
         (14, 'zhangsanfeng', '123456', '张三丰', 1, '14.jpg', 2, '2002-08-01', 2, now(), now()),
         (15, 'yulianzhou', '123456', '俞莲舟', 1, '15.jpg', 2, '2011-05-01', 2, now(), now()),
         (16, 'songyuanqiao', '123456', '宋远桥', 1, '16.jpg', 2, '2010-01-01', 2, now(), now()),
         (17, 'chenyouliang', '123456', '陈友谅', 1, '17.jpg', NULL, '2015-03-21', NULL, now(), now());
  
  -- DQL: 基本查询
  
  -- 1.查询某张表中的指定字段如: name, entrydate并返回
  select name, entrydate from tb_emp;
  
  -- 2.查询某张表中的所有字段并返回
  select * from tb_emp; -- 不推荐,性能低
  
  -- 推荐这样查询所有字段
  select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from tb_emp;
  
  -- 3.查询所有员工的name,entrydate,并起别名(姓名,入职日期)
  select name as 姓名, entrydate as 入职日期 from tb_emp;
  select name as '姓 名', entrydate 入职日期 from tb_emp;
  
  -- 4.查询已有的员工关联了哪些职位(不能重复)
  select distinct job from tb_emp;	
  ```

  