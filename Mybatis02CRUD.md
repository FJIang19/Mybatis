# Mybatis CRUD

## Mybatis_CRUD(使用maven构建项目,采用xml方式)
1. 配置环境(参照Mybatis_User demo)
2. 在dao接口加上CRUD方法
3. 在mapper映射xml文件里加上CRUD方法
![](https://github.com/FJIang19/Mybatis/blob/master/images/Mybatis02_CRUD/CRUD.png "")
4. 编写测试类
     1. 将执行方法封装，降低代码冗余
     ```
      @Before
      public void Init() throws IOException{
          in = Resources.getResourceAsStream("SqlMapConfig.xml");
          //2.创建SqlSessionFactory工厂
          SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
          SqlSessionFactory factory = builder.build(in);
          //3.使用工厂创建SqlSession对象
          session = factory.openSession();
          //4.使用SqlSession创建Dao接口代理对象
          userDao = session.getMapper(UserDao.class);
      }

      @After
      public void destroy() throws IOException {
          session.commit();
          session.close();
          in.close();
      }
     ```
     2. 测试增方法
     ```
      @Test
      public void testInsert(){
          User user = new User();
          user.setUsername("裁判");
          user.setPassword("123456");
          userDao.InsertUser(user);
      }
     ```
     3. 测试减方法
     ```
      @Test
      public void testDelete(){
          userDao.DeleteUser(1);
      }
     ```
     4. 测试改方法
     ```
      @Test
      public void testUpdate(){
          User user = new User();
          user.setId(11);
          user.setUsername("裁判");
          user.setPassword("123456777777777");
          userDao.UpdateUser(user);
      }
     ```
     5. 模糊查询
     ```  
      @Test
      public void testFindByName(){
          List<User> users = userDao.findByName("%王%");
          for (User user : users) {
              System.out.println(user);
          }
      }
     ```
 ## 注意事项
1. 在mapper映射xml文件中 id必须为接口里的方法名 resultType要对应实体类的全类名，parameterType为传入类型，必须与方法传入的类型的全类名一致
2. 模糊查询中，在mapper映射xml文件里不支持%符号，所以必须在实现方法里加%，具体看测试步骤
3. 模糊查询中，在mapper映射xml文件中，将sql语句写为 select * from user where username like '%${value}' 效果是一样的，
且不用在执行方法里加%符号，不过此时value是固定的，底层使用的是字符串拼接，即Statement,而不在xml文件里加%的使用PrepateStatement,显然，
后者更好，因为PrepateStatement有效的防止了sql注入的问题
