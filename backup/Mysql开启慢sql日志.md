查询慢查询日志是否开启，开启后会有一定性能影响
```
show variables like ‘%slow_query_log%’;
```

开启慢查询sql日志
```
set global slow_query_log=1;
```

查看慢SQL时间阀值，超过则记录进慢sql日志，需要重新连接
```
show variables like ‘long_query_time%’;
```

查看慢SQL时间阀值，超过则记录进慢sql日志-修改该值后直接便可查询
```
show global variables like ‘long_query_time’;
```

设置慢sql时间阀值
```
set global long_query_time=3
```

查看有多少条慢sql记录
```
show global status like ‘%Slow_queries%’;
```
