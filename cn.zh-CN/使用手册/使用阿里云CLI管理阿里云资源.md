# 使用阿里云CLI管理阿里云资源 {#concept_90398_zh .concept}

云命令行\(Cloud Shell\)中预装了阿里云CLI，阿里云CLI是基于阿里云开放 API 建立的管理工具。您可以通过阿里云CLI管理阿里云资源。

阿里云云产品的API分为RPC和RESTful两种类型，大部分产品提供RPC API，例如云服务器ECS，云数据库RDS和负载均衡等。

不同类型的API的调用方法也不同。您可以通过以下特点判断API类型：

-   API参数中包含Action字段的是RPC API，需要PathPattern参数的是RESTful API。
-   一般情况下，一个云产品的API类型是一致的。

## 启动Cloud Shell {#section_ezs_yy5_lgb .section}

选择一种方式启动云命令行：

-   在控制台中运行

    单击[控制台首页](https://home.console.aliyun.com/new?cloudshell=true)头部导航的命令行按钮，启动云命令行。

-   独立运行

    在浏览器中输入[https://shell.aliyun.com](https://shell.aliyun.com)或在 [OpenAPI Explorer](https://pre-api.aliyun.com/new#/cli) 中打开云命令行操作界面。

    您可以根据实际需要打开多个命令行窗口，但最多可同时打开5个云命令行窗口。


在启动云命令时，请注意：

-   第一次连接云命令行时会为您创建虚拟机，会消耗一些时间，最长不超过40秒。

-   打开多个云命令行窗口时，所有窗口都会连接到同一台虚拟机。虚拟机数量不会因为您打开新的命令行窗口而增加。


## 在Cloud Shell中调用RPC API {#rpc .section}

在云命令中调用RPC API需遵循以下格式：

```
aliyun <ProductCode> <ActionName> [--parameter1 value1 --paramter2 value2]
```

其中：

-   ProductCode：要调用的云产品 code，比如云服务器的产品code为`ecs`，负载均衡的产品code 为`slb`。您可以执行`aliyun --help`命令查看产品code。
-   ActionName：要调用的 API。比如使用 ECS 的 `DescribeInstanceAttribute` 接口查看一个 ECS 实例的详细信息。
-   parameter：要传入的请求参数。具体参见各产品的API文档。

**示例**

在Cloud Shell中执行以下命令，查看指定ECS实例的配置信息。

**说明：** 运行前，请替换实例ID。

```
aliyun ecs DescribeInstanceAttribute --InstanceId i-bp198exxxxxx | jq 
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/92351/155774012136711_zh-CN.png)

## 在Cloud Shell中调用RESTful API {#section_uhy_4cc_kgb .section}

部分阿里云产品如容器服务是RESTful API。RESTful API与RPC API的调用方式不同。使用阿里云CLI调用RESTful API的基本结构如下：

-   **GET请求** 

    ```
    aliyun <ProductCode> GET /<Resource>
    ```

    **示例**

    ```
    aliyun cs GET /clusters
    ```

-   **POST请求** 

    ```
    aliyun <ProductCode> POST /<Resource> --body "$(cat input.json)"
    ```

    **示例**

    ```
    aliyun cs POST /clusters//attach --header "Content-Type=application/json" --body "$(cat attach.json)"
    ```

-   **DELETE请求** 

    ```
    aliyun <ProductCode> DELETE /<Resource>
    ```

    **示例**

    ```
    aliyun cs DELETE /clusters/<cluster-id>
    ```


## 相关文档 {#section_v5n_ffc_kgb .section}

-   [使用阿里云CLI](https://help.aliyun.com/document_detail/90767.html)
-   [CLI参数格式说明](../cn.zh-CN/.md#)

