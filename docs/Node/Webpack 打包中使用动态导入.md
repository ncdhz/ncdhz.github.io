# Webpack 打包中使用动态导入

首先把 `require` 换成

```js
const requireFunc = typeof __webpack_require__ === 'function' ? __non_webpack_require__ : require
```

这块代码的解释可以看

1. 原始 require 函数 [__webpack_require__](https://webpack.docschina.org/api/module-variables/#webpack_require-webpack-specific)

2. 生成一个不会被 webpack 解析的 require 函数 [__non_webpack_require__](https://webpack.docschina.org/api/module-variables/#non_webpack_require-webpack-specific)
