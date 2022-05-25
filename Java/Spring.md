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

![image-20210922165208094](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210922165208094.png)

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

![image-20210922174151644](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210922174151644.png)

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改 `UserServiceImpl` 的源代码。如果程序代码量十分大，修改一次的成本代价十分昂贵！

![image-20210923140421541](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210923140421541.png)

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

![image-20210923140445863](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210923140445863.png)

## IOC本质

控制反转loC（Inversion of Control），是一种设计思想，DI（依赖注入）是实现loC的一种方法，也有人认为DI只是loC的另一种说法。没有loC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![image-20210923141329997](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210923141329997.png)

+ loC是Spring框架的核心内容，使用多种方式完美的实现了loC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现loC。
+ Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从loc容器中取出需要的对象。

![image-20210923142352445](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210923142352445.png)采用XML方式配置Bean的时候，采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

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

![image-20210923205421682](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210923205421682.png)

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
<beans xmlns="http://www.springframework.org/schema/beans"    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    xmlns:p="http://www.springframework.org/schema/p"    xsi:schemaLocation="http://www.springframework.org/schema/beans        https://www.springframework.org/schema/beans/spring-beans.xsd">    <bean name="john-classic" class="com.example.Person">        <property name="name" value="John Doe"/>        <property name="spouse" ref="jane"/>    </bean>    <bean name="john-modern"        class="com.example.Person"        p:name="John Doe"        p:spouse-ref="jane"/>    <bean name="jane" class="com.example.Person">        <property name="name" value="Jane Doe"/>    </bean></beans>
```

#### 实例演示

```java
package com.strawberry.pojo;public class User {    private String name;    private int age;    /* 省略getter和setter */}
```

```xml
<!-- p命名空间注入，可以直接注入属性的值，property --><bean id="user" class="com.strawberry.pojo.User" p:name="草莓汁" p:age="18"/>
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
xmlns:p="http://www.springframework.org/schema/p"xmlns:c="http://www.springframework.org/schema/c"
```

## 作用域 bean-scopes

|    范围     |                             描述                             |
| :---------: | :----------------------------------------------------------: |
|  singleton  |  （默认）为每个Spring lOC容器的单个 Object 实例定义单个bean  |
|  prototype  |             为任意数量的 Object 实例定义单个bean             |
|   request   | 将单个bean定义范围限定为单个HTTP请求的生命周期。也就是说，每个HTTP请求都有自己的bean实例，该实例是在单个bean定义的后面创建的。仅在web-aware Spring `ApplicationContext` 的context中有效 |
|   session   | 将单个bean定义范围限定为HTTP Session的生命周期。仅在web-aware Spring `ApplicationContext` 的context中有效 |
| application | 将单个bean定义范围限定为ServletContext的生命周期。仅在web-aware Spring `ApplicationContext` 的context中有效 |
|  websocket  | 将单个bean定义范围限定为 webSocket的生命周期。仅在web-aware Spring `ApplicationContext` 的context中有效 |

### 单例 singleton

<img src="https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210925145005670.png" alt="image-20210925145005670" style="zoom:80%;" />

默认机制为单例模式，也可以显式指定 scope

```xml
<bean id="accountService" class="com.something.DefaultAccountService"/><!-- the following is equivalent, though redundant (singleton scope is the default) --><bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

### 原型 prototype

<img src="https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210925150814868.png" alt="image-20210925150814868" style="zoom:80%;" />

每次从容器中 get 的时候，都会产生一个新对象

显式指定原型模式：

```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

# 5、bean自动装配

+ 自动装配，是Spring满足bean依赖的一种方式
+ autowire
+ Spring会在上下文中自动寻找，并自动给bean装配属性

在Spring中有三种装配的方式：

1. 在 xml 中显式配置
2. 在 java 中显式配置
3. 隐式的自动装配【重要】

## XML方式（传统）

先定义三个类用于测试

```java
package com.strawberry.pojo;

public class Dog {
    public void shout() {
        System.out.println("汪~");
    }
}
```

```java
package com.strawberry.pojo;

public class Cat {
    public void shout() {
        System.out.println("喵~");
    }
}
```

```java
package com.strawberry.pojo;

public class People {
    private Cat cat;
    private Dog dog;
    private String name;

	/* 省略get和set方法 */
}
```

xml配置为

```xml
<bean id="cat" class="com.strawberry.pojo.Cat"/>
<bean id="dog" class="com.strawberry.pojo.Dog"/>

<bean id="people" class="com.strawberry.pojo.People">
    <property name="name" value="草莓汁"/>
    <property name="cat" ref="cat"/>
    <property name="dog" ref="dog"/>
</bean>
```

测试方法如下：

```java
@Test
public void test() {
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    People people = context.getBean("people", People.class);
    System.out.println(people.toString());
    people.getCat().shout();
    people.getDog().shout();
}
```

## XML自动装配

在xml配置文件中，设置bean的 `autowire` 进行自动装配

### byName

```xml
<!--
	byName: 会自动在容器上下文中查找和自己对象set方法后面的值对应beanId的bean
	如 setCat() 方法，就回去找 beanId 为 cat 的 bean
	如果更改了Dog类的beanId为dog1，则会报错，无法找到对应类
-->
<bean id="people" class="com.strawberry.pojo.People" autowire="byName">
    <property name="name" value="草莓汁"/>
</bean>
```

### byType

```xml
<!--
	byType: 会自动在容器上下文中查找和自己对象set方法的参数对应类型的bean
	但是该类型对象对应的bean不能重复，全局只能有一个
	使用byType方法自动装配时，对应类型的bean可以省略id，因为用不到
-->
<bean id="people" class="com.strawberry.pojo.People" autowire="byType">
    <property name="name" value="草莓汁"/>
</bean>
```

小结：

+ byName的时候，需要保证所有bean的 id 唯一，并且这个id需要和自动注入的属性的set方法的方法名一致
+ byType的时候，需要保证所有bean的 class 唯一，并且这个class需要和自动注入的属性的类型一致

## 注解自动装配

使用注解自动装配须知：

1. 导入约束：`xmlns:context="http://www.springframework.org/schema/context"`
2. 配置注解的支持：`<context:annotation-config/>`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

### @Autowired

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <bean id="cat" class="com.strawberry.pojo.Cat"/>
    <bean id="dog" class="com.strawberry.pojo.Dog"/>
    <bean id="people" class="com.strawberry.pojo.People"/>
</beans>
```

```java
package com.strawberry.pojo;

import org.springframework.beans.factory.annotation.Autowired;

public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    
    private String name;

    /* 省略set和get方法 */
}
```

+ 可以在 set 方法上使用该注解
+ 也可以直接在属性上使用
  + 在属性上使用时，可以省略对应的 set 方法，前提是该属性在 IOC（Spring）容器中存在且符合 byType 要求或 byName 要求（先以 byType 方式查找，再以 byName 方式）
  + 如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解@Autowired完成，上述两个要求都不符合的时候，即没有对应的 id，并且有重复类型的bean，则需要显式的指定该属性使用哪一个bean进行注入，方法为 `@Qualifier(value = "bean-id")`

```xml
<bean id="dog2" class="com.strawberry.pojo.Dog"/>
<bean id="dog1" class="com.strawberry.pojo.Dog"/>
```

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog1")	// 使用@Qualifier配合@Autowired
    private Dog dog;
    private String name;
}
```

+ @Autowired有一个唯一参数required，默认参数值为true，若设置为false，`@Autowired(required = false)`，则表示自动装配的对象可以为 null
  + 使用@Nullable修饰自动装配的对象也表示可以为null

```java
public class People {
    @Autowired(required = false)
    private Cat cat;
    private Dog dog;
    
    @Autowired
    public void setDog(@Nullable Dog dog) {
        this.dog = dog;
    }
}
```

### @Resource

@Resource的使用与@Autowired使用方法类似，与@Autowired不同的是@Resource先以 byName 方式查找，之后以 byType 方式查找

```java
public class People {
    @Resource
    private Cat cat;
    @Resource
    private Dog dog;
    private String name;
}
```

@Resource有很多参数

![image-20210925225012760](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210925225012760.png)

可以使用 name 和 type 参数显式指定使用哪一个或哪一类 bean

### 总结

- @Autowired先以 byType 方式查找，之后以 byName 方式查找
  - 可以定义在很多处位置，但是直接定义在成员上最方便
  - 可以配合@Qualifier注解进行限定
  - 内置required参数可以允许属性为null，和@Nullable功能一致
  - 这个注解是Spring的

- @Resource先以 byName 方式查找，之后以 byType 方式查找
  - 内置很多参数，使用方便
  - 这个注解是java的

# 6、使用注解注入

在 applicationContext.xml 配置文件中指定要扫描的包，才能使用注解

```xml
<context:component-scan base-package="com.strawberry.pojo"/>
```

## bean

测试类为 User.java

```java
package com.strawberry.pojo;

public class User {
    public String name = "草莓汁";
}
```

### xml方式

一种方式是直接在xml中注册bean

```xml
<bean id="user" class="com.strawberry.pojo.User"/>
```

### @Component

另一种方式是在 java 代码中使用注解 @Component 修饰类

```java
package com.strawberry.pojo;

@Component
public class User {
    public String name = "草莓汁";
}
```

这两种方式等价

## 属性

### xml方式

```xml
<bean id="user" class="com.strawberry.pojo.User">
    <property name="name" value="草莓汁"/>
</bean>
```

### @Value

使用 @Value 注解，既可以修饰属性，也可以修饰对应的 set 方法

```java
package com.strawberry.pojo;

@Component
public class User {
//    @Value("草莓汁")
    public String name;
    
    @Value("草莓汁")
    public void setName(String name) {
        this.name = name;
    }
}
```

这两种方式等价

## 衍生的注解

@Component有几个衍生注解，我们在web开发中，会按照MVC三层架构分层。

+ dao【@Repository】
+ service【@Service】
+ controller【@Controller】

这四个注解功能是一样的，都是表示将某个类注册到Spring容器中装配Bean，只是习惯上每个层都使用对应的注解

```java
@Repository
public class UserDao {
}
```

```java
@Service
public class UserService {
}
```

```java
@Controller
public class UserController {
}
```

## 自动装配

[见上文](#注解自动装配)

## 作用域

```java
@Component
@Scope("prototype")
public class User {
    public String name;
}
```

等价于

```xml
<bean id="user" class="com.strawberry.pojo.User" scope="prototype"/>
```

## 小结

xml与注解：

+ XML适用于任何场合，维护简单
+ 注解：不是自己的类使用不了，维护相对复杂，

最佳配合方法

+ xml管理Bean

+ 注解只负责完成属性注入（切记开启注解的支持和指定扫描的包）

```xml
<!--指定要扫描的包，这个包下的注解就会生效-->
<context:component-scan base-package="com.strawberry"/>
<context:annotation-config/>
```

# 7、使用Java的方式配置Spring

自定义 User.java 实体类

```java
// 这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中
@Component
public class User {
    private String name;

    @Value("strawberry") // 属性注入值
    public void setName(String name) {
        this.name = name;
    }
	
    /* 省略get方法和toString方法 */
}
```

实现一个 JavaConfig 配置类

```java
// @Configuration也会Spring容器托管，注册到容器中
// 本质上就是一个@Component
// @Configuration代表这是一个配置类，就和 beans.xml 一样
@Configuration
@ComponentScan("com.strawberry.pojo")
public class Config {

    // 注册一个bean，相当于我们之前写的一个bean标签
    // 这个方法的名字，相当于bean标签中的id属性
    // 这个方法的返回值，相当于bean标签中的class属性
    @Bean
    public User getUser() {
        return new User(); // 返回要注入到bean的对象
    }
}
```

- 经过@Configuration的修饰 Config 已经成为一个配置类了，类似于 Context.xml 配置文件
- @ComponentScan设置查找范围
- @Bean相当于之前写的bean标签，所修饰的方法的方法名就是bean-id
- 方法的返回值相当于 bean-class

测试方法如下：

```java
@Test
public void test() {
    //如果完全使用了配置类方式去做，我们就只能通过 AnnotationConfig 上下文来获取容器，通过配置类的class对象加载
    ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
    User user = context.getBean("getUser", User.class);
    System.out.println(user);
}
```

+ 如果有多个@Configuration，可以使用@Import注解，加入配置类

```java
@Configuration
@ComponentScan("com.strawberry.pojo")
@Import(Config2.class)
public class Config {
    @Bean
    public User getUser() {
        return new User();
    }
}
```

这种纯Java的配置方式，在SpringBoot中随处可见

# 8、AOP

## 代理模式

代理模式是SpringAOP的底层！

代理模式分类：

+ 静态代理
+ 动态代理

![image-20210927165240871](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210927165240871.png)

### 静态代理

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人

#### 租房中介例子

通过租房例子来理解：

```java
// 租房
public interface Rent {
    public void rent();
}
```

```java
// 房东
public class Landlord implements Rent {
    @Override
    public void rent() {
        System.out.println("房东要出租房子！");
    }
}
```

```java
public class Proxy implements Rent {
    private Landlord landlord;

    public Proxy() {
    }

    public Proxy(Landlord landlord) {
        this.landlord = landlord;
    }

    @Override
    public void rent() { // 可以扩展业务
        seeHouse();
        landlord.rent();
        fare();
    }

    // 看房
    public void seeHouse() {
        System.out.println("中介带你看房子！");
    }

    // 中介费
    public void fare() {
        System.out.println("收中介费");
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        // 房东
        Landlord landlord = new Landlord();
        // 代理
        Proxy proxy = new Proxy(landlord);
        // 租客不用面对房东，直接找中介即可
        proxy.rent();
    }
}
```

客户找中介，中介找房东，实现租房的方法，还能添加一些附属操作

代码步骤：

- 接口
- 真实角色
- 代理角色
- 客户端访问角色

#### 优缺点

代理模式的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共业务交给代理角色，实现业务分工
- 公共业务发生扩展时，方便管理

缺点：

- 一个真实角色就会产生一个代理角色，代码量增多

#### 加深理解

![image-20210927173515597](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20210927173515597.png)

如果一条线已经做出来了，突然要在中间加一个功能，不能去修改原有的业务代码（这在公司中是大忌），只能在原有的基础上添加功能，通过代理，调用的是原有的功能，但是在调用原有的功能的同时能像上面的租房的例子一样，增加一些功能。

### 动态代理

+ 动态代理和静态代理的角色一样
+ 静态代理每代理一个角色就要重新编写一个静态代理对象，代码量十分庞大
+ 动态代理的代理类是动态生成的，不是直接写好的

动态代理分两大类：基于接口的动态代理、基于类的动态代理

- 基于接口的动态代理：JDK原生的动态代理
- 基于类的动态代理：cglib
- 基于java字节码实现：JavaAssist（现在用的比较多）

需要了解两个类：Proxy，InvocationHandler

- Proxy：代理
- InvocationHandler：调用处理程序

>+ Proxy提供了创建动态代理类和实例的静态方法，它也是由这些方法创建的所有动态代理类的超类。
>+ InvocationHandler是由代理实例的调用处理程序实现的接口。每个代理实例都有一个关联的调用处理程序。当在代理实例上调用方法时，方法调用将被编码并分派到其调用处理程序的invoke方法。

自定义动态代理工具类：

通过反射的方式，判断被代理对象的类型，处理代理实例

```java
// 用这个类来自动生成代理类
public class ProxyInvocationHandler implements InvocationHandler {
    // 被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    // 生成得到代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), 
                target.getClass().getInterfaces(), this);
    }

    // 处理代理示例，并返回结果
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 动态代理的本质，就是使用反射机制实现
        Object res = method.invoke(target, args);
        return res;
    }
}
```

使用示例：

```java
public class Client {
    public static void main(String[] args) {
        // 真实角色
        UserServiceImpl userService = new UserServiceImpl();
        // 代理角色，不存在
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        pih.setTarget(userService); // 设置要代理的对象
        // 动态生成代理类
        UserService proxy = (UserService) pih.getProxy();
        
        proxy.rent();
    }
}
```

动态代理的好处：

+ 包含静态代理的所有好处
+ 一个动态代理类代理的是一个接口，一般就是对应的一类业务
+ 一个动态代理类可以代理多个类，只要是实现了同一个接口即可

## AOP

### 什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![image-20211004200247046](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20211004200247046.png)

不改变原有的业务，增加功能

### AOP在Spring中的作用

==提供声明式事务；允许用户自定义切面==

+ 横切关注点：跨越应用程序多个模块的方法或功能。与我们业务逻辑无关的，但我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等 ...
+ 切面（Aspect）：横切关注点被模块化的特殊对象。它是一个类。
+ 通知（Advice）：切面必须要完成的工作。它是类中的一个方法。
+ 目标（Target）：被通知对象。
+ 代理（Proxy）：向目标对象应用通知之后创建的对象。
+ 切入点（PointCut）：切面通知执行的 “地点” 的定义。
+ 连接点（JointPoint）：与切入点匹配的执行点。

![image-20211004200820080](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20211004200820080.png)

SpringAOP中，通过Advice定义横切逻辑，支持五种类型的的advice

![image-20211004201809576](https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20211004201809576.png)

即 AOP 不改变原有代码的情况下，去增加新的功能

### 使用Spring实现AOP

导入依赖包：

