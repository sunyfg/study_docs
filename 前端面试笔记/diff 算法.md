# diff 算法

## 概述

* diff 即对比，是一个广泛的概念，如 linux diff 命令，git diff 等
* 两个 js 对象也可以做 diff，如 https://github.com/cujojs/jiff
* 两棵树做 diff，如这里的 vdom diff

![image-20230518080846519](/Users/sunyanfeng/Library/Application Support/typora-user-images/image-20230518080846519.png)

## 树 diff 的时间复杂度 O(n^3)

* 第一、遍历 tree1；第二，遍历 tree2
* 第三，排序
* 1000 个节点，要计算 1亿次，算法不可用

## 优化时间复杂度到 O(n)

* 只比较同一层级，不跨级比较
* tag 不同，则直接删掉重建，不再深入比较
* tag 和 key，两者都相同，则认为是相同节点，不再深入比较

![image-20230518081339328](/Users/sunyanfeng/Library/Application Support/typora-user-images/image-20230518081339328.png)

![image-20230518081403406](/Users/sunyanfeng/Library/Application Support/typora-user-images/image-20230518081403406.png)