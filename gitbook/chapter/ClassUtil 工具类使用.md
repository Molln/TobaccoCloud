# ClassUtil 工具类使用

使用 ClassUtil 工具类中如下方法时

````java
String name = ClassUtil.getPropertyName(DemoBase.class, DemoBase::getLabel);
````

出现错误

````java
java.lang.ClassCastException: com.neusoft.tobaccocloud.demo.base.entity.DemoBase$ByteBuddy$mUPGXxwx cannot be cast to com.neusoft.tobaccocloud.demo.base.entity.DemoBase
````

解决方案

关闭 Spring Boot 开发热部署

