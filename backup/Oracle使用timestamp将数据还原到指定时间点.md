Oracle使用timestamp将数据还原到指定时间点

误删数据怎么办？

### 查询还原时间点数据
```
select * from table_name as of timestamp to_timestamp('2018-03-27 15:40:00','yyyy-mm-dd hh24:mi:ss');
```

### 还原数据
```
flashback table table_name to timestamp to_timestamp('2018-03-27 15:40:00','yyyy-mm-dd hh24:mi:ss');
```
