# HTTP 请求

## 一 返回类设计

**注意：**由于涉及迁移代码工作量的原因，Controller 层直接返回业务数据，而不再使用 Result 类进行封装。本类仅供学习了解。

Controller 层方法返回结果统一使用 Result.java 进行封装。该类位于

````
com.neusoft.tobaccocloud.framework.base.entity.Result
````

该类主要结构

````java
public class Result {
	private boolean success = true;
	private int code;
	private String message;
	private Object data = new Object();
````

* succes

  请求是否被成功处理。取值 true 或 false。

* code

  返回结果码。结果码在 `com.neusoft.tobaccocloud.framework.base.http.ResultCode` 类中定义。

* message

  返回处理消息

* data

  返回数据对象

## 二 JSON 解析配置

SpringBoot JSON 工具包默认是 Jackson，只需要引入 spring-boot-starter-web 依赖包，自动引入相应依赖包，相较于 Alibaba FastJson 存在一些缺陷，此处建议使用 FastJSON 替换 Jackson。

在 tobaccocloud-framework 项目中，已经添加了一个简单的 FastJSON 配置类。如无特殊要求，可直接使用该配置类。该配置类位于

`com.neusoft.tobaccocloud.framework.base.config.FastJsonConfiguration`

## 三 响应设计

前台发送 HTTP 请求后，根据 响应头中的 **Status Code** 进行判断本次请求结果，若为 200，则成功，否则失败。

**后台抛出异常如何响应？**

如果后台抛出异常，则响应头中  **Status Code** 为 400，响应体中包含错误信息，结构如下

```json
{
  "exception": "com.neusoft.tobaccocloud.framework.base.exception.BaseServiceException",
  "help": "",
  "localDateTimeString": "2020-02-20T10:31:51.582",
  "message": "手动抛出",
  "path": "/demo/base/selectAllData"
}
```

**参数说明**

| 表头                | 表头     |
| ------------------- | -------- |
| exception           | 异常类型 |
| help                | 帮助信息 |
| localDateTimeString | 时间     |
| message             | 错误信息 |
| path                | 请求路径 |