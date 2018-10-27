# nodemon配置文件知识整理

```javascript
{
  "restartable": "rs", // 重启的命令，默认是rs
  "ignore": [""], // 忽略的文件后缀名或者文件夹，文件的书写路径相对于nodemon.json的所在位置
  "verbose": true, // 表示输出详细启动与重启信息
  "execMap": { // 设置运行服务的后缀名与对应的命令
    "js": "node --harmony" // 表示使用nodemon代替node运行js服务
  },
  "ext": "js json", // 监控指定后缀名的文件，用空格间隔。
  "watch": [ // 监控的目录数组
    "./src/**"
  ],
  "env": {
    "NODE_ENV": "env",
    "PORT": "3000"
  },
  "legacy-watch": false // nodemon 使用 Chokidar 作为底层监控系统，但是如果监控失效，或者提示没有需要监控的文件时，就需要使用轮询模式（polling mode），即设置 legacy-watch 为 true，也可以在命令行中指定
}
```
