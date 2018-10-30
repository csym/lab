# CSS Modules

## CSS Modules
> 产生局部作用域的唯一方法，就是使用一个独一无二的class的名字

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

## 参考文档

- [阮一峰CSS Modules 用法教程](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)
- [CSS Modules 详解及 React 中实践](https://github.com/camsong/blog/issues/5)
- [github/css-modules](https://github.com/css-modules/css-modules)
