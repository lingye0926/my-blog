## DispatcherServlet 配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```
## SpringMVC参数配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

   <!-- 扫描组件，将加上@Controller注解的类作为springMVC的控制层   -->
    <context:component-scan base-package="com.ellin.test"/>

   <!-- 配置视图解析器   -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/view/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>

</beans>
```
## 在SpringMVC组件所指定的包中创建controller和handler
```java
@Controller
public class TestController {
    /*
    @requestMapping:设置请求映射，把请求和控制层中的方法设置映射关系
    当请求路径和@RequestMapping的value属性一致时，则该注解所标注的方法即为请求的方法

    @requestMapping 可以加在类上，也可以加在方法上
    如果类和方法上都有，应该一层一层的访问，先访问类，再访问类中的方法

    1. method 用于设置请求方式，只有客户端发送请求的方式和method的值一致，才能处理请求
       请求方式： GET查询 POST添加 PUT更新 DELETE删除
    2. params:用来设置客户端传到服务器的数据，支持表达式
    3. headers: 用来设置请求头信息，所发送的请求的请求头信息一定要和headers属性一致
     */
    //括号里的约束越多，请求对应的范围就越小
    @RequestMapping(value = "/hello", method = RequestMethod.GET,params = {"username","!age"},headers = {"Accept-Language: en,zh-CN;q=0.9,zh;q=0.8"})
    public String helloworld(String username, String password){
        System.out.println(username + " " + password);
        System.out.println("success");
        return "success";
    }
}
```
### springMVC支持Ant方式的请求路径
   在Ant中，有3种匹配符<br>
    *：任意字符<br>
    ？：任意一个字符<br>
    **：任意多层目录<br>

### springMVC URL请求参数占位符
通过注解@PathVariable来获取占位符的值

```java
   /*
    以前：localhost:8080/SpringMVC_01/testREST?id=1001&username=admin
    现在：localhost:8080/SpringMVC_01/testREST/1001/admin
     */
    @RequestMapping("testREST/{id}/{username}")
    public String testREST(@PathVariable("id") Integer id,@PathVariable("username") String username){
        System.out.println("id:" + id +", username:" + username);
        return "success";
    }
```