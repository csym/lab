# log4j代码片段

## 配置文件

> cat log4js.json
```
{
  "appenders": {
    "file": {
      "type": "file",
      "filename": "logs/dataReceiver.log",
      "maxLogSize": 10485760,
      "numBackups": 5,
      "compress": true,
      "encoding": "utf-8",
      "mode": "0o0640",
      "flags": "w+"
    },
    "dateFile": {
      "type": "dateFile",
      "filename": "logs/more-dataReceiver.log",
      "pattern": "yyyy-MM-dd-hh",
      "compress": true
    },
    "out": {
      "type": "stdout"
    }
  },
  "categories": {
    "default": { "appenders": ["file", "dateFile", "out"], "level": "trace" }
  }
}

```

## 使用方式
> 读取配置
```
/**
 * Initialise log4js first, so we don't miss any log messages
 */
let log4js = require('log4js');
log4js.configure('./config/log4js.json', { reloadSecs: 300 });

let log = log4js.getLogger("startup");
```
> 调用方式
```
let log4js = require('log4js');
let log = log4js.getLogger("startup");
logger.info("xmlStr = "+xmlStr);
```

## 参考文档

- [log4j](https://www.npmjs.com/package/log4j)