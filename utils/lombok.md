# 依赖导入

```xml
<dependency>
     <groupId>org.projectlombok</groupId>
     <artifactId>lombok</artifactId>
</dependency>
```

# 常用注解

## `@Data`

自动生成缺失的 getter()、setter()、toString()、equals()、hashCode()

## `@NoArgsConstructor`

为实体类生成无参的构造器方法

## `@AllArgsConstructor` 

为实体类生成除了static修饰的字段之外带有各参数的构造器方法