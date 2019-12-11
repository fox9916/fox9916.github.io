---
layout:     post
title:      如何通过mybatis返回oracle自增长id
subtitle:   如何通过mybatis返回oracle自增长id
date:       2019-04-04
author:     漆黑的夜漫天的星
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - mybatis
    - mysql
    - oracle
    - 数据库
---
大家都知道像mysql,serveSql这样的数据库都提供有自增id的功能，而像oracle这样的数据库采取的是序列的方式来实现自增主键的，所以如果通过mybatis插入一条数据后返回主键id的方式也是不同的，网上有很多种说法，大都没说太明白，下面举个例子，详细说明一下，避免再入坑  

建表语句  


```
CREATE TABLE sys_student  

(  

id int PRIMARY KEY NOT NULL AUTO_INCREMENT,  

name varchar(50) COMMENT '名字'  

);  

ALTER TABLE sys_student COMMENT = '学生表';  
```


#### 一、mysql  
 mysql有自增功能，可以采用useGeneratedKeys="true"开启判断是否是自增ID， keyProperty="id" 指定插入数据后自增ID返回时赋值给实体类的那个属性(这里是id属性)

```
<insert id="insertStudent" useGeneratedKeys="true" keyProperty="id">
insert into sys_student(name) values(#{name})
</insert>
```

#### 二、oracle  
创建序列语句:  


```
CREATE SEQUENCE student_seq  INCREMENT BY 1  START WITH 1  NOMAXVALUE  NOCYCLE ;  
```
  

创建触发器：


```
CREATE OR REPLACE TRIGGER student_trigger 

BEFORE INSERT ON sys_student

FOR EACH ROW

BEGIN

SELECT student_seq.NEXTVAL INTO :NEW.id FROM DUAL;

END;
```

这里要说明一下，通常oracle实现自增，是通过触发器实现的。先建一个序列，再建一个触发器，每次在执行插入时，都会触发触发器实现自动加1。  
像Oracle数据库采用序列来作为自增主键，可以通过 selectKey子来获取主键值   

```
<insert id="insertStudent" useGeneratedKeys="false">

<selectKey resultType="java.lang.Integer" keyProperty="id" order="BEFORE">

select student_seq.nextval from dual

</selectKey>

insert into sys_student(name) values(#{name})

</insert>
```
 网上很多都是这样说的，但是最后的你可能会发现每次都自增2，而不是自增1，那么问题出在哪了呢？

问题就出在了这句‘ select student_seq.nextval from dual’，这句的意思就是每执行一次，自增序列都会加1，如果你的oracle是通过触发器实现的，那么恭喜你，你会发现每插入一条数据，都会自增2，之所以这样是因为你执行了两次 student_seq.nextval 。

还有一种就是利用max()函数，这种也是有问题的，而这个问题还不太容易发现，如果刚开始你的表里一条数据都没有，第一次执行，你会发现返回自增主键id，居然是null，这个问题应该也很好解决，把order="BEFORE" 改成 order="AFTER"，没准能行，我没有试，有兴趣可以自己试一下。


```
<insert id="insertStudent" useGeneratedKeys="false">

<selectKey resultType="java.lang.Integer"  keyProperty="id" order="BEFORE">

select max(id) from sys_student

</selectKey>

insert into sys_student(name) values(#{name})

</insert>

```

 

正确的方法是什么呢？
如果你有需要返回自增后的主键id，如果已经建了触发器，那么就删掉触发器，每次执行插入时，手动实现自增，如下，注意标红的地方，如果没有需要要返回自增id,那么采用触发器实现自增是完全没有问题的。
删除触发器:

```
DROP TRIGGER student_trigger;

```

```
<insert id="insertStudent" useGeneratedKeys="false">
<selectKey resultType="java.lang.Integer" keyProperty="id" order="AFTER">
select student_seq.currval from dual
</selectKey >
insert into sys_student(id,name) values(student_seq.nextval,#{name})
</insert>
```
如果觉得有用，不妨关注一下我的公众号  
![QDSCB8.jpg](https://s2.ax1x.com/2019/12/10/QDSCB8.jpg)
