# MVVM和MVC有什么区别？

模型—视图—视图模型（Model-View-ViewModel，MVVM），本质上是MVC（模型—视图—控制器）的改进版，其最重要的特性是数据绑定（data binding）。

## MVC

模型—视图—控制器（Model-View-Controller，MVC）模式

* View视图层是用户能给个看到并进行交互的客户端界面，如桌面应用的图形界面、浏览器端渲染的网页等
* Model指业务模型，用于计算、校验、处理和提供数据，但不直接于用户产生交互
* Controller控制器则负责收集用户输入的数据，向相关模型请求数据并返回相应的视图来完成交互请求

MVC模式实现了M和V的代码分离,M专注于数据，V专注于表达，C则在M和V之间架起了一座桥梁。

## MVVM

MVVM的主要目的是分离视图（View）和模型（Model），ViewModel层封装了界面展示和操作的属性和接口。通过数据绑定，我们可以将View和ViewModel关联在一起，当ViewModel中的数据发生变化时，View也会同步进行更新。

MVVM模式解耦了视图和模型。在模式中，每一个视图都有对应的一个ViewModel，同时ViewModel与模型建立联系。当接收到用户请求后，ViewModel获取模型响应数据，并通过数据绑定将相应的视图页面重新渲染。模型层的数据只需要传入ViewModel即可实现视图的同步更新，从而实现了视图和模型之间的松散耦合。