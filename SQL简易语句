# SQL 简单语句

##### 基本语法
```
-- 通过 * 查询表中所有数据
-- SELECT * FROM my_db_01.users

-- 从user 表中把 username 和 password 对应的数据查询出来alter
-- SELECT username, password from my_db_01.users

-- 向表中插入 Tony 和密码：098123
-- INSERT INTO my_db_01.users (username,password) values('Tony','098123')
-- insert into my_db_01.users (id,username,password) values(1,'zs','123')

-- UPDATA 语句
-- UPDATE my_db_01.users SET password = '123456' WHERE id = 4
-- 同时更新多个列
-- UPDATE my_db_01.users SET password = 'admin123', status = 1 where id = 2

-- DELETE 语句 DELETE FROM 
-- delete from my_db_01.users where id = 1
```

##### ORDER BY 子句
```
SELECT * FROM users ORDER BY status
SELECT * FROM users ORDER BY status ASC

降序排序
SELECT * FROM users ORDER BY status DESC
```

##### ORDER BY 子句 - 多重排序
```
对 user 表中的数据，先按照 status 进行降序排序，再按照 username 字母的顺序，进行升序的排序
SELECT * FROM users ORDER BY status DESC, username ASC
```

##### COUNT(*)
用于返回查询结果的总数据条数。

```
-- 使用 COUNT(*) 来统计 users 表中，状态为 0 用户的总数量
SELECT COUNT(*) FROM users WHERE status= 0
```
##### NodeJS中使用mySQL
```
const mysql = require('mysql')

const db = mysql.createPool(
    {host: '127.0.0.1', user: 'root', password: 'ggat.123', database: 'my_db_01'}
)
// 查询users表中所有的数据
// 注意：如果执行的是 select 查询语句，则执行的结果是数组
const sqlStr = 'SELECT * FROM users'

db.query(sqlStr,(err,res) =>{
    if(err) return console.log(err.message);
    //查询成功
    console.log(res);
})
```

##### 使用 AS 为列设置别名
```
SELECT COUNT(*) AS total FROM users WHERE status = 0
```

##### 插入数据
```
const mysql = require('mysql')

const db = mysql.createPool(
    {host: '127.0.0.1', user: 'root', password: 'ggat.123', database: 'my_db_01'}
)
// 向users 表中，新增一条数据，其中username的值为spider-man,password为pcc123

const user = {username:'spider-man',password:'pcc123'}

const sqlStr = 'insert into users (username,password) values(?,?)'

db.query(sqlStr,[user.username,user.password],(err,res) =>{
    if(err) return console.log(err.message);
    if(res.affectedRows === 1){
        console.log('插入数据成功');
    }
})
```

##### 插入数据的便捷方式
向表中新增数据时，如果数据对象的每个属性和数据表的字段一一对应，则可以：
```
const user = {username: 'Spider-Man2', password: 'pcc4321'}
const sqlStr = 'INSERT INTO users SET ?'
db.query(sqlStr, user, (err, results) => {
    if(err) return console.log(err.message);
    if(results.affectedRows === 1 ){console.log('插入数据成功')}
})
```

##### 下周内容
