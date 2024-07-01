## 查询表的最后修改时间
```
select uat.* from user_all_tables uat;

select object_name, created,last_ddl_time from user_objects;

select 
  uat.table_name as "表名",
  (select wmsys.wm_concat(to_char(last_ddl_time, 'yyyy-MM-dd HH24:mi')||'、') from user_objects where object_name = uat.table_name ) as "最后修改日期" 
from user_all_tables uat 
where uat.tablespace_name = 'ZHIYE';
```
