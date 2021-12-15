# 1. SQL优化

## <font color=red>1.1 最佳左前缀法则</font>

如果索引了多列，要遵守最左前缀法则。指的是查询从索引的最左前列开始并且<font color=red>不跳过索引中的列。</font>

带头大哥不能死，中间兄弟不能断。

## <font color=red>1.2 不要在索引列上做任何操作</font>

计算、函数、（自动or手动）类型转换等，会导致索引失效而转向全表扫描。

## <font color=red>1.3 存储引擎不能使用索引中范围条件右边的列</font>

索引列上不能有范围查询

```sql
select * from staffs where name = "julu" and age > 25 and pos = 'manager';
```

此时age用到了range级别索引，pos索引失效

## <font color=red>1.4 全值匹配我最爱</font>

## <font color=red>1.5 尽量使用覆盖索引</font>

只访问索引的查询，索引列和查询列一致，减少select *

## <font color=red>1.6 使用不等于（!=或者<>）的时候</font>

无法使用索引会导致全表扫描

## <font color=red>1.7 is null, is not null也无法使用索引</font>

## <font color=red>1.8 like以通配符开头（'%abc'）mysql索引失效会变成全表扫描的操作</font>

解决like '%字符串%' 时索引不被使用的方法?

使用覆盖索引，select name from table where name like '%aa%'

## <font color=red>1.9 字符串不加单引号索引失效</font>

## <font color=red>1.10 少用or，用它来连接时会索引失效</font>

## 1.11 group by和order by

定值、范围还是排序，一般order by是给个范围

1、order by子句，尽量使用Index方式排序，它指MySQL扫描索引方式本身完成排序，避免使用FileSort方式排序，FileSort方式效率低。

2、order by满足两种情况，会使用Index方式排序：1）order by 语句使用索引最左前列 2）使用where子句与order by子句条件列组合满足索引最左前列

![image-20210530145819043](/mysql_img/image-20210530145819043.png)

![image-20210530150306546](/mysql_img/image-20210530150306546.png)



group by 基本上都需要进行排序，会有临时表产生

![image-20210530151850857](/mysql_img/image-20210530151850857.png)



## 1.12 索引优化建立的单表分析

select * from table where a = 1 and b > 2 order by c desc limit 1

建立a,c索引才有效

## 1.13 索引两表优化案例分析

select * from a left join b on a.name = b.name

索引加在右表b，因为左表数据都有，主要找右表

三表查询也类似

<font color=red>Join语句的优化</font>

<font color=red>尽可能减少Join语句中的NestedLoop的循环总数：永远用小结果集驱动大的结果集</font>

## <font color=red>1.14 SQL分析流程</font>

1、观察，至少跑1天，看看生产的慢SQL情况。

2、开启慢查询日志，设置阙值，比如超过5秒钟的就是慢SQL，并将它抓取出来。

3、explain + 慢SQL分析

4、show profile

5、运维经理 or DBA，进行SQL数据库服务器的参数调优

## 1.15 排序优化

<font color=red>永远小表驱动大表</font>，因为外层循环越小，建立连接的次数越少

![image-20210530112358718](/mysql_img/image-20210530112358718.png)



![image-20210530113146035](/mysql_img/image-20210530113146035.png)

# 2. 慢查询日志

![image-20210530154317455](/mysql_img/image-20210530154317455.png)

![image-20210530154435758](/mysql_img/image-20210530154435758.png)

![image-20210530154607980](/mysql_img/image-20210530154607980.png)

使用set_global_query = 1开启了慢查询日志只对当前数据库生效，如果MySQL重启后则会失效。

![image-20210530173252641](/mysql_img/image-20210530173252641.png)

![image-20210530173803664](/mysql_img/image-20210530173803664.png)

设置慢查询超时命令

```sql
set global long_query_time = 3
```



![image-20210530174257409](/mysql_img/image-20210530174257409.png)

让数据库执行睡眠4秒

```sql
select sleep(4)
```

查询当前系统中有多少条慢查询记录

```sql
show global status like '%Slow_queries%'
```

![image-20210530175157057](/mysql_img/image-20210530175157057.png)

## 2.1 日志分析工具mysqldumpslow

本地测试命令使用不了，可能是版本问题

![image-20210530180925366](/mysql_img/image-20210530180925366.png)

# 3. 各种经验问题

## 3.1 mysql迁移数据

<font color=red>修改/etc/my.conf文件下的数据存放文件，把/var/lib/mysql文件下的数据迁移到其它目录，同时也要把socket位置迁移，否则不成功</font>

