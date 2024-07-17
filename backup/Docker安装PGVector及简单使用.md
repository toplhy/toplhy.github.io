1. docker 安装 postgresql
```
docker run -d -p 5432:5432 -e POSTGRES_PASSWORD=123456 --name postgres postgres:latest
```

2. 进入容器内部
```
docker exec -it CONTAINER_ID /bin/bash
```

3. 更新apt-get
```
apt-get update
```

4. 查看pg版本
```
psql -h localhost -U postgres -p 5432
show server_version;
```

4. 安装pgvector
```
apt-get install postgresql-15-pgvector
```

6. 添加 vector 扩展
```
psql -h localhost -U postgres -p 5432
create extension vector;
```

7. 查询可使用的扩展
```
select * from pg_available_extensions where name = 'vector';
```

8. 日常使用
+ 创建表：向量最多可以有16000个维度
```
create table test_vecotr(
    id bigserial primary key,
    embedding vector(3)
);
```

+ 已有表新增向量字段
```
alter table test add column embedding vector(3);
```

+ 插入向量数据
```
insert into test_vecotr(embedding) values('[1,2,3]'), ('[4,5,6]'),('[10,13,15]');
```

+ 更新向量数据
```
update test_vecotr set embedding = '[1,3,5]' where id = 1;
```

+ 查询数据
<table>
    <tr><th>操作</th><th>说明</th></tr>
    <tr><td><-></td><td>欧式距离</td></tr>
    <tr><td><#></td><td>负内积</td></tr>
    <tr><td><=></td><td>余弦距离</td></tr>
</table>

```
select * from test_vecotr order by embedding <-> '[1,4,5]' limit 5;
select * from test_vecotr where embedding <-> '[1,4,5]' < 5;
select embedding <-> '[1,4,5]' from test_vecotr;
select avg(embedding) from test_vecotr;
```

9. 索引
+ HNSW索引
HNSW索引创建了一个多层图。在速度-召回权衡方面，它的查询性能优于IVFFlat，但构建时间较慢且占用更多内存。
另外，由于没有像IVFFlat那样的训练步骤，可以在表中没有数据的情况下创建索引。

+ IVFFlat索引
IVFFlat索引将向量划分为列表，然后搜索最接近查询向量的那些列表的子集。
它的构建时间比HNSW快，且占用更少内存，但查询性能（就速度-召回权衡而言）较低。
