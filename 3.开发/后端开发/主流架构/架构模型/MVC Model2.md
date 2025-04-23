# MVC框架

MVC拆分解释：  视图（View）：用户界面。  控制器（Controller）：业务逻辑  模型（Model）：数据保存

MVC通信方式  View 传送指令到 Controller  Controller 完成业务逻辑后，要求 Model 改变状态  Model 将新的数据发送到 View，用户得到反馈

MVC局限性  View里会包含业务逻辑  View当中的业务逻辑无法重用  模型的代码少，控制器的代码却是越写越多