# 前端测试入门介绍

## 单元测试
- Mocha与chai
- enzyme
- sinon
- karma
- jest

## e2e端到端测试

> 端到端测试也叫冒烟测试，用于测试真实浏览器环境下前端应用的流程和表现，相当于代替人工去操作应用。

> puppeteer 作为 E2E 测试的工具，puppeteer 是 Google Chrome 团队官方的无界面（Headless）Chrome 工具。它默认使用 chrome / chromium 作为浏览器环境运行你的应用，并且提供了非常语义化的 API 来描述业务逻辑。

- [puppeteer](https://zhaoqize.github.io/puppeteer-api-zh_CN/#/)

> ant-pro2 中例子

```
import puppeteer from 'puppeteer';
import RouterConfig from '../../config/router.config';

const BASE_URL = `http://localhost:${process.env.PORT || 8000}`;

function formatter(data) {
  return data
    .reduce((pre, item) => {
      if (item.routes) {
        return pre.concat(formatter(item.routes));
      }
      pre.push(item.path);
      return pre;
    }, [])
    .filter(item => item);
}

describe('Homepage', () => {
  let browser;
  let page;

  const testAllPage = async layout =>
    new Promise(async (resolve, reject) => {
      const loadPage = async index => {
        const path = layout[index];
        console.log(`path: ${path}`);
        try {
          await page.goto(`${BASE_URL}${path}`, { waitUntil: 'networkidle2' });
          const haveFooter = await page.evaluate(
            () => document.getElementsByTagName('footer').length > 0
          );
          console.log(`index... ${index}.path:${path}`);
          expect(haveFooter).toBeTruthy();

          if (index < layout.length - 1) {
            loadPage(index + 1);
          } else {
            resolve('ok');
          }
        } catch (error) {
          reject(error);
        }
      };
      loadPage(0);
    });

  beforeAll(async () => {
    browser = await puppeteer.launch({ args: ['--no-sandbox'] });
    page = await browser.newPage();
    jest.setTimeout(1000000);
  });

  it('test user layout', async () => {
    const userLayout = formatter(RouterConfig[0].routes);
    await testAllPage(userLayout);
  });

  it('test base layout', async () => {
    const baseLayout = formatter(RouterConfig[1].routes);
    await testAllPage(baseLayout);
  });

  afterAll(() => browser.close());
});

```


## 参考文档
- [前端单元测试入门01_Mocha与chai](https://www.cnblogs.com/vvjiang/p/8579046.html)
- [前端单元测试入门02_react的单元测试之Enzyme](https://www.cnblogs.com/vvjiang/p/8599980.html)
- [前端单元测试入门03 Sinon](https://www.cnblogs.com/vvjiang/p/8607580.html)
- [前端单元测试入门04Karma](https://www.cnblogs.com/vvjiang/p/8615368.html)
- [前端单元测试入门05react的单元测试之jest](https://www.cnblogs.com/vvjiang/p/8620847.html)
- [enzyme](https://airbnb.io/enzyme/docs/api/index.html)
- [jest](https://jestjs.io/docs/en/getting-started)
- [ant-test](https://pro.ant.design/docs/ui-test-cn)
- [puppeteer](https://github.com/googlechrome/puppeteer)