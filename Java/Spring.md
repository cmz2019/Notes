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

