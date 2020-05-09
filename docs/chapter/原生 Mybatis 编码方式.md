# 原生 Mybatis 编码方式

本框架持久化部分使用 通用 Mapper，可以实现基本的增删改查。如果业务复杂，需要手动编写 SQL，本框架也支持原生 Mybatis 编码方式。

1 xxxMapper.xml 中编写语句

````xml
<select id="selectAllData" resultMap="BaseResultMap">
  	select ROW_ID,LABEL,PHOTO_PATH,CREATED_BY,CREATED_TIME,LAST_UPD_BY,LAST_UPD_TIME
  	from md_demo_base
</select>
````

2 xxxMapper.java 声明同名方法

````java
public interface DemoBaseMapper extends Mapper<DemoBase> {
	public List<DemoBase> selectAllData();
}
````

3 xxxService.java 调用 xxxMapper.java

```java
public List<DemoBase> selectAllData(){
    return this.mapper.selectAllData();
}
```

4 xxxController.java 调用 xxxService.java

````java
@GetMapping(value = "/selectAllData")
public Result selectAllData(){
    return ResultUtil.converter(demoBaseService.selectAllData());
}
````

