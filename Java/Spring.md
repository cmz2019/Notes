# 1、Spring

## 简介

官网：[https://spring.io/projects/spring-framework#overview](https://spring.io/projects/spring-framework#overview)

官方下载地址：[http://repo.spring.io/release/org/springframework/spring](http://repo.spring.io/release/org/springframework/spring)

GitHub：[https://github.com/spring-projects/spring-framework](https://github.com/spring-projects/spring-framework)

Maven仓库：[https://mvnrepository.com/search?q=spring](https://mvnrepository.com/search?q=spring)

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.17.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.17.RELEASE</version>
</dependency>
```

## 优点

+ Spring是一个开源的免费的框架（容器）
+ Spring是一个轻量级的、非入侵式的框架
+ 控制反转（IOC），面向切面编程（AOP）
+ 支持事务的处理，对框架整合的支持

==总结一句话：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！==

## 组成

![image-20210922165208094](https://gitee.com/cmz2000/album/raw/master/image/image-20210922165208094.png)

# 2、IOC

## IOC理论推导

1. UserDao 接口

```java
public interface UserDao {
    void getUser();
}
```

2. UserDaoImpl 实现类

```java
public class UserDaoImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("默认获取用户的数据");
    }
}
```

```java
public class UserDaoMySQLImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("MySql获取用户配置！");
    }
}
```

3. UserService 业务接口

```java
public interface UserService {
    void getUser();
}
```

4. UserServiceImpl 业务实现

```java
public class UserServiceImpl implements UserService {
    // 此处写死了实现类，若要修改需要改动源代码
    private UserDao userDao = new UserDaoImpl(); 

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```

5. 测试类实现

```java
public class MyTest {
    public static void main(String[] args) {
        // 用户实际调用的是业务层，他们不需要接触dao层
        UserService userService = new UserServiceImpl();
        userService.getUser();
    }
}
```

![image-20210922174151644](https://gitee.com/cmz2000/album/raw/master/image/image-20210922174151644.png)

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改 `UserServiceImpl` 的源代码。如果程序代码量十分大，修改一次的成本代价十分昂贵！

![image-20210923140421541](https://gitee.com/cmz2000/album/raw/master/image/image-20210923140421541.png)

我们使用一个 `set` 接口实现 `UserServiceImpl` 中的动态注入

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao;

    // 利用set进行动态实现值的注入
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```

测试类为

```java
public class MyTest {
    public static void main(String[] args) {
        // 用户实际调用的是业务层，他们不需要接触dao层
        UserService userService = new UserServiceImpl();
        ((UserServiceImpl) userService).setUserDao(new UserDaoMySQLImpl());
        userService.getUser();
    }
}
```

+ 之前，程序是主动创建对象，控制权在程序员手上
+ 使用了 set 注入后，程序不再具有主动性，而是变成了被动的接受对象

这种思想，从本质上解决了问题，程序员不用再去管理对象的创建了。系统的耦合性大大降低，可以更加专注的在业务的实现上。这就是 IOC 的原型。

![image-20210923140445863](https://gitee.com/cmz2000/album/raw/master/image/image-20210923140445863.png)

## IOC本质

控制反转loC（Inversion of Control），是一种设计思想，DI（依赖注入）是实现loC的一种方法，也有人认为DI只是loC的另一种说法。没有loC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![image-20210923141329997](https://gitee.com/cmz2000/album/raw/master/image/image-20210923141329997.png)

+ loC是Spring框架的核心内容，使用多种方式完美的实现了loC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现loC。
+ Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从loc容器中取出需要的对象。

![image-20210923142352445](https://gitee.com/cmz2000/album/raw/master/image/image-20210923142352445.png)采用XML方式配置Bean的时候，采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

官网上xml配置的框架为：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是loC容器，其实现方法是依赖注入（Dependency Injection，Dl）。

## 通过例子理解

Hello.java 代码如下：

```java
package com.strawberry;

public class Hello {
    private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}
```

可以看到定义了 str 变量，但是没有赋值

自定义配置文件 beans.xml 内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--使用Spring来创建对象，在Spring这些都称为Bean
        类型 变量名= new 类型();
        Hello hello = new Hello();

        id = 变量名
        class = new 的对象;
        property 相当于给对象中的属性设置一个值
    -->
    <bean id="hello" class="com.strawberry.Hello">
        <!--
            ref : 引用 Spring 容器中创建好的对象
            value : 具体的值，基本数据类型!
    	-->
        <property name="str" value="你好"/>
    </bean>

</beans>
```

自定义测试类如下：

```java
public class MyTest {
    public static void main(String[] args) {
        // 获取Spring的上下文对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.toString());
    }
}
```

+ Hello 对象是谁创建的？
  + hello对象是由Spring创建的
+ Hello 对象的属性是怎么设置的？
  + hello对象的属性是由Spring容器设置的

这个过程就叫控制反转

+ 控制：谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring后，对象是由Spring来创建的

+ 反转：程序本身不创建对象，而变成被动的接收对象

+ 依赖注入：就是利用set方法来进行注入

IOC是一种编程思想，由主动的编程变成被动的接收。一句话总结IOC：对象由Spring进行创建、管理、装配。

## IOC创建对象的方式

### 无参构造

默认使用无参构造方法创建对象

Hello.java 代码如下：

```java
package com.strawberry.pojo;

public class Hello {
    private String name;

    public Hello() {
        System.out.println("Hello的无参构造器");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show() {
        System.out.println("name=" + name);
    }
}
```

在 beans.xml 中配置如下：

```xml
<bean id="hello" class="com.strawberry.pojo.Hello">
    <property name="name" value="草莓汁"/>
</bean>
```

测试如下：

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Hello hello = (Hello) context.getBean("hello");
        hello.show();
    }
}
```

输出为：

```
Hello的无参构造器
name=草莓汁
```

### 有参构造

#### 下标赋值

```java
public Hello(String name) {
    this.name = name;
    System.out.println("Hello的有参构造器");
}
```

```xml
<!-- 第一种，下标赋值 -->
<bean id="hello" class="com.strawberry.pojo.Hello">
    <constructor-arg index="0" value="草莓汁"/>
</bean>
```

#### 类型赋值

```xml
<!-- 第二种，类型赋值 -->
<!-- 不建议使用，无法区分多个同类型的参数 -->
<bean id="hello" class="com.strawberry.pojo.Hello">
    <constructor-arg type="java.lang.String" value="草莓汁"/>
</bean>
```

#### 参数名赋值（推荐）

```xml
<!-- 第三种，通过构造器参数名赋值 -->
<bean id="hello" class="com.strawberry.pojo.Hello">
    <constructor-arg name="name" value="草莓汁"/>
</bean>
```

# 3、Spring配置

## 别名alias

```xml
<bean id="hello" class="com.strawberry.pojo.Hello">
    <constructor-arg name="name" value="草莓汁"/>
</bean>
<!-- 名字hello和名字helloNew指的是同一个对象，这就是别名 -->
<alias name="hello" alias="helloNew"/>
```

原来的名字也能用，别名也能用，但是不推荐用 alias 这种方法

## bean配置

```xml
<!--
	id : bean的唯一标识符
	class : bean对象所对应的全限定名（包名+类名）
	name : 别名，相当于alias，但是name可以同时取多个别名（用逗号或空格或分号分隔）
-->
<bean id="hello" class="com.strawberry.pojo.Hello" name="hello2 hello3,hello4">
    <property name="name" value="草莓汁"/>
</bean>
```

## import配置文件

一个正规的配置文件命名为applicationContext.xml，这是一个总的配置文件，项目中可以有多个配置文件，这样的好处是，可以由多个人编写，最后汇总，汇总到主配置文件applicationContext.xml

![image-20210923205421682](https://gitee.com/cmz2000/album/raw/master/image/image-20210923205421682.png)

创建 ApplicationContext 实例的时候可以选用多个配置文件

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml","Beans.xml");
```

或者只选择 applicationContext.xml 配置文件，将其他的配置文件导入 applicationContext.xml

```xml
<import resource="beans.xml"/>
<import resource="beans2.xml"/>
<import resource="beans3.xml"/>
```

多个配置文件中存在相同内容的bean是没关系的（内容相同会被合并）

# 4、依赖注入

## 构造器注入

前面已经说过了，[点击跳转](#IOC创建对象的方式)

## set方式注入（重点）

+ 依赖注入：Set注入
  + 依赖：bean对象的创建依赖于容器
  + 注入：bean对象中的所有属性，由容器来注入

### 通过例子理解

+ 自定义复杂对象类型

```java
package com.strawberry.pojo;

public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

+ 真实测试对象

```java
package com.strawberry.pojo;

import java.util.*;

public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbies;
    private Map<String, String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

	/* getter和setter方法，省略 */
}
```

+ beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.strawberry.pojo.Address">
        <property name="address" value="北京"/>
    </bean>

    <bean id="student" class="com.strawberry.pojo.Student">
        <!-- 第一种，普通值注入，value -->
        <property name="name" value="草莓汁"/>
        
        <!-- 第二种，bean注入，ref -->
        <property name="address" ref="address"/>
        
        <!-- 第三种，数组注入 -->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>三国演义</value>
                <value>西游记</value>
                <value>水浒传</value>
            </array>
        </property>
        
        <!-- 第四种，list注入 -->
        <property name="hobbies">
            <list>
                <value>听歌</value>
                <value>看电影</value>
            </list>
        </property>
        
        <!-- 第五种，map注入 -->
        <property name="card">
            <map>
                <entry key="身份证" value="123456"/>
                <entry key="银行卡" value="abcdef"/>
            </map>
        </property>
        
        <!-- 第六种，set注入 -->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>COC</value>
            </set>
        </property>
        
        <!-- 第七种，null注入 -->
        <property name="wife">
            <null/>
        </property>
        
        <!-- 第八种，properties注入 -->
        <property name="info">
            <props>
                <prop key="username">root</prop>
                <prop key="password">123456</prop>
            </props>
        </property>
    </bean>
</beans>
```

## 拓展方式注入

### p-namespace

p表示property，用于存在==无参构造器==的bean对象的依赖注入操作

官网模板

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="john-classic" class="com.example.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>

    <bean name="john-modern"
        class="com.example.Person"
        p:name="John Doe"
        p:spouse-ref="jane"/>

    <bean name="jane" class="com.example.Person">
        <property name="name" value="Jane Doe"/>
    </bean>
</beans>
```

#### 实例演示

```java
package com.strawberry.pojo;

public class User {
    private String name;
    private int age;

    /* 省略getter和setter */
}
```

```xml
<!-- p命名空间注入，可以直接注入属性的值，property -->
<bean id="user" class="com.strawberry.pojo.User" p:name="草莓汁" p:age="18"/>
```

### c-namespace

c表示constructor，用于存在==有参构造器==的bean对象的依赖注入操作

官网模板

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:c="http://www.springframework.org/schema/c"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="beanTwo" class="x.y.ThingTwo"/>
    <bean id="beanThree" class="x.y.ThingThree"/>

    <!-- traditional declaration with optional argument names -->
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg name="thingTwo" ref="beanTwo"/>
        <constructor-arg name="thingThree" ref="beanThree"/>
        <constructor-arg name="email" value="something@somewhere.com"/>
    </bean>

    <!-- c-namespace declaration with argument names -->
    <bean id="beanOne" class="x.y.ThingOne" c:thingTwo-ref="beanTwo"
        c:thingThree-ref="beanThree" c:email="something@somewhere.com"/>

</beans>
```

#### 实例演示

```java
package com.strawberry.pojo;

public class User {
    private String name;
    private int age;
    
    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    /* 省略getter和setter */
}
```

```xml
<!-- c命名空间注入，通过有参构造器注入，constructor-arg -->
<bean id="user2" class="com.strawberry.pojo.User" c:name="草莓汁" c:age="18"/>
```

**注意**：p命名和c命名空间不能直接使用，需要导入xml约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```





