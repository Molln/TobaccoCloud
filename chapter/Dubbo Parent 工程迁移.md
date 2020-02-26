# Dubbo Parent 工程迁移

迁移至新框架父级工程

Dubbo 中的 Parent 工程与 Spring Cloud 父级工程作用一致，均起到**聚合子模块**作用，且不含有业务代码。仅有 pom.xml 文件，下方介绍如何迁移 pom.xml 文件中的内容

## 1 `<properties></properties>` 标签部分

`<properties>` 标签主要用来定义 Jar 版本，新框架中所有的 Jar 包版本在 `tobaccocloud-parent` 工程中定义，其它工程**不允许**定义 Jar 包版本。

* 同一个 Jar 包版本冲突，建议留存高版本 Jar 包
* 新框架父级工程 pom.xml 文件中不应出现此标签

## 2 `<dependencies></dependencies>` 标签部分

`<dependencies>` 标签主要用于引入 Jar 依赖，新框架中 `tobaccocloud-framework` 工程定义了部分依赖，如果 framework 工程中没有引入待迁移的依赖，则需要进行迁移。

- 原框架 API 工程需要用到的 Jar 包，迁移至新框架 API 工程
- 其它依赖迁移至 新框架 Service 工程
- 新框架父级工程 pom.xml 文件中不应出现此标签

## 3 `<dependencyManagement></dependencyManagement>` 标签部分

`<dependencyManagement>` 标签主要用于管理依赖及版本，建议转换成 `<dependencies>` 进行迁移。

* 新框架父级工程 pom.xml 文件中不应出现此标签