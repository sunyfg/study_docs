# DllPlugin、HardSourceWebpackPlugin

还是针对第三方库模块。项目中引入了很多第三方库，这些库在很长的一段时间内，都是不会更新的。那么我们打包的时候可以分开打包来提升打包速度。只需要编译一次，编译完成后存在指定的文件（这里可以称为动态链接库）中。在之后的构建过程中不会再对这些模块进行编译，而是直接使用对应的插件来引用动态链接库的代码。因此可以大大提高构建速度。

**本质上就是做缓存嘛！**

这样的工作，之前是 DllPlugin 与 DllReferencePlugin 合力完成的。但是整个[配置过程](https://juejin.cn/post/6844903777296728072)较为繁琐（读者若有经历过，可知。且 React Vue 都已经摒弃了 DLL 的方式）。现在有了一个新的插件叫：HardSourceWebpackPlugin。它可以完成与 DllPlugin 、 DllReferencePlugin 同样的工作，但是配置的过程极为简单：

```js
//webpack.config.js
//引用
const HardSourceWebpackPlugin = require('hard-source-webpack-plugin');

//使用
new HardSourceWebpackPlugin()
```

