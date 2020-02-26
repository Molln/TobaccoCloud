# lombok

## 一 简介

Lombok能以简单的注解形式来简化java代码，提高开发人员的开发效率。例如开发中经常需要写的javabean，都需要花时间去添加相应的getter/setter，也许还要去写构造器、equals等方法，而且需要维护，当属性多时会出现大量的getter/setter方法，这些显得很冗长也没有太多技术含量，一旦修改属性，就容易出现忘记修改对应方法的失误。

Lombok能通过注解的方式，在编译时自动为属性生成构造器、getter/setter、equals、hashcode、toString方法。出现的神奇就是在源码中没有getter和setter方法，但是在编译生成的字节码文件中有getter和setter方法。这样就省去了手动重建这些代码的麻烦，使代码看起来更简洁些。

## 二 官网

https://projectlombok.org/download

## 三 引入

````xml
<!-- Lombok 插件：基于注解自动生成 set/get/构造器插件。（版本由 spring-boot-dependencies 仲裁） -->
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<scope>provided</scope>
</dependency>
````

## 四 使用

在 Entity 类上添加相关注解即可

* @Data

  会为类的所有属性自动生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。

* @NoArgsConstructor

  无参构造器

* @RequiredArgsConstructor

  部分参数构造器。Lombok没法实现多种参数构造器的重载。

* @AllArgsConstructor

  全参构造器。

* @Accessors

  Accessor的中文含义是存取器，@Accessors用于配置 getter 和 setter 方法的生成结果。

  <https://projectlombok.org/features/experimental/Accessors>

  * fluent

    fluent的中文含义是流畅的，设置为true，则getter和setter方法的方法名都是基础属性名，且setter方法返回当前对象。

    ```java
    @Data
    @Accessors(fluent = true)
    public class User {
        private Long id;
        private String name;
        
        // 生成的getter和setter方法如下，方法体略
        public Long id() {}
        public User id(Long id) {}
        public String name() {}
        public User name(String name) {}
    }
    ```

  * chain

    chain的中文含义是链式的，设置为true，则setter方法返回当前对象。默认是假，如果fluent=true，那么chain默认为真。

    ````java
    @Data
    @Accessors(chain = true)
    public class User {
        private Long id;
        private String name;
        
        // 生成的setter方法如下，方法体略
        public User setId(Long id) {}
        public User setName(String name) {}
    }
    ````

  * prefix

    prefix的中文含义是前缀，用于生成getter和setter方法的字段名会忽视指定前缀（遵守驼峰命名）。如下

    ````java
    @Data
    @Accessors(prefix = "p")
    class User {
    	private Long pId;
    	private String pName;
    	
    	// 生成的getter和setter方法如下，方法体略
    	public Long getId() {}
    	public void setId(Long id) {}
    	public String getName() {}
    	public void setName(String name) {}
    }
    ````

## 五 在 Eclipse 中使用 lombak

1. 双击 lombok-1.16.18.jar 选择 eclipse.exe 安装
2. 重启 eclipse
3. 重新构建项目