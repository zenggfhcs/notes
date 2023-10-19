# mapper.xml

XML文件的名称与Mapper接口名称一致，并且放置在相同包下（同包同名）

XML文件的namespace属性为Mapper接口全限定名一致

XML文件中sql语句的id与Mapper 接口中的方法名一致

## 模板

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
      select * from Blog where id = #{id}
  </select>
</mapper>
```
## 数据操作

### Select
```xml
<select id="selectUser" parameterType="java.lang.Integer"
		resultType="com.study.entity.User">
     select * from Blog where id = #{id}
</select>
```
- id 跟接口方法名保持一致
- parameterType 说明参数类型，非必须
- resultType 说明返回值类型，如果进行了字段映射，使用resultMap代替

### Insert
```xml
<!-- 使用了别名替代全限定名 -->
<insert id="insertUser" parameterType="User" 
        keyProerty="id" useGenerateKeys="true">
    insert into 
        User(name)
    values(#{name})
</insert>
```
- id 跟接口方法名保持一致
- parameterType 说明参数类型
- 主键自增
  - keyProerty="id" 指定了对象接收返回的主键的字段为 id
  - useGenerateKeys="true" ，表示主键自增

### Update
```xml
<update id="updateUserName" parameterType="Student">
     UPDATE User SET name=#{name}
</update>
```
- id 跟接口方法名保持一致

- parameterType 说明参数类型

### Delete

```xml
<delete id="deleteStudentById" parameterType="int">
     delete from student where id=#{id}
</delete>
```

- id 跟接口方法名保持一致
- parameterType 说明参数类型

### 储存过程



## 标签

### `<if>`

属性 if.test 判断为 true 时，将 `<if></if>` 中的内容拼接到 sql 语句中

```xml
<if test="name != null and name != ''">
    name = #{name},
</if>
```

### `<where>`

去除内容中多余的 `and、or`
```sql
-- emp.sql
create table study.emp
(
    id          int unsigned auto_increment comment 'ID' 
    	primary key,
    username    varchar(20)                  not null comment '用户名',
    password    varchar(32) default '123456' null comment '密码',
    name        varchar(10)                  not null comment '姓名',
    gender      tinyint unsigned             not null comment '性别, 说明: 1 男, 2 女',
    image       varchar(300)                 null comment '图像',
    job         tinyint unsigned             null comment '职位, 说明: 1 班主任,2 讲师, 3 学工主管, 4 教研主管, 5 咨询师',
    entry_date  date                         null comment '入职时间',
    dept_id     int unsigned                 null comment '部门ID',
    create_time datetime                     not null comment '创建时间',
    update_time datetime                     not null comment '修改时间',
    constraint username						 
    	unique (username)
)  comment '员工表';
```
```xml
<select id="select" resultType="study.entity.Emp" parameterType="map">
    select
        id, username, password, name, gender, image, job, entry_date, dept_id, create_time, update_time
    from 
    	emp
    <where>
        <if test="gender != null and gender.length() > 0">
            and gender = #{gender}
        </if>
        <if test="begin != null">
            and entry_date >= #{begin}
        </if>
        <if test="end != null">
            and #{end} >= entry_date
        </if>
        <if test="name != null and name.length() > 0">
            and name like concat('%', #{name}, '%')
        </if>
    </where>
</select>
```
### `<set>`

去除内容中多余的 `,`

```xml
<update id="update" parameterType="study.entity.Dept">
    UPDATE dept
    <set>
        <if test="name != null">
            name = #{name},
        </if>
        <if test="createTime != null">
            create_time = #{createTime},
        </if>
        <if test="updateTime != null">
            update_time = #{updateTime},
        </if>
    </set>
    WHERE id = #{id}
</update>
```
### `<choose>`

该标签里面包含`<when>、<otherwise>`两个子标签，可以包含多个`<when>`与一个`<otherwise>`

它们联合使用，可完成类似Java中的开关语句switch…case的功能

用于优先级条件查询：
```xml
<!--若姓名不为空，则按照姓名查询；
    若姓名为空，则按照年龄查询；
    若没有查询条件，则没有查询结果 -->
<select id="searchStudentsChoose" resultMap="studentResultMap" parameterType= "Student">
    SELECT * FROM STUDENT
    <where>
        <choose>
            <when test="sname!=null and sname!=''">
                and studentname like '%' #{sname} '%'
            </when>
            <when test="age>0">
                and age=#{age}
            </when>
            <otherwise>
                and 1!=1
            </otherwise>
        </choose>
    </where>
</select>
```

### `<foreach>`

```xml
// 参数是数组 int[] ids
// 生成 (id1,..,idn)
<foreach collection="array" 
         open="(" close=")" separator=","
         item="id">
    #{id}
</foreach>
```

- collection属性表示要遍历的集合类型，
  - 如果是数组，其值就用 `array `
  - 如果是List，就用 `list` ，如果 List 的泛型是实体类型，使用 `.` 引用属性

- open  close  separator 这些属性用于对遍历内容进行SQL拼接

- item 属性说明集合每一项的别名


### `<sql>`

与 `<include />` 共同使用，用于定义可复用的 sql 片段

```xml
// sql
<sql id="all">
    select * from user
</sql>
```

在需要引用的地方，插入 `<include />` ：

```xml
// include
<include refid="all" />
```

- include.refid 与被引用sql片段的 sql.id 一致

### `<resultMap>`

实体别名定义

```xml
<resultMap id="BaseResultMap" type="com.qyci.entity.User">
    <!-- ... -->
    <!-- <id/> <result/> -->
</resultMap>
```

- id 用于引用该 resultMap

- type 指定与表映射的实体类


#### `<id>`

映射实体属性与表主键

```xml
<id column="id" jdbcType="INTEGER" property="id" />
```

- column 指定表内对应的列名

- jdbcType 说明该列对应的 sql 数据类型

- property 指定对应的实体属性


#### `<ruselt>`

同 `<id>`，用于映射实体除主键外属性

```xml
<result column="name" jdbcType="VARCHAR" property="name" />
```

#### `<association>`

另类的 `<resultMap>`，用于多对一映射

```xml
<association property="classes" javType="Classes">
    <!-- <id/> <result/> -->
</association>
```

- property 指定一方在实体中对应的属性

- javaType 指定属性的类型

- 可以使用 select属性 指定单独查询的 select.id ，要配合 column属性 使用，其用于指定 select 的动态参数所对应的字段


#### `<collection>`

另类的 `<resultMap>`，用于一对多映射

```xml
<collection property="students" ofType="Student">
    <!-- <id/> <result/> -->
</collection>
```

- property 指定在该实体中的对应的数组集合字段

- ofType 指定数组集合中数据的类型

- 可以使用 select属性 指定单独查询的 select.id ，要配合 column属性 使用，其用于指定 select 的动态参数所对应的字段
