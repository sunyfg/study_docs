# Hash、ContentHash、ChunkHash 之间的区别？

* hash值的生成和整个项目有关系：
  * 比如我们现在有两个入口index.js和main.js
  * 它们分别会输出到不同的bundle文件中，并且在文件名称中我们有使用hash
  * 这个时候，如果修改了index.js文件中的内容，那么hash会发生变化, 意味着两个文件的名称都会发生变化
* chunkhash可以有效的解决上面的问题，它会根据不同的入口进行解析来生成hash值：
  - 比如我们修改了index.js，那么main.js的chunkhash是不会发生改变的
* contenthash表示生成的文件hash名称，**只和内容有关系**：
  * 比如我们的index.js，引入了一个style.css，style.css有被抽取到一个独立的css文件中(使用MiniCssExtractPlugin)
  * 这个css文件在命名时，如果我们使用的是 chunkhash
  * 那么当index.js文件的内容发生变化时，css文件的命名也会发生变化
  * 这个时候使用contenthash可以解决这个问题