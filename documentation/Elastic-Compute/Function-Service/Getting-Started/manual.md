# 使用流程

本章简要介绍函数服务的使用流程。

使用简介：

公测期间，您如果想使用函数服务，请在产品详情页申请公测。

函数服务使用流程：

1. 创建函数，编写代码，设置执行条件，部署应用；

2. 创建触发器，调用函数；

3. 查看函数日志；

4. 查看函数监控。

 

## 创建函数

函数（Function）是调度与运行的基本单位，是一段代码的处理逻辑。您需要根据Function提供的函数接口形式编写代码，并将代码以函数的形式部署到函数服务。

函数创建详细信息请参阅[函数构建](../../Function-Service/Operation-Guide/buildfunction/function-overview.md)。

 
## 触发函数

函数由事件触发，函数服务目前支持OSS触发器和API网关触发器。

触发器详细信息请参阅[触发器管理](../../Function-Service/Operation-Guide/invokefunction/triggermanagement/triggeroverview.md)。

此外，如不配置触发器，您也可以使用控制台、SDK等方式直接调用函数执行。



## 查看执行日志

函数被执行后，您可以通过日志服务查询函数执行情况，Function将函数日志记录在日志服务中，您可以通过配置日志服务获取函数日志。

函数日志详细信息请参阅[函数日志](../../Function-Service/Operation-Guide/log.md)。



## 查看函数监控

通过云监控，用户可实时监控函数运行状况，保证业务平稳运行。

函数监控详细信息请参阅[函数监控](../../Function-Service/Operation-Guide/monitor.md)。
