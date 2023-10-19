# maven 导入

```xml
<!-- 上一个版本，常用 -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>xx.xx.xx</version>
</dependency>
```
```xml
<!-- 最新版本 -->
<dependency>
  <groupId>com.mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
</dependency>
```
# 数据源配置
在 `application.yml` 中
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/study
    username: yunxia
    password: 768874
```