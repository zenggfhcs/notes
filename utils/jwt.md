https://jwt.io/

# 依赖导入

```xml
<dependency>
   <groupId>io.jsonwebtoken</groupId>
   <artifactId>jjwt</artifactId>
   <!-- <version>0.9.1</version> -->
</dependency>
```

# 生成 JWT

```java
public String generateJWT() {
   // 定义载荷
   Map<String, Object> claims = new HashMap<>();
   // ... 设置载荷内容 ...
   String jwt = Jwts.builder()
     .setClaims(claims) // 设置载荷
     .signWith(SignatureAlgorithm.HS256, "miyao") // 签名算法  密钥
     .setExpiration(new Date(System.currentTimeMillis() + 12 * 3600 * 1000)) // 有效期
     .compact();
   return jwt;
}
```

# 校验 JWT

```java
public void parseJwt(String jwtStr){
    Claims claims = Jwts.parser()
   		.setSigningKey("miyao") //指定签名秘钥
        .parseClaimsJws(jwtStr) //解析令牌
        .getBody();
   	// 如果校验失败，会抛出异常
   	//
}
```