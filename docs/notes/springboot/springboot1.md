---
title: springboot1
createTime: 2025/04/23 00:09:53
permalink: /article/ll4b5zvy/
---
## 1.热部署

1.在实际的项目开发调试过程中会频繁地修改后台类文件，导致需要重新编译、重新启动，整个过程非常麻烦，影响开发效率。

2.Spring Boot提供了spring-boot-devtools组件，使得无须手动重启Spring Boot应用即可重新编译、启动项目，大大缩短编译启动的时间。

3.devtools会监听classpath下的文件变动，触发Restart类加载器重新加载该类，从而实现类文件和属性文件的热部署。

4.并不是所有的更改都需要重启应用（如静态资源、视图模板），可以通过设置spring.devtools.restart.exclude属性来指定一些文件或目录的修改不用重启应
用

### 1.1步骤

#### a.pom.xml新增如下依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional> <!-- 不会传递到依赖此项目的其他项目 -->
</dependency>
```

#### b.在application.properties新增配置

```properties
# springboot启动热部署
spring.devtools.restart.enabled=true
# 热部署监听目录
spring.devtools.restart.additional-paths=src/main/java
```

#### c.idea额外配置

- 打开Settings页面，在左边的菜单栏依次找到 Build,Execution,Deployment→Compile，勾选Build project automatically
- 按Ctrl+Shift+Alt+/快捷键调出Maintenance页面，单击Registry，勾选compiler.automake.allow.when.app.running复选框。
- 做完这两步配置之后，若开发者再次在IntelliJ IDEA中修改代码，则项目会自动
  重启。

## 2.Controller

### 2.1web入门

1. Spring Boot将传统Web开发的mvc、json、tomcat等框架整合，提供了spring-boot-starter-web组件，简化了Web应用配置。

2. 创建SpringBoot项目勾选Spring Web选项后，会自动将spring-boot-starterweb组件加入到项目中。

3. spring-boot-starter-web启动器主要包括web、webmvc、json、tomcat等基础依赖组件，作用是提供Web开发场景所需的所有底层依赖。

4. webmvc为Web开发的基础框架，json为JSON数据解析组件，tomcat为自带的容器依赖。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## 2.2 控制器

1. Spring Boot提供了@Controller和@RestController两种注解来标识此类负责接收和处理HTTP请求

2. 如果请求的是页面和数据，使用@Controller注解即可；如果只是请求数据，则可以使用@RestController注解
3. 默认情况下，@RestController注解会将返回的对象数据转换为JSON格式。

```java
@RestController
public class HelloController {
    @RequestMapping(path = "/hello", method = RequestMethod.GET)
    public String hello() {
        return "你好";
    }
}    
```

### 2.3路由映射

- @RequestMapping注解主要负责URL的路由映射。添加在类上，则这个类的所有路由映射都会加上此映射规则前缀，如果添加在方法上，则仅对该方法生效
- @RequestMapping注解包含很多属性参数来定义HTTP的请求映射规则。常用的属性参数如下
  - value 请求URL的路径
  - method: HTTP请求方法  例：method = RequestMethod.GET

- @RequestMapping支持使用通配符匹配URL。”*”匹配任意字符，符号“**”匹配任意路径，符号“?”匹配单个字符。

- Method匹配也可以使用@GetMapping、@PostMapping等注解代替。

### 2.4参数传递

- @RequestParam：将请求参数绑定到控制器的方法参数上，接收的参数来自HTTP请求体或请求url的QueryString，当请求的参数名称与Controller的业务方法参数名称一致时,@RequestParam可以省略

- @PathVaraible：用来处理动态的URL，URL的值可以作为控制器中处理方法的参数

- @RequestBody：接收的参数是来自requestBody中，即请求体。一般用于处理非 Content-Type: application/x-www-form-urlencoded编码格式的数据，比如：`application/json`、`application/xml`等类型的数据