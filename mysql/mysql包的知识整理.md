# mysql包的知识整理

### 0、安装包
```shell
npm install mysql -S
```

### 1、建立连接
```javascript
const mysql = require('mysql')
const connection = mysql.createConnection({
    host: '数据库服务器主机',
    user: '数据库用户名',
    password: '数据库密码'
})

connection.connect((err) => {
    if(err) {
        console.error(`连接错误：${err.stack}`)
        return;
    }

    console.log(`连接成功，连接id为：${connection.threadId}`)
})
```

### 2、常见连接选项
|属性|简介|默认值|
|---|---|---|
|host|连接的数据库主机名|'localhost'|
|port|连接的端口|3306|
|user|mysql用户名|无默认|
|password|用户名密码|无默认|
|database|用于此连接的数据库名称|可选参数，无默认|
|charset|字符排序规则|'UTF8_GENERAL_CI'|
|timezone|mysql服务器上配置的时区|'local'|
|connectTimeout|建立连接超时的毫秒数设置|10000|

### 3、关闭连接
```javascript
connection.end((err) => {
    // The connection is terminated now
})
```

### 4、创建连接池
```javascript
const mysql = require('mysql');
const pool  = mysql.createPool({
  connectionLimit : 10,
  host            : 'example.org',
  user            : 'bob',
  password        : 'secret',
  database        : 'my_db'
});

pool.getConnection((err, connection) => {
  if (err) throw err; // not connected!

  // Use the connection
  connection.query('SELECT something FROM sometable', (error, results, fields) => {
    // When done with the connection, release it.
    connection.release();

    // Handle error after the release.
    if (error) throw error;

    // Don't use the connection here, it has been returned to the pool.
  });
});
```

### 5、执行查询
```javascript
// 变量查询
connection.query('SELECT * FROM `books` WHERE `author` = ?', ['David'], function (error, results, fields) {
  // error will be an Error if one occurred during the query
  // results will contain the results of the query
  // fields will contain information about the returned results fields (if any)
});

// 带条件的变量查询
connection.query({
    sql: 'SELECT * FROM `books` WHERE `author` = ?',
    timeout: 40000, // 40s
  },
  ['David'],
  function (error, results, fields) {
    // error will be an Error if one occurred during the query
    // results will contain the results of the query
    // fields will contain information about the returned results fields (if any)
  }
);
```

### 6、事务
```javascript
connection.beginTransaction(function(err) {
  if (err) { throw err; }
  connection.query('INSERT INTO posts SET title=?', title, function (error, results, fields) {
    if (error) {
      return connection.rollback(function() {
        throw error;
      });
    }

    var log = 'Post ' + results.insertId + ' added';

    connection.query('INSERT INTO log SET data=?', log, function (error, results, fields) {
      if (error) {
        return connection.rollback(function() {
          throw error;
        });
      }
      connection.commit(function(err) {
        if (err) {
          return connection.rollback(function() {
            throw err;
          });
        }
        console.log('success!');
      });
    });
  });
});
```