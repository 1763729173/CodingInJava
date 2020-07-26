

### 什么是nexus？

nexus是一个maven仓库管理器，使用nexus可以快速便捷的搭建自己的maven私有仓库。

### docker安装nexus

#### 拉取镜像

```shell
docker pull sonatype/nexus3
```

#### 后台执行镜像

```shell
docker run -d -p 8081:8081 --name nexus-dev
```
#### 查看nexus容器是否启动

![查看nexus](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595755709881.png)

### 访问本地的nexus

在浏览器url地址中输入localhost:8081，如果此时未能成功加载，等待几秒后再尝试刷新浏览器。

![nexus启动页面](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595755908761.png)

成功访问后，点击右上角sigin，按照提示从指定目录下获取admin账号的密码。

![登陆界面](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595756921379.png)

#### 添加常用代理源

选择maven2代理方式

![选择代理方式](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595757120500.png)

添加阿里云代理源

![阿里云代理源](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595757217213.png)

#### 设置常用代理到maven-public

maven-public是一个聚合仓库，当从这个仓库中获取依赖时，它会从成员列表中依次往下遍历，从对应的成员仓库中获取依赖。

![maven-public](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595757277901.png)

#### 设置release仓库可重复发布

配置release仓库可重复发布之后，可以重复发布同一个版本号的依赖。这里大家可以根据实际情况勾选是否启用。

![release仓库](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595757459822.png)

### 本地maven配置

配置好私有仓库之后，我们需要修改本地的maven配置和项目中的pom文件才能够跟私有仓库进行互动操作。

#### 修改settings.xml

这里需要注意settings.xml文件的优先级（用户级别>全局设置>自定义路径），具体的配置看下面的xml文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

 <localRepository>D:\Program Files\Maven\repository</localRepository>

  <pluginGroups>
  </pluginGroups>

  <proxies>
  </proxies>

  <servers>
      <server>
          <id>nexus</id>
          <username>admin</username>
          <password>chenhao.123</password>
      </server>
  </servers>

  <mirrors>
      <mirror>
          <id>nexus</id>
          <mirrorOf>*</mirrorOf>
          <url>http://localhost:8081/repository/maven-public/</url>
      </mirror>
  <profiles>
      <profile>
          <id>jdk-1.8</id>
          <properties>
              <maven.compiler.source>1.8</maven.compiler.source>
              <maven.compiler.target>1.8</maven.compiler.target>
              <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
          </properties>
      </profile>
      <profile>
          <id>nexus</id>
          <repositories>
              <repository>
                  <id>maven-public</id>
                  <url>http://localhost:8081/repository/maven-public/</url>
                  <releases>
                      <enabled>true</enabled>
                  </releases>
                  <snapshots>
                      <enabled>false</enabled>
                  </snapshots>
              </repository>
              <repository>
                  <id>maven-snapshots</id>
                  <url>http://localhost:8081/repository/maven-snapshots/</url>
                  <releases>
                      <enabled>false</enabled>
                  </releases>
                  <snapshots>
                      <enabled>true</enabled>
                  </snapshots>
              </repository>
              <repository>
                  <id>maven-releases</id>
                  <url>http://localhost:8081/repository/maven-releases/</url>
                  <releases>
                      <enabled>true</enabled>
                  </releases>
                  <snapshots>
                      <enabled>false</enabled>
                  </snapshots>
              </repository>
          </repositories>
          <pluginRepositories>
              <pluginRepository>
                  <id>maven-public</id>
                  <url>http://localhost:8081/repository/maven-public/</url>
                  <releases>
                      <enabled>true</enabled>
                  </releases>
                  <snapshots>
                      <enabled>false</enabled>
                  </snapshots>
              </pluginRepository>
              <pluginRepository>
                  <id>maven-snapshots</id>
                  <url>http://localhost:8081/repository/maven-snapshots/</url>
                  <releases>
                      <enabled>false</enabled>
                  </releases>
                  <snapshots>
                      <enabled>true</enabled>
                  </snapshots>
              </pluginRepository>
              <pluginRepository>
                  <id>maven-releases</id>
                  <url>http://localhost:8081/repository/maven-releases/</url>
                  <releases>
                      <enabled>true</enabled>
                  </releases>
                  <snapshots>
                      <enabled>false</enabled>
                  </snapshots>
              </pluginRepository>
          </pluginRepositories>
      </profile>

  </profiles>
    <activeProfiles>
        <activeProfile>jdk-1.8</activeProfile>
        <activeProfile>nexus</activeProfile>
    </activeProfiles>
</settings>

```

#### 创建一个maven项目A作为依赖提供者

这里在idea创建一个maven模板的项目即可，没有什么特别的操作。

#### 修改A项目的pom.xml

```xml
    <distributionManagement>
        <repository>
            <id>nexus</id>
            <name>releases Repository</name>
            <url>http://127.0.0.1:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>nexus</id>
            <name>snapshots Repository</name>
            <url>http://127.0.0.1:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>
```

#### 在A项目中创建一个Math作为测试

```java
package com.chenhao.util;

public class Math {
    public static int add(int a, int b){
        return a+b;
    }
}
```

#### 将项目A的jar包发布到私有仓库

在idea项目右边的maven工具来中，点击deploy按钮，查看控制台输出

![deploy](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595758169894.png)

#### 查看nexus上是否存在这个依赖

![nexus depencies](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595758226326.png)


#### 创建一个maven项目B作为依赖使用者

这里创建一个项目B来作为依赖的使用者，并且需要注意的是，在项目A中使用了deploy操作，此时已经将依赖上传到本地仓库。所以此时应该将本地仓库中的依赖删除。

```java
package com.chenhao;
import com.chenhao.util.Math;
public class Main {
    public static void main(String[] args) {
        System.out.println(Math.add(1, 1));
    }
}

```
#### 修改项目B的pom.xml

```xml
 <dependencies>
        <dependency>
            <groupId>com.chenhao</groupId>
            <artifactId>example</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
```

#### 运行项目B

![run project b](https://gitee.com/chenhaogit/blogimages/raw/master/xsj/1595758528168.png)





