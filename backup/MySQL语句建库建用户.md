### 建库建表语句

```
create database tax_interview_test default character set utf8 collate utf8_general_ci;

create user 'interview'@'%' identified by 'interview123';

grant all on tax_interview_test.* to interview@'%';

grant all privileges on *.* to 'interview'@'localhost' identified by 'interview123' with grant option;
```
