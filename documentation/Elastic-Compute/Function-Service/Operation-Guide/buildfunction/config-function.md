# 配置函数

在创建函数时，除代码及任意关联依赖项外，还应对函数属性信息进行配置。Function提供控制台及API用来配置函数属性。函数配置信息如下：

 

**name**（必选）：函数名称。在当前服务内唯一，并符合如下约束：

   1. 只包含字母、数字、下划线和中划线
   2. 不能以数字、中划线开头
   3. 大小写敏感
   4. 长度为 1-128 字符
                         
**description**（可选）：函数的描述。面向用户提供标识、记录的属性，您可以对您的函数进行简单的描述用以标注区分。 

**entrance**（必选）： 处理函数，它是Function运行用户函数的调用入口。

**memory**（可选）：函数运行所需的内存资源，单位为 MB。取值范围为 [128, 1024]，步长为128 MB。

**runTime**（必选）：函数运行时类型。

**overTime**（可选）： 函数的最大运行时间，单位为秒。


**code**（必选）：代码包。必须是zip类型，本地直接上传。


**除以上内容，高级配置中还可配置以下内容：**

**environment**（可选）：在配置中定义的环境变量可在函数运行时从环境中获取到。

**vpcId**（可选）：配置 VPC 私有网络。

**subnetId**（可选）：配置子网，已配置VPC和子网的函数将置于配置的私有网络中运行。

公网（无需配置）：默认函数开启公网访问。

**logSetId**（可选）：指定日志集。

**logTopicId**（可选）：指定日志主题。

**说明**：除函数名称外，其他属性均可后续修改。
