# koa-router
## API参考
![api-reference](https://i.imgur.com/6o7hQO0.png)
## 基础用例
```javascript
const Koa = require('koa');
// 导入Router类
const Router = require('koa-router');

const app = new Koa();
// 创建router实例对象
const router = new Router();

// 调用router实例的get方法，返回一个router实例
router.get('/', (ctx, next) => {
  ctx.body = "你好， koa-router!"；
});


app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```

## router.get|put|post|patch|delete|del
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

router
  .get('/', (ctx, next) => {
    ctx.body = '首页';
  })
  .post('/users', (ctx, next) => {
    ctx.body = '更新用户信息列表';
  })
  .put('/users/:id', (ctx, next) => {
    ctx.body = '更新特定id的用户信息';
  })
  .del('/users/:id', (ctx, next) => {
    ctx.body = '删除指定id用户'
  })
  .all('/users/:id', (ctx, next) => {
    // 用于匹配所有方法
    ctx.body = '用于匹配所有方法'
  });

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```

## 命名路由(路由可以进行名称。这允许在开发期间生成URL并轻松修改URL)
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

// 命名路由（命名为user）
router.get('user', '/users/:id', (ctx, next) => {
	ctx.body = '获取特定id的用户信息';
});

// router实例调用url方法生成url
const getUserInfoUrl = router.url('user', 3);
console.log(getUserInfoUrl);

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```
## 多个中间件处理
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

// 多个中间件
let result = '';
router.get(
  '/',
  (ctx, next) => {
    result += '第一个中间件处理\n';
    next();
  },
  ctx => {
    result += '第二个中间件处理';
    ctx.body = result;
  }
);

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```
## 嵌套路由
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();

// 嵌套路由
const parent = new Router();
const child = new Router();

child.get('/', (ctx, next) => {
  ctx.body = `我是${ctx.url}`;
});

child.get('/:id', (ctx, next) => {
  ctx.body = `我是${ctx.url}`;
});

// 将子路由挂在到父路由的指定路径（/parent/:id/child）下
parent.use('/parent/:id/child', child.routes(), child.allowedMethods());

// responds to "/parent/123/child" and "/parent/123/child/123"
app
  .use(parent.routes())
  .use(parent.allowedMethods());


app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```
## 路由前缀
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
// 添加路由前缀
const router = new Router({
  prefix: '/users'
});

// 访问地址为"/users"
router.get('/', (ctx, next) => {
  ctx.body = ctx.url;
});
// 访问地址为"/users/:id"
router.get('/:id', (ctx, next) => {
  ctx.body = ctx.url;
});

app
  .use(router.routes())
  .use(router.allowedMethods());


app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```
## 路径参数
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

// 路径参数
router.get('/:category/:title', (ctx, next) => {
  console.log(ctx.params);
  ctx.body = `{ category: '${ctx.params.category}', title: '${ctx.params.title}' }`
  // => { category: 'programming', title: 'how-to-node' }
});

app
  .use(router.routes())
  .use(router.allowedMethods());


app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```
## 路由重定向
```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

// 路由重定向
router
  .get('/sign-in', ctx => {
    ctx.body = ctx.url;
  })
  .all('/login', ctx => {
    console.log('重定向');
    ctx.redirect('/sign-in');
    ctx.status = 301;
  });

app
  .use(router.routes())
  .use(router.allowedMethods());


app.listen(8080, '0.0.0.0', () => {
  console.log('服务器运行在http://localhost:8080');
});
```
