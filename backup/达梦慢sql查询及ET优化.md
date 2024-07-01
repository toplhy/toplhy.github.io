达梦慢sql查询及ET优化

### 达梦慢sql查询

当 INI 参数 ENABLE_MONITOR=1、MONITOR_TIME=1 打开时，显示系统最近 1000 条执行时间超过预定值的 SQL 语句。

默认预定值为 1000 毫秒。可通过 SP_SET_LONG_TIME 系统函数修改，通过 SF_GET_LONG_TIME 系统函数查看当前值。

+ 两个参数均为动态参数，可直接调用系统函数进行修改。

```
SP_SET_PARA_VALUE(1,'ENABLE_MONITOR',1);
SP_SET_PARA_VALUE(1,'MONITOR_TIME',1);
```

+ 超过执行时间阈值的 SQL 语句记录在 V$LONG_EXEC_SQLS 系统视图中。

```
SELECT * FROM V$LONG_EXEC_SQLS;

---------------------------------------
各字段详细信息介绍
列名	说明
SESS_ID	会话 ID，会话唯一标识
SQL_ID	语句 ID，语句唯一标识
SQL_TEXT	SQL 文本
EXEC_TIME	执行时间（毫秒）
FINISH_TIME	执行结束时间
N_RUNS	执行次数
SEQNO	编号
TRX_ID	事务号
```

### ET优化单条SQL

ET 功能默认关闭，可通过配置 INI 参数中的 ENABLE_MONITOR=1、MONITOR_SQL_EXEC=1 开启该功能。

+ 两个参数均为动态参数，可直接调用系统函数进行修改。

```
-- 开启ET
SP_SET_PARA_VALUE(1,'ENABLE_MONITOR',1);
SP_SET_PARA_VALUE(1,'MONITOR_SQL_EXEC',1);
-- 关闭 ET
SP_SET_PARA_VALUE(1,'ENABLE_MONITOR',0);
SP_SET_PARA_VALUE(1,'MONITOR_SQL_EXEC',0);
```

+ ET 查看方式
执行 SQL 语句后，客户端会返回 SQL 语句的执行号。鼠标单机执行号即可查看 SQL 语句对应的 ET 结果。

如果没有图形界面，调用存储过程可返回相同结果

```
CALL ET(55);

---------------------------------------
ET 结果说明
OP: 操作符
TIME(us): 时间开销，单位为微秒
PERCENT: 执行时间占总时间百分比
RANK: 执行时间耗时排序
SEQ: 执行计划节点号
N_ENTER: 进入次数
```
