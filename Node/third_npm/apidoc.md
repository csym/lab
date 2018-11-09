# apidoc实践

## 安装及配置

### 安装
> cnpm i apidoc -g

### 配置

> package.json 增加配置

```
"scripts": {
    ....
    "apidoc": "apidoc -i routes/ -o public/apidoc"
  },
"apidoc": {
    "title": "ccnaserver apiDoc",
    "url" : "http://localhost:3001"
 }
  
```

> 也可以用apidoc.json
apidoc.json  中重要字段
name
version
description
"url": "http://localhost:3333",
  "sampleUrl": "http://localhost:3333",     增加测试接口

## 语法说明
```

@api   {get} /user/:id Request User information
@apiDefine  定义一个公用的块
@apiDescription api描述
@apiError  api错误，错误名称，错误描述
@apiErrorExample api错误例子，描述，结果json串，黑底白字
@apiExample api例子
@apiGroup  api 所属的组，大标题
@apiHeader api消息头 同apiParam
@apiHeaderExample api消息头例子
@apiIgnore 忽略没有完成的接口
@apiName   api 名字
@apiParam  api 参数，参数名，字段类型，描述
@apiParamExample api 参数例子
@apiPermission  权限提示
@apiSampleRequest  
@apiSuccess   api 返回结果 ，参数名，字段类型，描述
@apiSuccessExample  api 成功例子，描述，结果json串，黑底白字
@apiUse   调用一个已经定义的公用的块名
@apiVersion
```

## 实践过程

### get 事例

```
/**
 * @api {get} /api/v1/phone 获取号码信息列表
 * @apiDescription 获取号码信息列表
 * @apiName phone
 * @apiGroup Phone
 * @apiSuccessExample {json} Success-Response:
 * {
 *    "status": 0,
 *    "message": "获取信息成功",
 *    "data": {}
 * }
 * @apiSuccess {number} status 状态码
 * @apiSuccess {string} message 结果
 * @apiSuccess {Object[]} data 信息列表

 * @apiSampleRequest http://localhost:3001/api/v1/phone
 * @apiVersion 0.1.0
 */

/* GET phone list. */
router.get('/', async (req, res) => {
  // res.send('respond with a resource');
  log.debug(`phone bodyinfo ${JSON.stringify(req.body)}`);
  log.debug(`phone queryinfo ${JSON.stringify(req.query)}`);
  try {
    const list = await phoneDao.getPhoneList(req.query);
    const result = {};
    result.status = 0;
    result.message = '获取号码列表成功';
    result.data = list;
    log.debug(`Phone queryinfo suc ${JSON.stringify(result)}`);
    res.json(rightObj(result));
    operationLogDao.insert(req, SUCCESS);
  } catch (e) {
    const result = {};
    result.result = 1;
    result.message = JSON.stringify(e);
    log.debug(`Phone queryinfo failed ${JSON.stringify(result)}`);
    res.json(errObj(1, '获取号码列表异常'));
    operationLogDao.insert(req, FAILURE);
  }
});
```


### post 实例

```
/**
 * @api {post} /api/v1/staff/updateStafforder 更新订单信息
 * @apiDescription 更新订单信息
 * @apiName updateStafforder
 * @apiGroup Staff
 * @apiParam {string} orderid 订单ID
 * @apiParam {string} staffid 员工ID
 * @apiParam {string} username 员工名称
 * @apiParam {string} dealstatus 处理状态
 * @apiParam {string} dealcontent 处理内容
 * @apiSuccess {number} status 状态码
 * @apiSuccess {string} message 结果
 * @apiSuccess {json} data 信息
 * @apiSuccessExample {json} Success-Response:
 * {
 *    "status": 0,
 *    "message": "更新订单信息成功",
 *    "data": {}
 * }
 * @apiErrorExample {json} Error-Response:
 * {
 *    "status": 1,
 *    "message": "更新订单信息失败",
 *    "data": {}
 * }
 * @apiSampleRequest http://localhost:3001/api/v1/staff/updateStafforder
 * @apiVersion 0.1.0
 */

/* update order updateStafforder . */
router.post('/updateStafforder', async (req, res) => {
  // res.send('respond with a resource');
  log.debug(`staff updateStafforder ${JSON.stringify(req.body)}`);
  try {
    await faultOrderDao.createDealRecord(req.body);
    const result = await faultOrderDao.modifyDealStatus(req.body);
    res.json((result));
    operationLogDao.insert(req, SUCCESS);
  } catch (e) {
    const result = {};
    result.status = 1;
    result.message = JSON.stringify(e);
    log.error(`staff post error ${JSON.stringify(e)}`);
    operationLogDao.insert(req, FAILURE);
  }
});
```

## 生成接口文档
> 会在指定目录下生成接口文档
```
cnpm run apidoc 
"apidoc": "apidoc -i routes/ -o public/apidoc"
```

## 查看
> 访问接口网址 http://localhost:3001/apidoc

## 遇到的问题
### eslint 校验不通过
```
你可以通过在项目根目录创建一个 .eslintignore 文件告诉 ESLint 去忽略特定的文件和目录。.eslintignore 文件是一个纯文本文件，其中的每一行都是一个 glob 模式表明哪些路径应该忽略检测。例如，以下将忽略所有的 JavaScript 文件：

**/*.js

当 ESLint 运行时，在确定哪些文件要检测之前，它会在当前工作目录中查找一个 .eslintignore 文件。如果发现了这个文件，当遍历目录时，将会应用这些偏好设置。一次只有一个 .eslintignore 文件会被使用，所以，不是当前工作目录下的 .eslintignore 文件将不会被用到。
```

## 参考资料
- [ESLint规则整理与实际应用](https://blog.csdn.net/lhb_11/article/details/77962933)

- [apidoc](http://apidocjs.com/#getting-started)
- [NodeJs - Express项目 自动生成API文档](https://www.jianshu.com/p/7e1b057b047c/)
- [VS Code 列编辑功能说明](https://blog.csdn.net/u011127019/article/details/74039598)