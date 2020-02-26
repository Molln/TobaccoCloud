# Dubbo API 工程迁移

迁移至新框架 API 工程

## 一 POM 文件

### 1 `<properties></properties>` 标签部分

`<properties>` 标签主要用来定义 Jar 版本，新框架中所有的 Jar 包版本在 `tobaccocloud-parent` 工程中定义，其它工程**不允许**定义 Jar 包版本。

### 2 其它部分

迁移至新框架 API 工程即可。

## 二 src/main/resources 文件夹

不建议迁移

## 二 src/main/java 文件夹

### 1 entity 包

迁移至新框架 API 工程，代码一般不需要修改

### 2 event 包

迁移至新框架 API 工程，代码一般不需要修改

### 3 service 包

不需要进行迁移。

### 4 vo 包

迁移至新框架 API 工程，代码一般不需要修改