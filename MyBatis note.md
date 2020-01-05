MyBatis Note

## 配置

```xml
<maven.compiler.source>1.8</maven.compiler.source>
```

```xml
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
```

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.4.6</version>
</dependency>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

4 编写pojo

​	省略 get set 引入 lombok 工具

​	4.1要提供@Data注解

​	4.2要安装 lombok 插件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

* MyBatis 核心对象
   * SqlSessionFactoryBuilder
   * SqlSessionFactory
   * SqlSession

* mybatis-config.xml 系统核心配置文件

* mapper.xml SQL映射文件

> SqlSessionFactoryBuilder
>
> ​	用过即丢,其生命周期只存在于方法体内
>
> ​	可重用其来创建多个SqlSessionFactory事例
>
> ​	负责构建SqlSessionFactory,并提供多个build方法的重载

> SqlSessionFactory
>
> ​	SqlSessionFactory 是每个MyBatis应用的核心
>
> ​	作用: 创建SqlSession实例
>
> ``` SqlSession session = sqlSessionFactory.openSession(boolean autoCommit);``` --->true:关闭事务控制(默认)--->false:开启事务控制
>
> 作用域: Application
>
> 生命周期与应用的生命周期相同











































