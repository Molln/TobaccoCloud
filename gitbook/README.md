# Introduction

东软烟草 Spring Cloud 分布式微服务框架

## 一 SVN 地址

<http://esdsvn.neusoft.com:8888/cig/product/qomo/TobacooCloud>

## 二 框架主要技术 & 插件

* Spring Boot

* Spring Cloud

* Mybatis（通用 Mapper）

* MySQL

* FastDFS

* Nacos

* Feign

* Swagger

* Maven

* lombok

* Redis

  ......

## 三 框架结构

| 模块名                 | 说明                                                     |
| ---------------------- | -------------------------------------------------------- |
| tobaccocloud-parent    | 顶级工程：版本仲裁中心，管理所有 JAR 包依赖版本          |
| tobaccocloud-framework | 框架工程：常用工具类，基类，异常，通用配置类......       |
| tobaccocloud-generator | 代码生成器：1生成Maven Module；2基于表业务代码           |
| tobaccocloud-file      | 文件微服务：与 FastDFS 交互处理文件操作。                |
| tobaccocloud-demo      | 示例微服务：示例代码，包含普通业务，主细表业务，树形业务 |

## 四 典型微服务示例

| 模块名                    | 说明                                                |
| ------------------------- | --------------------------------------------------- |
| tobaccocloud-demo         | 微服务父级工程：继承 tobaccocloud-parent（pom）     |
| tobaccocloud-demo-api     | API 模块：存放 Entity（jar）                        |
| tobaccocloud-demo-service | Service 模块：Controller、Service、Mapper...（jar） |

