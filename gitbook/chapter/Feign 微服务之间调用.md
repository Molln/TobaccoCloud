# Feign 微服务之间调用

## 一 介绍

官网解释：

<http://projects.spring.io/spring-cloud/spring-cloud.html#spring-cloud-feign>

Feign 是一个声明式 WebService 客户端。使用 Feign 能让编写 Web Service 客户端更加简单, 它的使用方法是定义一个接口，然后在上面添加注解，同时也支持 JAX-RS 标准的注解。Feign 也支持可拔插式的编码器和解码器。Spring Cloud对Feign进行了封装，使其支持了 Spring MVC 标准注解和 HttpMessageConverters 。Feign 可以与 Eureka 和 Ribbon 组合使用以支持负载均衡。

## 二 开发步骤

### 1 服务端开发

服务端正常开发 Controller，Service 等组件，确保正常调用无问题

### 2 Feign 客户端开发

Feign 客户端建议放置在服务提供方的 API 工程模块中，以便其它微服务调用。

#### （1）引入 Feign 相关依赖

```xml
<!-- Spring Cloud OpenFeign -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

此依赖已在 tobaccocloud-framework 工程中引入，依赖 framework 则不需要重复引入

#### （2）客户端开发

示例代码：

````java
package com.neusoft.file.feign.client;

import java.util.List;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestParam;

import com.neusoft.framework.base.entity.FileUploadInfo;
import com.neusoft.framework.base.entity.Result;

/**
 * @Description 文件微服务 Feign 客户端
 * @author 		guoteng  Email:guoteng@neusoft.com
 * @date 		2019年12月12日上午9:57:32
 */
@FeignClient(name = "file-service") //name 指定服务提供方在 Nacos 注册的名称，即 spring.application.name
public interface FileClientService {
	
	@PostMapping(value = "/file/uploadFileByModel")
	public Result uploadFileByModel(@RequestBody FileUploadInfo fileUploadInfo);
	
	@PostMapping(value = "/file/uploadFileByModels")
	public Result uploadFileByModels(@RequestBody List<FileUploadInfo> fileUploadInfos);
	
	@DeleteMapping(value = "/file/deleteFile")
	public Result deleteFile(@RequestParam(name = "fileUrl") String fileUrl);
}
````

### 3 调用端开发

#### （1）引入 Feign 相关依赖

````xml
<!-- Spring Cloud OpenFeign -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
````

此依赖已在 tobaccocloud-framework 工程中引入，依赖 framework 则不需要重复引入

#### （2）启动类开启 Feign

````java
//basePackages 配置 Feign 客户端所在的包
@EnableFeignClients(basePackages= {"com.neusoft.**.feign.client"})
````

#### （3）调用

````java
public class FileHandleAspect {
	
	@Autowired
	private FileClientService fileClientService;
    
    public void demo(){
        fileClientService.deleteFile("demo.txt");
    }
}
````

