# bookshelf
### 简介
NodeJs的关系型数据库的ORM库（基于knex）

### 安装
```bash
npm install knex -S
npm install bookshelf -S
npm install mysql -S
```
### 1、连接数据库
```JavaScript
const knex = require('knex')

// 读取数据库配置信息
require('env2')('../.env')
const {env} = process

// 连接数据库
const bookshelf = (database) => {
    let db = undefined
    let config = {
        client: 'mysql',
        connection: {
            host: env.mysqlHost,
            user: env.user,
            password: env.password,
            database: database,
            charset: 'utf8'
        }
    }
    // 保证数据库连接只初始化一次
    if(!db) {
        db = knex(config)
    }
    const bookshelf = require('bookshelf')(db)
    return bookshelf
}
```
### 2、创建model
```JavaScript
//  创建model
    let User = utils.bookshelf('cauth').Model.extend({
        tableName: 'csessioninfo'
    })
```

### 3、新建记录
```JavaScript
new User({
    name: 'King',
    userId: 123456
})
    .save()
    .then((model) => {
        // console.log(model);
    })
    .catch((err) => {
        // console.log(err);
    });
```
### 4、更新一条记录
```JavaScript
new User()
        .where({'userId': userId})
        .save({name: '魏振兴'}, {patch: true})
        .then((res) => {
            // console.log(res)
        })
        .catch((err) => {
            // console.log(err)
        })
```
### 5、查询所有记录
```JavaScript
new User()
        .fetchAll()
        .then((res) => {
            // console.log(res)
        })
        .catch((err) => {
            // console.log(err)
        })
```

### 6、查询一条记录
```JavaScript
new User()
        .where({'userId': userId})
        .fetch()
        .then((res) => {
            // console.log(res)
        })
        .catch((err) => {
            // console.log(err)
        })
```

### 7、删除一条记录
```JavaScript
new User()
        .where({'userId': userId})
        .destroy()
        .then((res) => {
            // console.log(res)
        })
        .catch((err) => {
            // console.log(err)
        })
```