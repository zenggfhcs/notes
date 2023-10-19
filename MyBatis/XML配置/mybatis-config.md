# mybatis-config.xml

## 模板

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
     <environment id="development">
     <!-- 使用 spring，事务管理和数据源都交由 spring 管理 -->
       <!-- 事务管理配置 -->
       <transactionManager type="JDBC"/>
       <!-- 数据源配置 -->
       <dataSource type="POOLED">
         <property name="driver" value="${driver}"/>
         <property name="url" value="${url}"/>
         <property name="username" value="${username}"/>
         <property name="password" value="${password}"/>
       </dataSource>
     </environment>
  </environments>
    
  <mappers>
     <!-- mapper 配置 -->
     <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

## 标签说明

### 结构

![img](D:\study\study-notes\images\mybatis\mybatis-config-structure.jpg)

### `<properties>`

将内部的配置转化为外部的配置，从而能够动态地替换内部定义的属性

#### 书写属性配置文件

jdbc.properties 文件

```properties
jdbc.driver = com.mysql.cj.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/study
jdbc.username = yunxia
jdbc.password = 768874
```
#### 导入属性文件中的属性

使用 `properties.resource` 属性说明属性文件的位置：

```xml
<properties resource="jdbc.properties" />
```
#### 引用属性

在此配置文件中，使用 ${} 内加属性全名引用属性：

```xml
<property name="driver" value="${jdbc.driver}" />
```
### `<setting>`

用来改变MyBatis运行时的行为，如开启延迟加载以及二级缓存：
```xml
<settings>
    <!-- 是否开启缓存 -->
    <setting name="cacheEnabled" value="true"/>
    <!-- 是否开启延迟加载，如果开启的话所有关联对象都会延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 是否启用关联对象属性的延迟加载，
        如果启用，对任意延迟属性的调用都会使用带有延迟加载属性的对象完整加载，
        否则每种属性都按需加载 -->
    <setting name="aggressiveLazyLoading" value="true"/>
</settings>
```
### `<typeAliases>`

作用是为 Java 的 POJO 类起别名

如果不取别名，配置文件若要引用一个POJO实体类，必须输入全限定性类名，

全限定性类名比较长，用了别名之后引用起来就简单多了

```xml
<typeAliases>
    <!-- 属性 alias 为别名，type 为类的全限定名 -->
    <typeAlias alias="Student" type="com.lifeng.entity.Student"/>
    <!-- 可以配置多个 typeAlias 标签 -->
</typeAliases>
```
如果有很多个，也可采用扫描包的方式：
```xml
<typeAliases>
    <!-- 属性 name 指定需要扫描的包，别名是首字母小写 -->
    <typeAlias name="com.lifeng.entity"/>
    <!-- 可以配置多个 typeAlias 标签 -->
</typeAliases>
```
### `<typeHandlers>`

将传入参数的javaType（Java类型）转换为jdbcType（JDBC类型，对应数据库的数据类型），

反之从数据库读出结果（即将jdbcType转换为javaType），作用是进行数据的类型映射

MyBatis提供了大量默认的内置转换器，可以完成大部分常见的类型转换，

但若默认的转换器无法满足要求时，可以自定义转换器。

自定义转换器需实现TypeHandler接口或者继承BaseTypeHandler类。创建好后需要进行如下注册：
```xml
<typeHandlers>
    <typeHandler handler="com.study.entity.MyTypeHandler"/>
</typeHandlers>
```
配置完成后由 `<ResultMap>` 引用

### `<environments>`

环境配置，主要用于数据源的配置

####   `<transactionManager>` 子标签

配置事务管理器，可选 JDBC 和 MANAGED 两种：

JDBC：使用JDBC的事务管理，通过数据源得到的连接来提交或回滚事务。

 MANAGED：使用容器来管理事务。

使用 Spring 可以交由 Spring 管理，无需配置

#### `<dataSource>` 子标签

用来配置数据源，即数据库的连接，它有 UNPOOLED、POOLED 和 JNDI 3种类型：
UNPOOLED 无连接池，每次请求都重新打开和关闭连接，即每一次都是新的连接，大型应用连接会很频繁，浪费资源，降低效率，一般只用于小型应用
POOLED 连接池，效率较高，响应速度快，通常都使用这种方式
JNDIJNDI 数据源，常用于EJB等容器

```xml
<environments>
    <environment>
        <transactionManager type="JDBC" />
        <!-- 属性 type 配置数据源类型 -->
        <dataSource type="POOLED" >
            <property name="driver" value="${jdbc.driver}" />
            <property name="url" value="${jdbc.url}" />
            <property name="username" value="${jdbc.username}" /> 
            <property name="password" value="${jdbc.password}" />       
        </dataSource>
    </environment>
</environments>
```


### `<mappers>`
**用于配置映射文件，指定映射文件的位置**

**使用 `/` 分割各级，mapper 标签可以有多个**

#### 使用resource属性引入类路径

```xml
<mappers>
	<mapper resource="com/study/dao/StudentMapper.xml" />
</mappers>
```

#### 使用url属性引入本地文件（也可以是网络地址）

```xml
<mappers>
     <mapper url="file:///E:/com/study/dao/StudentMapper.xml"/>
</mappers>
```

#### 使用class属性引入接口类

```xml
<mappers>
     <mapper class="com.study.dao.StudentMapper"/>
</mappers>
```

使用该种方式需要满足以下要求：

- 接口名称与映射文件名称一致。

- 接口与映射文件必须在同一个包中。

- 映射文件中标签的namespace命名空间的值为接口的全限定类名。

#### 使用包名引入

```xml
<mappers>
     <package name="com.study.dao" />
</mappers>
```

使用该种方式需要满足以下要求：

- DAO的实现类采用mapper动态代理实现。

- 映射文件与接口的名称相同。

- 映射文件与接口在同一个包中。

- 映射文件中标签的namespace命名空间的值为接口的全限定类名。

