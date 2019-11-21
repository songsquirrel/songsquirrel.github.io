---
title: Oracle Sql优化
tags: Oracle
---

1. 选择最有效率的表名顺序(只在基于规则的优化器中有效)：
> Oracle的解析器按照从右到左的顺序处理FROM子句中的表名，FROM子句中写在最后的表(基础表 driving table)将被最先处理，在
> FROM子句中包含多个表的情况下,你必须选择记录条数最少的表作为基础表。如果有3个以上的表连接查询, 那就需要选择交叉表
> (intersection table)作为基础表, 交叉表是指那个被其他表所引用的表。
2. WHERE子句中的连接顺序：
> Oracle采用自下而上的顺序解析WHERE子句,根据这个原理,表之间的连接必须写在其他WHERE条件之前, 那些可以过滤掉最大数量记录
> 的条件必须写在WHERE子句的末尾。
3. 用EXISTS替换DISTINCT：
> 分组可以考虑使用exists
4. SQL语句用大写的：
> 因为Oracle总是先解析SQL语句，把小写的字母转换成大写的再执行。 
5. 避免在索引列上使用NOT: 
> 通常，我们要避免在索引列上使用NOT, NOT会产生在和在索引列上使用函数相同的影响。当Oracle“遇到”NOT,他就会停止使用索引
> 转而执行全表扫描。
6. 适当建立索引:
> 索引分类: b-tree索引, bitmap位图索引, 函数索引 (hash索引)
> 建立索引语句: 
``` sql
create[unique]|[bitmap] index index_name --UNIQUE表示唯一索引、BITMAP位图索引
on table_name(column1,column2...|[express])--express表示函数索引
[tablespace tab_name] --tablespace表示索引存储的表空间
[pctfree n1]    --索引块的空闲空间n1
[storage         --存储块的空间
 (
    initial 64K  --初始64k
    next 1M
    minextents 1
    maxextents unlimited  
)];
```
7. 建立分区表
> 当数据库中表数据量达到一定程度, 影响系统运行效率, 可以考虑对表进行分区:
> 分区语法:
``` sql
create table STUDENT.SCORE
(
  scoreid  VARCHAR2(18) not null,
  stuid    VARCHAR2(11),
  courseid VARCHAR2(9),
  score    NUMBER,
  scdate   DATE
)
partition by range(scdate)(
partition p_score_2018 values less than (TO_DATE('2019-01-01 00:00:00','yyyy-mm-ddhh24:mi:ss'))
 TABLESPACE TS_2018,
partition p_score_2019 values less than (TO_DATE('2020-01-01 00:00:00','yyyy-mm-ddhh24:mi:ss'))
 TABLESPACE TS_2019,
partition p_score_2020 values less than (MAXVALUE)
 TABLESPACE TS_2020
);
```
> 分区有根据范围(partition by range), 根据列表(partition by list), 根据hash(partition by hash)