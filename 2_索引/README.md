# 正确使用索引

数据库表中添加索引后确实会让查询速度起飞，但前提必须是正确的使用索引来查询，如果以错误的方式使用，则即使建立索引也会不奏效。

即使建立索引，索引也不会生效：
```

- like '%xx'
       select * from tb1 where name like '%cn';
   - 使用函数
       select * from tb1 where reverse(name) = 'wupeiqi';
   - or
       select * from tb1 where nid = 1 or email = 'seven@live.com';
       特别的：当or条件中有未建立索引的列才失效，以下会走索引
               select * from tb1 where nid = 1 or name = 'seven';
               select * from tb1 where nid = 1 or email = 'seven@live.com' and name = 'alex'
   - 类型不一致
       如果列是字符串类型，传入条件是必须用引号引起来，不然...
       select * from tb1 where name = 999;
   - !=
       select * from tb1 where name != 'alex'
       特别的：如果是主键，则还是会走索引
           select * from tb1 where nid != 123
   - >
       select * from tb1 where name > 'alex'
       特别的：如果是主键或索引是整数类型，则还是会走索引
           select * from tb1 where nid > 123
           select * from tb1 where num > 123
   - order by
       select email from tb1 order by name desc;
       当根据索引排序时候，选择的映射如果不是索引，则不走索引
       特别的：如果对主键排序，则还是走索引：
           select * from tb1 order by nid desc;
 
   - 组合索引最左前缀
       如果组合索引为：(name,email)
       name and email       -- 使用索引
       name                 -- 使用索引
       email                -- 不使用索引
```
