# babel

babel默认只转换新的 JavaScript 语法，比如箭头函数、扩展运算（spread）

不转换新的 API，例如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象；

以及一些定义在全局对象上的方法（比如 Object.assign）都不会转译。如果想使用这些新的对象和方法，则需要为当前环境提供一个垫片（polyfill）；

## babel-polyfill

通过改写全局prototype的方式实现，它会加载整个polyfill，针对编译的代码中新的API进行处理，并且在代码中插入一些帮助函数，比较适合单独运行的项目。

缺点：

babel-polyfill解决了Babel不转换新API的问题，但是直接在代码中插入帮助函数，会导致污染了全局环境，并且不同的代码文件中包含重复的代码，导致编译后的代码体积变大。

## babel-runtime

Babel为了解决上述问题，提供了单独的包babel-runtime用以提供编译模块的工具函数，启用插件babel-plugin-transform-runtime后，Babel就会使用babel-runtime下的工具函数。

babel-runtime插件能够将这些工具函数的代码转换成require语句，指向为对babel-runtime的引用。每当要转译一个api时都要手动加上require('babel-runtime')。简单说 babel-runtime 更像是一种按需加载的实现。

比如你哪里需要使用 Promise，只要在这个文件头部 require Promise from 'babel-runtime/core-js/promise'就行了

## babel-plugin-transform-runtime

为了方便使用 babel-runtime，解决手动 require 的苦恼。它会分析我们的 ast 中，是否有引用 babel-rumtime 中的垫片（通过映射关系），如果有，就会在当前模块顶部插入我们需要的垫片。

transform-runtime 是利用 plugin 自动识别并替换代码中的新特性，你不需要再引入，只需要装好 babel-runtime 和 配好 plugin 就可以了。

好处是按需替换，检测到你需要哪个，就引入哪个 polyfill，如果只用了一部分，打包完的文件体积对比 babel-polyfill 会小很多。而且 transform-runtime 不会污染原生的对象，方法，也不会对其他 polyfill 产生影响。