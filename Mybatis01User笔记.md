# Mybatis
## Mybatis入门
**Mybatis封装了Dao层，使其不用再覆写实体类接口的方法，仅需在映射mapper.xml里写sql语句即可，简化了开发**

### Mybatis_User demo(使用maven构建项目）
  1. 在Java包下创建实体类User
  2. 在Java包下创建接口UserDao
  3. 导入Mybatis依赖
  ```
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>x.x.x</version>
    </dependency>
  ```
  4. 创建Mybatis连接xml文件
  ```
    <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration
                PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-config.dtd">
        <configuration>
            <environments default="development">
                <environment id="development">
                    <transactionManager type="JDBC"/>
                    <dataSource type="POOLED">
                        <property name="driver" value="com.mysql.jdbc.Driver"/>
                        <property name="url" value="jdbc:mysql://localhost:3306/demo3"/>
                        <property name="username" value="root"/>
                        <property name="password" value="root"/>
                    </dataSource>
                </environment>
            </environments>
            <mappers>
                <mapper resource="dao/UserDao.xml"/>
            </mappers>
     </configuration>
  ```
  5. 创建映射xml文件（执行sql语句）
  ```
     <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper
              PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="dao.UserDao">
          <select id="findAll" resultType="domain.User">
          select * from user
        </select>
      </mapper>
  ```
### 测试demo
  1. 在Test包下创建Test类
  2. 执行步骤
      
      
      
      1. 读取配置文件
      ```
      InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
      ```
      2. 创建SqlSessionFactory工厂
      ```
       SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
       SqlSessionFactory factory = builder.build(in);
      ```
      3. 使用工厂创建SqlSession对象
      ```
       SqlSession session = factory.openSession();
      ```
      4. 使用SqlSession创建Dao接口代理对象
      ```
       UserDao userDao = session.getMapper(UserDao.class);
      ```
      5. 使用代理对象执行方法
      ```
      List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }
      ```
      6. 释放资源
      ```
      session.close();
      in.close();
      ```
 
 ## 注意事项
 1. Mybatis连接xml文件必须放在resource根目录下
 2. 执行sql的xml文件必须放在resource目录与java包下Dao包同级的目录
  ![注意](https://github.com/FJIang19/Mybatis/blob/master/%E6%B3%A8%E6%84%8F.png "注意")
 3. 执行sql的xml文件里的 namespace 必须为Java包下的接口的全名，select id必须与接口里的方法一致，resultType必须与Java包下实体类全名
  ![](https://github.com/FJIang19/Mybatis/blob/master/%E6%B3%A8%E6%84%8F2.png "注意2")
      
