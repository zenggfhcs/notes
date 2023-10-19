# 导入

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.4.2</version>
</dependency>
```

# 准备全查询
```java
@Select("SELECT * FROM user")
public List<User> list();
```
根据需要准备好需要分页的查询内容即可

# 查询分页
```java
public PageBean page (Integer page, Integer pageSize) {
    //1. 设置分页参数
    PageHelper.startPage(page,pagesize);
    //2. 执行全查询
    List<User> emplist = empMapper.list();
    // 分页封装
    Page<User> p = (Page<User>) empList;
    //3. 封装PageBean对象
    PageBean pageBean = new PageBean(p.getTotal()， p.getResult())
    return pageBean;
}
```