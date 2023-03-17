## Springboot 项目过程中疑难笔记

#### 1. SecurityConfig类中的各种弃用问题。

> 快速的解决方法：给Springboot版本降级
>
> ![image-20230315025412287](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230315025412287.png)
>
> 直接去mvn repo搜这个spring-boot-starter-parent, 然后找一个低的版本。一般来讲Springboot 3.0出现方法或者类弃用问题的，降到SpringBoot 2.7及其以下版本就ok。
>
> https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter



* 改完parent之后，还需要将其他有关springboot的依赖版本也修改。这里修改了这些. 可以看到spring-boot-starter-jdbc, Spring-boot-starter-security还是3.0.4版本，目前这两个没有导致mvn install的时候报错，所以就没有改。改完版本后，执行mvn clean和mvn install。不知道怎么执行的idea插件里下个maven helper。然后右键pom.xml -> run Maven就能找到。

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.6.14</version>
          <relativePath/> <!-- lookup parent from repository -->
      </parent>
      <groupId>com.ls</groupId>
      <artifactId>lsBackend</artifactId>
      <version>0.0.1-SNAPSHOT</version>
      <name>LsBackend</name>
      <description>LsBackend</description>
      <properties>
          <java.version>17</java.version>
      </properties>
      <dependencies>
  
          <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-devtools</artifactId>
              <version>2.6.14</version>
          </dependency>
  
  
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-thymeleaf</artifactId>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
              <version>2.6.14</version>
          </dependency>
  
  
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-test</artifactId>
              <scope>test</scope>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-jdbc -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-jdbc</artifactId>
              <version>3.0.4</version>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
          <dependency>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
              <version>1.18.26</version>
              <scope>provided</scope>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->
          <dependency>
              <groupId>com.mysql</groupId>
              <artifactId>mysql-connector-j</artifactId>
              <version>8.0.32</version>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/com.baomidou/mybatis-plus-boot-starter -->
          <dependency>
              <groupId>com.baomidou</groupId>
              <artifactId>mybatis-plus-boot-starter</artifactId>
              <version>3.5.3.1</version>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/com.baomidou/mybatis-plus-generator -->
          <dependency>
              <groupId>com.baomidou</groupId>
              <artifactId>mybatis-plus-generator</artifactId>
              <version>3.5.3.1</version>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-security -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-security</artifactId>
              <version>3.0.4</version>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-api -->
          <dependency>
              <groupId>io.jsonwebtoken</groupId>
              <artifactId>jjwt-api</artifactId>
              <version>0.11.5</version>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-impl -->
          <dependency>
              <groupId>io.jsonwebtoken</groupId>
              <artifactId>jjwt-impl</artifactId>
              <version>0.11.5</version>
              <scope>runtime</scope>
          </dependency>
  
  
          <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-jackson -->
          <dependency>
              <groupId>io.jsonwebtoken</groupId>
              <artifactId>jjwt-jackson</artifactId>
              <version>0.11.5</version>
              <scope>runtime</scope>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/org.jetbrains/annotations -->
          <dependency>
              <groupId>org.jetbrains</groupId>
              <artifactId>annotations</artifactId>
              <version>24.0.1</version>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
          <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>javax.servlet-api</artifactId>
              <version>4.0.1</version>
              <scope>provided</scope>
          </dependency>
  
          <!-- https://mvnrepository.com/artifact/com.auth0/java-jwt -->
          <dependency>
              <groupId>com.auth0</groupId>
              <artifactId>java-jwt</artifactId>
              <version>4.3.0</version>
          </dependency>
  
      </dependencies>
  
      <build>
          <plugins>
              <plugin>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-maven-plugin</artifactId>
              </plugin>
          </plugins>
      </build>
  
  </project>
  
  ```

   

  

  
  
  > 原因出现在 **@RequestParam**的使用。RequestParam和PathVariable类似，还是需要在url传参的。至于怎么传，看这个文章。
  >
  > http://events.jianshu.io/p/64e0f54fab11

  * LoginService接口的实现类**LoginServiceImpl**中，如果登录出现错误（账号不存在、密码错误），会自动报异常，**目前来讲**是只能用postman看异常，在idea中并不会显示。
  
  ```java
  Authentication authenticate = authenticationManager.authenticate(usernamePasswordAuthenticationToken);//如果登陆失败，会自动处理（报异常）
  ```

  ​	错误示范：
  
  ![image-20230315041840707](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230315041840707.png)

​		 正确样例（用户名存在、密码正常）：

​	![image-20230315042012389](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230315042012389.png)





#### 2. 登录（Login）接口调试时出现403错误

> 原因出现在 **@RequestParam**的使用。RequestParam和PathVariable类似，还是需要在url传参的。至于怎么传，看这个文章。
>
> http://events.jianshu.io/p/64e0f54fab11

* LoginService接口的实现类**LoginServiceImpl**中，如果登录出现错误（账号不存在、密码错误），会自动报异常，**目前来讲**是只能用postman看异常，在idea中并不会显示。

```java
Authentication authenticate = authenticationManager.authenticate(usernamePasswordAuthenticationToken);//如果登陆失败，会自动处理（报异常）
```

​	错误示范：

![image-20230315041840707](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230315041840707.png)

​		 正确样例（用户名存在、密码正常）：

​	![image-20230315042012389](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230315042012389.png)





#### 3. 展示信息（Info）界面调试时细节

键值对取决于JwtAuthenticationTokenFilter中这几行代码的参数怎么写

```java
String token = request.getHeader("Authorization");//这个是key

if (!StringUtils.hasText(token) || !token.startsWith("Bearer ")) {  //startwith 后面的是Value
	filterChain.doFilter(request, response);
	return;
}

token = token.substring(7);
```

在这里，键值对就是Authorization=Bearer Token  注意空格。并且是放在headers里面，不是data！

![image-20230315054637224](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230315054637224.png)



#### 4. 注册用户细节(Register)

* 使用Post方法
* @RequestParam    和Login一样
* 调试样例
  ![image-20230315071820089](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230315071820089.png)



#### 5. 和前端对接时遇到的一些问题

> 1. Https localhost 和 Http localhost 区别？为什么会出现跨域问题
> 2. Controller接管了映射的url。通过Map接受了数据。并且将数据提出来，重新封装并交给ServiceImpl类来处理。
