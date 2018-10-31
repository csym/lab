# umi2 创建mockServer

## 背景

> 使用antd pro2 创建项目，build后，需要连接后台服务；这里使用umi2 新建一项目，使用其mock功能，对外提供模拟数据接口；

## 创建过程

### 基础umi项目创建
> 创建项目

```
mkdir umiMockServer
cd umiMockServer
npm init
```

> 使用umi创建页面

```
umi g page index
umi g page user
```

> 修改package.json ,增加启动测试

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "umi dev"
  },
```

> 基本测试

```
cnpm run start
测试地址
http://localhost:8000/users 
```

## mock数据增加

### mockjs,monent安装

```
yarn add mockjs
yarn add moment
```

### mock数据增加
> 创建mock目录,增加user.js

```
// 代码中会兼容本地 service mock 以及部署站点的静态数据
export default {
  // 支持值为 Object 和 Array
  'GET /api/currentUser': {
    name: 'Serati Ma',
    avatar: 'https://gw.alipayobjects.com/zos/rmsportal/BiazfanxmamNRoxxVxka.png',
    userid: '00000001',
    email: 'antdesign@alipay.com',
    signature: '海纳百川，有容乃大',
    title: '交互专家',
    group: '蚂蚁金服－某某某事业群－某某平台部－某某技术部－UED',
    tags: [
      {
        key: '0',
        label: '很有想法的',
      },
      {
        key: '1',
        label: '专注设计',
      },
      {
        key: '2',
        label: '辣~',
      },
      {
        key: '3',
        label: '大长腿',
      },
      {
        key: '4',
        label: '川妹子',
      },
      {
        key: '5',
        label: '海纳百川',
      },
    ],
    notifyCount: 12,
    country: 'China',
    geographic: {
      province: {
        label: '浙江省',
        key: '330000',
      },
      city: {
        label: '杭州市',
        key: '330100',
      },
    },
    address: '西湖区工专路 77 号',
    phone: '0752-268888888',
  }
}

```

### api测试

```
cnpm run start

浏览器打开如下接口
http://localhost:8000/api/currentUser
```
> 测试antdpro时，可以直接将mock文件拷贝过来；


### 默认端口修改

```
# Or use cross-env for all platforms
$ yarn add cross-env --dev
$ cross-env PORT=8005 umi dev
```

## 参考文档
- [umi官方文档](https://umijs.org/zh/guide/getting-started.html)
- [umi .env 和环境变量](https://umijs.org/zh/guide/env-variables.html#%E5%A6%82%E4%BD%95%E9%85%8D%E7%BD%AE)
- [ant-pro官方文档](https://pro.ant.design/docs/getting-started-cn)
