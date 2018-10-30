# CSS Modules

## CSS Modules
> 产生局部作用域的唯一方法，就是使用一个独一无二的class的名字
- 所有样式都是 local 的，解决了命名冲突和全局污染问题
- class 名生成规则配置灵活，可以此来压缩 class 名
- 只需引用组件的 JS 就能搞定组件所有的 JS 和 CSS
- 依然是 CSS，几乎 0 学习成本

### 局部作用域

> 编译前
```
<h1 className={style.title}>
    Hello World
</h1>
```

> 编译后
```
<h1 class="_3zyde4l1yATCOkgn-DBWEL">
  Hello World
</h1>
```

> webpack配置参考
```
{
    test: /\.css$/,
    loader: "style-loader!css-loader?modules"
},
```

> 定制哈希类名
```
{
  test: /\.css$/,
  loader: "style-loader!css-loader?modules&localIdentName=[path][name]---[local]---[hash:base64:5]"
},
```

### 全局作用域
> 使用:global(.className)，classname 不会被编译成哈希字符串
```
.projectList {
  :global {
    .ant-card-meta-description {
      color: @text-color-secondary;
      height: 44px;
      line-height: 22px;
      overflow: hidden;
    }
  }
  .cardTitle {
    font-size: 0;
    a {
      color: @heading-color;
      margin-left: 12px;
      line-height: 24px;
      height: 24px;
      display: inline-block;
      vertical-align: top;
      font-size: @font-size-base;
      &:hover {
        color: @primary-color;
      }
    }
  }
}
```

### Class 的组合
> 在 CSS Modules 中，一个选择器可以继承另一个选择器的规则，这称为"组合"（"composition"）

```
.className {
  background-color: blue;
}

.title {
  composes: className;
  color: red;
}
```
> 继承的内容，可以来源于其他文件
```
.title {
  composes: className from './another.css';
  color: red;
}
```

> 编译后
```
<button class="button--base-daf62 button--normal-abc53">Submit</button>
```

### 支持变量
> CSS Modules 支持使用变量，不过需要安装 PostCSS 和 postcss-modules-values。
```
npm install --save postcss-loader postcss-modules-values
```

> webpack 文件修改
```
{
test: /\.css$/,
loader: "style-loader!css-loader?modules!postcss-loader"
},
```

> **变量例子**

> colors.css

```
@value blue: #0c77f8;
@value red: #ff0000;
@value green: #aaf200;
```
> cat App.css
```
@value colors: "./colors.css";
@value blue, red, green from colors;

.title {
  color: red;
  background-color: blue;
}
```

## 使用经验(来源于网络)
### class 命名技巧
> CSS Modules 的命名规范是从 BEM 扩展而来。BEM 把样式名分为 3 个级别，分别是：

- Block：对应模块名，如 Dialog
- Element：对应模块中的节点名 Confirm Button
- Modifier：对应节点相关的状态，如 disabled、highlight

> BEM 最终得到的 class 名为 dialog__confirm-button--highlight。使用双符号 __ 和 -- 是为了和区块内单词间的分隔符区分开来

### CSS Modules 历史遗留项目实践
> 当生成混淆的 class 名后，可以解决命名冲突，但因为无法预知最终 class 名，不能通过一般选择器覆盖。我们现在项目中的实践是可以给组件关键节点加上 data-role 属性，然后通过属性选择器来覆盖样式

> dialog.js
```
  return <div className={styles.root} data-role='dialog-root'>
      <a className={styles.disabledConfirm} data-role='dialog-confirm-btn'>Confirm</a>
      ...
  </div>
```
> dialog.css
```
[data-role="dialog-root"] {
  // override style
}
```

### 如何与全局样式共存
> 前端项目不可避免会引入 normalize.css 或其它一类全局 css 文件。使用 Webpack 可以让全局样式和 CSS Modules 的局部样式和谐共存。下面是我们项目中使用的 webpack 部分配置代码：

```
module: {
  loaders: [{
    test: /\.jsx?$/,
    loader: 'babel'
  }, {
    test: /\.scss$/,
    exclude: path.resolve(__dirname, 'src/styles'),
    loader: 'style!css?modules&localIdentName=[name]__[local]!sass?sourceMap=true'
  }, {
    test: /\.scss$/,
    include: path.resolve(__dirname, 'src/styles'),
    loader: 'style!css!sass?sourceMap=true'
  }]
}
/* src/app.js */
import './styles/app.scss';
import Component from './view/Component'

/* src/views/Component.js */
// 以下为组件相关样式
import './Component.scss';
目录结构如下：

src
├── app.js
├── styles
│   ├── app.scss
│   └── normalize.scss
└── views
    ├── Component.js
    └── Component.scss
```
> 注意如上配置loader中的include, exclude;

## 参考文档

- [阮一峰CSS Modules 用法教程](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)
- [CSS Modules 详解及 React 中实践](https://github.com/camsong/blog/issues/5)
- [github/css-modules](https://github.com/css-modules/css-modules)
