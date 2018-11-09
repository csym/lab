# express新建项目流程

## 新建流程

### 安装express-generator
```
npm install express-generator -g
```

### 使用express新建项目
```
express ccnaserver
```

### 测试运行
```
cd ccnaserver
cnpm i
cnpm run start
```

### 安装热启动
```
cnpm i nodemon --save-dev
```
> 修改启动脚本

```
"start": "nodemon ./bin/www"
```

### 增加eslint支持
```
cnpm i eslint --save-dev
cnpm i eslint-config-airbnb-base --save-dev
cnpm i eslint-plugin-import --save-dev

```
> 根目录下增加.eslintrc，内容如下
```
{
    "extends": "airbnb-base",
    "rules": {
        "no-use-before-define": [0],
        "consistent-return": [0],
        "linebreak-style": [0]
    }
}
```
> vscode 重新打开工程，修复错误；
> 手工修复
```
./node_modules/eslint/bin/eslint.js --fix app.js
```
> 命令修复package.json scripts 中增加如下命令
```
"lint": "eslint --ext .js bin config constant dao routes util",
"lint:fix": "eslint --fix --ext .js bin config constant dao routes util"
```

### 增加babel 支持import等
```
cnpm i babel-cli --save-dev
cnpm i babel-preset-env --save-dev
cnpm i babel-plugin-transform-object-rest-spread --save-dev
```
> 增加.babelrc，内容如下:
```
{
    "presets": [
      ["env", {
        "targets": {
          "node": "current"
        }
      }]
    ],
    "plugins": ["transform-object-rest-spread"]
  }
```

> 修改启动脚本
```
"start": "nodemon ./bin/www --exec babel-node",
```

linux下使用如上命令报错，可以查看是否确实包kexec
```
cnpm i kexec 
```


### git配置
> git 初始化
> git 配置

### commit 前eslint检查
```
npm install -D husky
npm install -D lint-staged
```
> lint-staged 每次提交只检查本次提交所修改的文件
> package.json中修改如下：
```
"precommit": "lint-staged"
"lint-staged": {
    "*.js": "eslint --ext .js"
  }
```

### 根据数据表自动生成models

```
cnpm i mysql --D
cnpm i sequelize-auto -D
```

> package.json 中增加启动项,eg

```
"models": "sequelize-auto -o pojo -d showroom -h IP地址 -u root -p 3306 -x 密码 -e mysql",
    "models:fix": "eslint --fix --ext .js pojo",
```

### 增加日志

> 安装
```
cnpm install log4js --save

```

> 配置文件 config/log4js.js
```
// Log4js配置
module.exports = {
  appenders: {
    ccnaserver: {
      type: 'dateFile',
      filename: './logs/ccnaserver',
      pattern: '-yyyy-MM-dd-hh.log',
    },
    console: {
      type: 'console',
    },
  },
  categories: {
    default: {
      appenders: ['ccnaserver', 'console'],
      level: 'all',
    },
  },
};
```

> 使用方式 ./bin/www中增加
```
/**
 * make a log directory, just in case it isn't there.
 */
try {
  fs.mkdirSync('./logs');
} catch (e) {
  if (e.code !== 'EEXIST') {
    console.error('Could not set up log directory, error was: ', e);
    process.exit(1);
  }
}

/**
 * Initialise log4js first, so we don't miss any log messages
 */
const log4js = require('log4js');
const log4jsConf = require('../config/log4js');

log4js.configure(log4jsConf);

const log = log4js.getLogger('startup');
log.info('start');
```

> app.js 中增加所有http请求的日志
```
app.use(log4js.connectLogger(log4js.getLogger('http'), { level: log4js.levels.INFO }));
```

### body-parser
> 安装
cnpm i body-parser -D

> 使用
```
const bodyParser = require('body-parser');

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```

### 安装mysql和Sequelize

> 安装
```
cnpm install --save sequelize
cnpm install --save mysql2
```

> 使用
参考sequelize 语法总结；

### 安装password jwt相关
> 安装
```
cnpm i lodash --save
cnpm i jsonwebtoken --save
cnpm i passport --save
cnpm i passport-jwt --save
```

> 使用
参考[express 使用password](https://jonathanmh.com/express-passport-json-web-token-jwt-authentication-beginners/)


