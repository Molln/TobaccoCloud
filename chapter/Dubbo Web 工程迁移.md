# Dubbo WEB 工程迁移

迁移至新框架 Service 工程

## 一 POM 文件

### 1 `<properties></properties>` 标签部分

`<properties>` 标签主要用来定义 Jar 版本，新框架中所有的 Jar 包版本在 `tobaccocloud-parent` 工程中定义，其它工程**不允许**定义 Jar 包版本。

### 2 其它部分

迁移至新框架 Service 工程即可。

## 二 src/main/resources 文件夹

迁移至新框架 Service 工程 src/main/resources 文件夹

* application.yml

* bootstrap.yml

  zookeeper 相关的配置要删除，替换为 Nacos，Nacos 配置可参考 demo 工程。

## 三 src/main/java 文件夹

迁移至新框架 Service 工程 src/main/java 文件夹

### 1 controller 包

由于不再使用 Dubbo，所以 @Reference 应替换成 @Autowired、@Resource 等标签。且注入的类不再是 API 工程中的接口，而是同工程下具体的实现类。
