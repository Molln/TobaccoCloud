# 通用 Mapper

## 一 关于通用 Mapper

建议先行查看官方文档：

<https://github.com/abel533/Mapper/wiki/4.1.mappergenerator>

## 二 Example 用法

### 1 查询

示例

````java
// 1 创建 Example 对象，Country 为待查询的实体类
Example example = new Example(Country.class);
// 2 设置查询条件：id > 100 且 id < 151
example.createCriteria().andGreaterThan("id", 100).andLessThan("id",151);
// 3 设置查询条件（与第二步构成 or 关系）：id < 41
example.or().andLessThan("id", 41);
// 4 执行查询
List<Country> countries = mapper.selectByExample(example);
````

日志

````
DEBUG [main] - ==>  Preparing: SELECT id,countryname,countrycode FROM country WHERE ( id > ? and id < ? ) or ( id < ? ) ORDER BY id desc 
DEBUG [main] - ==> Parameters: 100(Integer), 151(Integer), 41(Integer)
DEBUG [main] - <==      Total: 90
````

### 2 动态 SQL

示例

````java
// 1 创建 Example 对象，Country 为待查询的实体类
Example example = new Example(Country.class);
// 2 创建 查询条件对象
Example.Criteria criteria = example.createCriteria();
// 3 判断某个参数是否为 null
if(query.getCountryname() != null){
    // 设置查询条件：countryname like 参数
    criteria.andLike("countryname", query.getCountryname() + "%");
}
// 4 判断某个参数是否为 null
if(query.getId() != null){
    // 设置查询条件：id > 指定参数
    criteria.andGreaterThan("id", query.getId());
}
List<Country> countries = mapper.selectByExample(example);
````



日志

````
DEBUG [main] - ==>  Preparing: SELECT id,countryname,countrycode FROM country WHERE ( countryname like ? ) ORDER BY id desc 
DEBUG [main] - ==> Parameters: China%(String)
DEBUG [main] - <==      Total: 1
````

### 3 排序

示例

````java
Example example = new Example(Country.class);
example.orderBy("id").desc().orderBy("countryname").orderBy("countrycode").asc();
List<Country> countries = mapper.selectByExample(example);
````

日志

```
DEBUG [main] - ==>  Preparing: SELECT id,countryname,countrycode FROM country order by id DESC,countryname,countrycode ASC 
DEBUG [main] - ==> Parameters: 
DEBUG [main] - <==      Total: 183
```

### 4 去重

示例

````java
Example example = new Example(Country.class);
//设置 distinct
example.setDistinct(true);
example.createCriteria().andLike("countryname", "China%");
example.or().andIdGreaterThan(100);
List<Country> countries = mapper.selectByExample(example);
````

日志

````
DEBUG [main] - ==>  Preparing: SELECT distinct id,countryname,countrycode FROM country WHERE ( countryname like ? ) or ( Id > ? ) ORDER BY id desc 
DEBUG [main] - ==> Parameters: A%(String), 100(Integer)
DEBUG [main] - <==      Total: 95
````

### 5 设置查询列

示例

````java
Example example = new Example(Country.class);
example.selectProperties("id", "countryname");
List<Country> countries = mapper.selectByExample(example);
````

日志

````
DEBUG [main] - ==>  Preparing: SELECT id , countryname FROM country ORDER BY id desc 
DEBUG [main] - ==> Parameters: 
DEBUG [main] - <==      Total: 183
````



除了这里提到的方法外，还有很多其他的方法，可以查看 Example 源码进行了解。

````java
tk.mybatis.mapper.entity.Example
````

