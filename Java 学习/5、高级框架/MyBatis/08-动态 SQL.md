[TOC]

## 一、if 语句

动态 SQL：简化拼装 SQL 的操作。

if 语句，可以动态的根据你的值来决定，是否需要动态的添加查询条件。

### 1、UserMapper 接口

```java
//根据user对象中的lastName和sex属性查询用户，希望lastName值不为null，性别只能是0和1
public List<User> queryUsersByNameAndSex(User user);
```

### 2、UserMapper.xml 配置文件

```xml
<!--    //根据user对象中的lastName和sex属性查询用户，希望lastName值不为null，性别只能是0和1-->
<!--    public List<User> queryUsersByNameAndSex(User user);-->
<select id="queryUsersByNameAndSex" resultType="com.xuwei.pojo.User">
  select
    id,last_name lastName,sex
  from
        t_user
  where
    <if test="lastName != null">
      last_name like concat ('%',#{lastName},'%')
    </if>
    <if test="sex == 0 || sex == 1">
      and sex = #{sex}
    </if>
</select>
```

### 3、测试文件

```java
@Test
public void queryUsersByNameAndSex() {
    SqlSession session = sqlSessionFactory.openSession();
    try {
        //得到Mapper接口
        UserMapper mapper = session.getMapper(UserMapper.class);
        System.out.println(mapper.queryUsersByNameAndSex(new User(null,"s",1)));
    } finally {
        session.close();
    }
}
```

## 二、where 语句

where语句，可以帮我们在多个动态语句中，有效的去掉前面的多余的 and  或 or 之类的多余关键字

```xml
<!--    //根据user对象中的lastName和sex属性查询用户，希望lastName值不为null，性别只能是0和1-->
<!--    public List<User> queryUsersByNameAndSex(User user);-->
<!-- 使用 where 语句 -->
<select id="queryUsersByNameAndSex" resultType="com.xuwei.pojo.User">
  select
    id,last_name lastName,sex
  from
    t_user
<!-- where 标签可以动态的去掉包含的内容前面的关键字 and 和 or -->
  <where>
    <!-- if 标签用来判断一个条件是否成立 -->
    <if test="lastName != null">
      last_name like concat('%',#{lastName},'%')
    </if>
    <if test = "sex == 0 || sex == 1">
      and sex = #{sex}
    </if>
  </where>
</select>
```

## 三、trim 语句

trim 可以动态在包含的语句前面和后面添加内容。也可以去掉前面或者后面给定的内容

- prefix 前面添加内容

- suffix 后面添加内容

- suffixOverrides 去掉的后面内容

- prefixOverrides 去掉的前面内容

```xml
<!--    //根据user对象中的lastName和sex属性查询用户，希望lastName值不为null，性别只能是0和1-->
    <!--    public List<User> queryUsersByNameAndSex(User user);-->
    <!-- 使用 trim 语句 -->
    <select id="queryUsersByNameAndSex" resultType="com.xuwei.pojo.User">
        select
        id,last_name lastName,sex
        from
        t_user
        <!-- 去掉and后面的语句，即sex语句失效，在前面添加where标签 -->
        <trim suffixOverrides="and" prefix="where">
            <if test="lastName != null">
                last_name like concat('%',#{lastName},'%') and
            </if>
            <if test = "sex == 0 || sex == 1">
                sex = #{sex}
            </if>
        </trim>
    </select>
```

## 四、choose(when，otherwise)语句

choose when otherwise 可以执行多路选择判断，但是只会有一个分支会被执行。类似switch case 语句

### 1、UserMapper 接口

```java
/**
  * 如果lastName有值，则使用它查询<br/>
  * 如果lastName没有值，sex值有效，则使用sex查询<br/>
  * 否则，你自己添加一个查询条件（默认）
*/
public List<User> queryUsersByNameAndSexChoose(User user);
```

### 2、UserMapper.xml 配置文件

```xml
<!--    /**-->
<!--    * 如果lastName有值，则使用它查询<br/>-->
<!--    * 如果lastName没有值，sex值有效，则使用sex查询<br/>-->
<!--    * 否则，你自己添加一个查询条件（默认）-->
<!--    */-->
<!--    public List<User> queryUsersByNameAndSexChoose(User user);-->
    <select id="queryUsersByNameAndSexChoose" resultType="com.xuwei.pojo.User">
        select
            id,last_name lastName,sex
        from
            t_user
        <where>
            <choose>
                <when test="lastName != null">
                    last_name like concat('%',#{lastName},'%')
                </when>
                <when test="sex == 0 || sex == 1">
                    sex = #{sex}
                </when>
                <otherwise>
                    last_name = 'weishao'
                </otherwise>
            </choose>
        </where>
    </select>
```

### 3、测试文件

```java
@Test
public void queryUsersByNameAndSexChoose() {
    SqlSession session = sqlSessionFactory.openSession();
    try {
        //得到Mapper接口
        UserMapper mapper = session.getMapper(UserMapper.class);
        System.out.println(mapper.queryUsersByNameAndSexChoose(new User(null,"16",1)));
    } finally {
        session.close();
    }
}
```

## 五、set 语句

删除条件后的逗号

**UserMapper 接口** ：

```java
//set语句
public int updateUser2(User user);
```

**UserMapper 配置文件** ：

```xml
<!--    //set语句-->
<!--    public int updateUser2(User user);-->
<update id="updateUser2" parameterType="com.xuwei.pojo.User">
  update
  t_user
  <set>
    <if test="lastName != null">
      last_name = #{lastName},
    </if>
    <if test="sex == 0 || sex == 1">
      sex = #{sex},
    </if>
  </set>
  where
  id = #{id}
</update>
```

**测试代码** ：

```java
@Test
public void updateUser2() {
    SqlSession session = sqlSessionFactory.openSession();
    try {
        //得到Mapper接口
        UserMapper mapper = session.getMapper(UserMapper.class);
        int result = mapper.updateUser2(new User(9,"weishao",0));
        System.out.println(result);
        session.commit();
    } finally {
        session.close();
    }
}
```

## 六、foreach 语句

类似于 select * from t_user where id in (1,3,4);

**UserMapper 接口** ：

```java
/**
     * 执行类似的语句：select * from t_user where id in (1,3,4);
     */
public List<User> queryUsersByIds(List<Integer> ids);
```

**UserMapper 配置文件** ：

```xml
<!--    /**-->
<!--    * 执行类似的语句：select * from t_user where id in (1,3,4);-->
<!--    */-->
<!--    public List<User> queryUsersByIds(List<Integer> ids);-->
<select id="queryUsersByIds" resultType="com.xuwei.pojo.User">
  select
    id,last_name lastName,sex
  from
    t_user
  <where>
    id in
    <!-- 
    foreach 遍历输出
     collection你要遍历的数据源
     items 表示当前遍历的数据
     open 表示遍历之前添加的内容 
     close 表示遍历之后添加的内容
     separator 给遍历的每个元素中间添加的内容
    -->
    <foreach collection="list" item="i" open="(" close=")" separator=",">
      #{i}
    </foreach>
  </where>
</select>
```

**测试代码** ：

```java
@Test
public void queryUsersByIds() {
    SqlSession session = sqlSessionFactory.openSession();
    try {
        //得到Mapper接口
        UserMapper mapper = session.getMapper(UserMapper.class);
        List list = new ArrayList();
        list.add(1);
        list.add(3);
        list.add(6);
        list.add(9);
        mapper.queryUsersByIds(list).forEach(System.out::println);
    } finally {
        session.close();
    }
}
```

## 七、总结

- if 语句可以动态地添加查询条件，比如

```xml
<if test="lastName != null">
  last_name like concat ('%',#{lastName},'%')
</if>
```

- where语句，可以帮我们在多个动态语句中，有效的去掉前面的多余的 and  或 or 之类的多余关键字

```xml
<where>
  <!-- if 标签用来判断一个条件是否成立 -->
  <if test="lastName != null">
    last_name like concat('%',#{lastName},'%')
  </if>
  <if test = "sex == 0 || sex == 1">
    and sex = #{sex}
  </if>
</where>
```

- trim 可以动态在包含的语句前面和后面添加内容。也可以去掉前面或者后面给定的内容

```xml
<!-- 去掉and后面的语句，即sex语句失效，在前面添加where标签 -->
<trim suffixOverrides="and" prefix="where">
  <if test="lastName != null">
    last_name like concat('%',#{lastName},'%') and
  </if>
  <if test = "sex == 0 || sex == 1">
    sex = #{sex}
  </if>
</trim>
```

- choose when otherwise 可以执行多路选择判断，但是只会有一个分支会被执行。类似switch case 语句

```xml
<choose>
  <when test="lastName != null">
    last_name like concat('%',#{lastName},'%')
  </when>
  <when test="sex == 0 || sex == 1">
    sex = #{sex}
  </when>
  <otherwise>
    last_name = 'weishao'
  </otherwise>
</choose>
```

- set 语句：删除条件后的逗号

```xml
<set>
    <if test="lastName != null">
      last_name = #{lastName},
    </if>
    <if test="sex == 0 || sex == 1">
      sex = #{sex},
    </if>
  </set>
```

- foreach 语句：类似于 select * from t_user where id in (1,3,4);

```xml
<!-- 
    foreach 遍历输出
     collection你要遍历的数据源
     items 表示当前遍历的数据
     open 表示遍历之前添加的内容 
     close 表示遍历之后添加的内容
     separator 给遍历的每个元素中间添加的内容
    -->
<foreach collection="list" item="i" open="(" close=")" separator=",">
  #{i}
</foreach>
```