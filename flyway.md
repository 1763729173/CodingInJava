# 数据库版本管理工具



### 什么是数据库版本管理？


做过开发的小伙伴们都知道，实现一个需求时，一般情况下都需要设计到数据库表结构的修改。那么我们怎么能保证项目多人开发时，多个数据库环境（测试，生产环境）能够保持一致呢？在没有数据库版本管理工具之前，需要将数据库修改脚本拷贝到每个数据库环境进行执行。而有了数据库版本管理工具之后，程序在启动的时候就会根据实现定义好的规则来进行数据库脚本的执行。




### 使用flyway


#### 使用环境
```
#博主用的是springboot项目，mysql数据库
```


#### 导入flayway和mysql依赖
```xml
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

#### 创建数据库脚本目录
```
在resources资源目录下创建db/migration目录。
```


#### 添加数据库脚本
```sql
#脚本命名规则 V<VERSION>__<NAME>.sql，P<VERSION>__<NAME>.sql。V代表只执行一次，P代表可以执行多次
#VERSION代表数据库脚本版本，NAME代表数据名称。

#这里使用V1_test.sql，脚本内容如下所示。

DROP TABLE IF EXISTS `role`;
CREATE TABLE `role`  (
  `id` int(11) NOT NULL,
  `name` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

INSERT INTO `role` VALUES (1, '1');

```


#### properties文件配置
```xml
#指定数据库脚本为UTF-8, flyway的配置有很多，有兴趣的小伙伴可以去看下
spring.flyway.encoding=utf-8

#如果原来的数据库不为空，则需要设置
spring.flyway.baseline-on-migrate=true

#设置数据库起始版本为0，默认为1。如果你写的sql脚本version小于等于起始版本则不会执行。
spring.flyway.baseline-version=0

#数据源配置
spring.datasource.url=jdbc:mysql://127.0.0.1/test?useUnicode=true&characterEncoding=utf8&useSSL=false
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=123456
```


#### 启动应用程序，查看控制台输出
![控制台图片](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595613155339.png)

#### 数据库查看
![数据库图片](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595613053441.png)

此时flyway会默认添加一张记录数据库版本信息的表，每次启动时会根据version值判断是否需要执行sql。


#### flyway是怎么执行的？


```
#spring-boot-dependencies 导入了flyway，mysql依赖。
#spring-boot-autoconfigure 中导入了FlywayAutoConfiguration自动配置类
```
