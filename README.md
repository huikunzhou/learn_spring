# Spring介绍

spring是一个分层的JavaSE/EEfull-stack（一站式）轻量级开源框架。框架的主要优势之一就是分层架构，spring负责管理项目中的所有对象，并在各个分层给予了解决方案。（所谓一么功能）。

[Spring官网](http://www.spring.io)

[Spring 官网指南](https://spring.io/guides)

[Spring 系列主要项目](https://spring.io/projects)

[Spring 官方文档翻译（1-6章）](https://blog.csdn.net/tangtong1/article/details/51326887)

## EE开发三层架构：

- WEB层：SpringMVC
- 业务层：Bean管理（IOC）
- 持久层：Spring的JDBC模板（ORM模板用于整合其它持久层框架）

## Spring 优点：

- 方便解耦，简化开发（Spring管理所有对象创建及依赖关系维护）
- AOP 编程的支持（Spring提供面向切面编程，可实现对程序进行权限拦截、运行监控等功能）
- 声明式事务的支持（通过配置完成对事务的管理，而无需手动编程）
- 方便程序的测试（Spring 对 Junit4 支持，通过注解方便测试 Spring 程序）
- 方便集成各种优秀框架（内部提供了对各种优秀框架的直接支持）
- 降低 JavaEE API 的使用难度（对 JavaEE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等），都提供了封装）

## Spring 的体系结构：

![Spring 的体系结构](http://ww1.sinaimg.cn/large/005MTBGuly1fvylu0c5gxj30l10c5tcr.jpg)


# Spring项目搭建

1. 创建web动态工程

2. 导入spring核心jar包

![spring核心jar包](http://ww1.sinaimg.cn/large/005MTBGuly1fvylvdapzjj309z032jrd.jpg)

3. 创建对象

4. 书写配置文件注册对象到容器

5. 编写测试程序



## Spring 思想

IOC：Inverse Of Control 反转控制，即反转对象的创建方式。对象的创建及依赖关系由spring来管理。

DI：Dependency Injection 依赖注入，实现IOC思想需要DI支持。

	注入方式：set/get方法、构造注入、字段注入	

	注入类型：值类型（8大基本数据类型）、引用类型（依赖对象）

BeanFactory接口：spring原始接口，功能单一；每次获得对象时才会创建对象

ApplicationContext接口：容器启动会创建容器中配置的所有对象
![](http://ww1.sinaimg.cn/large/005MTBGuly1fvym0ap11yj30ie0f2t9z.jpg)


## Spring 配置详解：

- bean 元素：描述需要spring容器管理的对象
- name属性：根据该名称获得对象 ，名称可重复，能使用特殊字符
- class属性:    被管理对象的完整类名
- id属性:         与name属性一致，名称不可重复，不能使用特殊字符
- scope属性：
  - singleton（默认单例子对象:只创建一个实例）
  - prototype：每次获得都会创建新的对象（整合strut时，ActionBean配置为多例）
  - request：web环境下，对象与request生命周期一致
  - session：web环境下，对象与session生命周期一致

Spring 模块化配置：

    <import resource="cn/stormzhou/bean/applicationContext.xml"/>

## Spring Bean 对象创建方式：

1. 空参构造创建：
    ```
    <bean name="user" class="cn.stormzhou.bean.UserService"></bean>
    ```
2. 静态工厂创建：
    ```
    <bean name="f_user" class="cn.stormzhou.bean.UserFactory" factory-method="createUser"></bean>
    ```
3. 实例化工厂创建：
    ```
    <bean name="user_factory" class="cn.stormzhou.bean.UserFactory"></bean>
    <bean name="s_user" factory-bean="user_factory" factory-method="createUser2"></bean>
    ```
## Bean 生命周期：



## Spring 属性注入（值类型 | 引用类型）：	

1. set方法注入：
    ```
    <bean name="user" class="cn.stormzhou.bean.UserService">
        <!-- 值类型注入 -->
        <property name="name" value="zh"></property>
        <property name="age" value="14"></property>
        <!-- 引用类型注入: 为car属性配置下面的car对象 -->
        <property name="car" ref="car"></property>
    </bean>
    
    <bean name="car" class="cn.stormzhou.bean.Car">
    	<property name="name" value="audi"></property>
        <property name="color" value="yellow"></property>
    </bean>
    ```
2. 构造函数注入：
     ```
    name属性：构造函数的参数名
    index属性: 构造函数的参数索引
    type属性：构造函数的参数类型
    
    <bean name="user2" class="cn.stormzhou.bean.UserService">
    	<constructor-arg name="name" value="111" index="0" 	type="Integer"></constructor-arg>
        <constructor-arg name="car" ref="car" ></constructor-arg>
    </bean>
    ```
3. P名称空间注入：
    ```
    导入p名称空间、p冒号属性注入
    
    <bean name="user3" p:name="hk" p:age="21" p:car-ref="car" class="cn.stormzhou.bean.UserService">
    </bean>
    ```
4. spel注入：
    ```
    <bean name="user4" class="cn.stormzhou.bean.UserService">
    	<property name="name" value="#{user.name}"></property>
        <property name="age" value="#{user3.age}"></property>
        <property name="car" ref="car"></property>
    </bean>
    ```
5. 复杂类型注入：
    ```
    <bean name="cb" class="cn.stormzhou.bean.CollectionBean">
        <!-- 数组、list只注入一个值/对象，用value/ref -->
        <property name="arr" value="tom"></property> 
        <property name="list" value="tom"></property>    
        <!-- 数组、list注入多个值/对象，用value/ref -->
        <property name="arr">
    <!-- 	  			<list></list> -->
            <array>
                <value>tom</value>
                <value>www</value>
                <ref bean="user4"/>
            </array>
        </property>
    	  		
        <!-- map类型注入 -->
        <property name="map">
            <map>
                <entry key="url" value="www.baidu.com"></entry>
                <entry key="user" value-ref="user4"></entry>
                <entry key-ref="user3" value-ref="user2"></entry>
            </map>
        </property>
    	  		
        <!-- proertie类型注入 -->
        <property name="prop">
            <props>
                <prop key="url">wwwwww</prop>
                <prop key="word">122</prop>
            </props>
        </property>		
    </bean>
    ```


