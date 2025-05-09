# MVVM框架

MVVM拆分解释:  Model：负责数据存储  View： 负责页面展示  View Model：负责业务逻辑处理（比如Ajax请求等），对数据进行加工后交给视图展示

为什么要使用 MVVM

* 低耦合    视图可以独立于Model变化和修改，一个ViewModel可以绑定到不同的"View"上，当View变化的 Model可以不变，当Model变化的时候View也可以不变。

* 可重用性   可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑 独立开发。开发人员可以专注于业务逻辑和数据的开发,设计人员可以专注于页面设计。

![MVVMimg](..\..\..\resources\image\MVVMimg.png)