[TOC]

#### 1、什么是 MyBatis？

1. MyBatis 是一个持久层的框架，它内部封装了 JDBC，开发时只需要关注 SQL 语句本身，不需要花费精力去处理加载驱动、创建连接、创建 statement  等繁杂的过程。程序员直接编写原生态 sql，可以严格控制 sql 执行性能，灵活度高。

2. MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO(普通的 Java 对象) 映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

3. 通过 xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过 java 对象和 statement 中sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回。（从执行 sql 到返回 result 的过程）。

#### 2、MyBatis 的优缺点？

优点：

1. 基于 SQL 语句编程，相当灵活，且 SQL 语句写在 XML 里，解除 sql 与程序代码的耦合，便于管理；提供 XML 标签，支持编写动态 SQL 语句，并可重用。
2. 与 JDBC 相比，减少了 50%以上的代码量，消除了 JDBC 大量冗余的代码，不需要手动开关连接。
3. 很好的与各种数据库兼容（因为 MyBatis 使用 JDBC 来连接数据库，所以只要JDBC 支持的数据库 MyBatis 都支持）。

4. 能够与 Spring 很好的集成；
5. 提供映射标签，支持对象与数据库的 ORM 字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

缺点：

1. SQL 语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写 SQL 语句的功底有一定要求。
2. SQL 语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。

#### 3、MyBatis 框架适用场合

1. MyBatis 专注于 SQL 本身，是一个足够灵活的 DAO 层解决方案。
2. 对性能的要求很高，或者需求变化较多的项目，如互联网项目，MyBatis 将是不错的选择。

#### 4、#{} 和 ${} 的区别是什么？

- #{} 是预编译处理，MyBatis 在处理 #{} 时，会将 sql 中的 #{} 替换为 ? 号，调用 PreparedStatement 的set 方法来赋值。
- ${}是字符串替换，Mybatis 在处理${}时，就是把${}替换成变量的值。
- 使用#{}可以有效的防止 SQL 注入，提高系统安全性。

#### 5、当实体类中的属性名和表中的字段名不一样 ，怎么办 ？

- 第 1 种：通过在查询的 sql 语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

```sql
select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
```

- 第 2 种：通过 <resultMap> 来映射字段名和实体类属性名的一一对应的关系。

```xml
<select id="getOrder" parameterType="int"
        resultMap="orderresultmap">
    select * from orders where order_id=#{id}
</select>
<resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
    <!–用 id 属性来映射主键字段–>
    <id property=”id” column=”order_id”>
    <!–用 result 属性来映射非主键字段，property 为实体类属性名，column 为数据表中的属性–>
    <result property = “orderno” column =”order_no”/>
    <result property=”price” column=”order_price” />
</reslutMap>
```

#### 6、模糊查询 like 语句应该怎么写？

- 第 1 种：在 sql 语句中拼接通配符，会引起 sql 注入。

```xml
<select id=”selectlike”>
    select * from foo where bar like "%"#{value}"%"
</select>
```

- 第 2 种：在 java 代码中添加 sql 通配符，意思就是在 java 中调用函数传参时就参数就已经加上了 sql 通配符。

```java
string wildcardname = “%smi%”;
list<name> names = mapper.selectlike(wildcardname);
```



- 第 3 种：使用 concat 函数进行拼接，当我们需要做类似于 like 这种查询的时候。我们可以使用 #{} 组合 concat 来解决参数输入，以及不需要在传递参数的时候，加两个 %% 的情况。还可以解决 sql 注入问题。

```xml
<!--    //字符串拼接-->
<!--    public User findUserLikeName3(String lastName);-->
<select id="findUserLikeName3" resultType="user">
    select id,last_name lastName,sex
    from t_user
    where last_name like concat('%',#{name},'%')
</select>
```

#### 7、通常一个 Xml 映射文件，都会写一个 Dao 接口与之对应，请问，这个 Dao 接口的工作原理是什么？Dao 接口里的方法，参数不同时，方法能重载吗？

Dao 接口即 Mapper 接口，接口的全限名，就是映射文件中的 namespace 的值；接口的方法名，就是映射文件中 Mapper 的 Statement 的 id 值；接口方法内的参数，就是传递给 sql 的参数。

Mapper 接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为 key 值，可唯一定位一个 MapperStatement。在 Mybatis 中，每一个<select>、<insert>、<update>、<delete>标签，都会被解析为一个 MapperStatement 对象。

举例：`com.mybatis3.mappers.StudentDao.findStudentById`，可以唯一找到 `namespace` 为 `com.mybatis3.mappers.StudentDao` 下面 id 为 `findStudentById` 的 `MapperStatement`。

Mapper 接口里的方法，是不能重载的，因为是使用 全限名+方法名 的保存和寻找策略。Mapper 接口的工作原理是 JDK 动态代理，Mybatis 运行时会使用 JDK动态代理为 Mapper 接口生成代理对象 proxy，代理对象会拦截接口方法，转而执行 MapperStatement 所代表的 sql，然后将 sql 执行结果返回。

####  8、MyBatis 是如何进行分页的？分页插件的原理是什么？

Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页。可以在 sql 内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。

分页插件的基本原理是使用 Mybatis 提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的 sql，然后重写 sql，根据 dialect 方言，添加对应的物理分页语句和物理分页参数。

#### 9、Mybatis是如何将sql执行结果封装为目标对象并返回的？都有哪些映射形式？

- 第一种是使用<resultMap>标签，逐一定义数据库列名和对象属性名之间的映射关系。
- 第二种是使用 sql 列的别名功能，将列的别名书写为对象属性名。

- 有了列名与属性名的映射关系后，Mybatis 通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。

#### 10、Xml 映射文件中，除了常见的 select|insert|update|delete 标签之外，还有哪些标签？

<resultMap>、<parameterMap>、<sql>、<include>、<selectKey>，加上动态 sql 的 9 个标签，其中<sql>为 sql 片段标签，通过<include>标签引入 sql 片段，<selectKey>为不支持自增的主键生成策略标签。

#### 11、一级、二级缓存

**一级缓存**：

1. 一级缓存(local cache), 即本地缓存, 作用域默认为 sqlSession。当  Session flush 或 close 后, 该 Session 中的所有 Cache 将被清空。
2. 本地缓存不能被关闭, 但可以调用 clearCache() 来清空本地缓存, 或者改变缓存的作用域.
3. 在mybatis3.1之后, 可以配置本地缓存的作用域. 在 mybatis.xml 中配置
4. 一级缓存的工作机制：同一次会话期间只要查询过的数据都会保存在当前 SqlSession 的一个 Map 中 key: hashCode+ 查询的 SqlId+ 编写的 sql 查询语句+参数。
5. 一级缓存失效的几种情况:
    1. 不在同一个 SqlSession 对象中。
    2. 执行语句的参数不同。缓存中也不存在数据。
    3. 执行增，删，改，语句，会清空掉缓存
    4. 手动清空缓存数据。

**二级缓存：**

1. 二级缓存(second level cache)，全局作用域缓存。
2. 二级缓存默认不开启，需要手动配置。
    1. 我们需要在mybatis的核心配置文件中配置setting选项：`<setting name="cacheEnabled" value="true"/>`
    2. 在Mapper的配置文件中加入cache标签：`<cache></cache>`
    3. 并且需要被二级缓存的对象必须要实现 java 的序列化接口，即实现 Serializable 接口。
3. MyBatis 提供二级缓存的接口以及实现，缓存实现要求 POJO 实现 Serializable 接口。
4. 二级缓存在 SqlSession 关闭或提交之后才会生效。

#### 12、Mybatis 是否支持延迟加载？如果支持，它的实现原理是什么？

Mybatis 仅支持 association 关联对象和 collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的就是一对多查询。在 Mybatis 配置文件中，可以配置是否启用延迟加载 lazyLoadingEnabled=true|false。

它的原理是，使用 CGLIB 创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用 a.getB().getName()，拦截器 invoke()方法发现 a.getB()是 null值，那么就会单独发送事先保存好的查询关联 B 对象的 sql，把 B 查询上来，然后调用 a.setB(b)，于是 a 的对象 b 属性就有值了，接着完成 a.getB().getName()方法的调用。这就是延迟加载的基本原理。

#### 13、Mybatis 映射文件中，如果 A 标签通过 include 引用了 B 标签的内容，请问，B 标签能否定义在 A 标签的后面，还是说必须定义在 A 标签的前面？

虽然 Mybatis 解析 Xml 映射文件是按照顺序解析的，但是，被引用的 B 标签依然可以定义在任何地方，Mybatis 都可以正确识别。

原理是，Mybatis 解析 A 标签，发现 A 标签引用了 B 标签，但是 B 标签尚未解析到，尚不存在，此时，Mybatis 会将 A 标签标记为未解析状态，然后继续解析余下的标签，包含 B 标签，待所有标签解析完毕，Mybatis 会重新解析那些被标记为未解析的标签，此时再解析 A 标签时，B 标签已经存在，A 标签也就可以正常解析完成了。

#### 14、如何执行批量插入？

#### 15、如何获取自动生成的主键值

#### 16、在 mapper 中如何传递多个参数?

- 第一种：DAO 层的函数。

```java
public UserselectUser(String name,String area);
```

对应的 xml, #{0}代表接收的是 dao 层中的第一个参数，#{1} 代表 dao 层中第二参数，更多参数一致往后加即可。

```xml
<select id="selectUser"resultMap="BaseResultMap">
    select * fromuser_user_t whereuser_name = #{0}
    anduser_area=#{1}
</select>
```

- 第二种：使用 @param 注解

```java
public interface usermapper {
    user selectuser(@param(“username”) String username,
                    @param(“hashedpassword”) String hashedpassword);
}
```

然后,就可以在 xml 像下面这样使用(推荐封装为一个 map, 作为单个参数传递给mapper):

```xml
<select id=”selectuser” resulttype=”user”>
    select id, username, hashedpassword
    from some_table
    where username = #{username}
    and hashedpassword = #{hashedpassword}
</select>
```

- 第三种：多个参数封装成 map

```

```

#### 17、Mybatis 动态 sql 有什么用？执行原理？有哪些动态 sql？

Mybatis 动态 sql 可以在 Xml 映射文件内，以标签的形式编写动态 sql，执行原理是根据表达式的值 完成逻辑判断并动态拼接 sql 的功能。

- if 标签：if 语句可以动态地添加查询条件。

```xml
<if test="lastName != null">
  last_name like concat ('%',#{lastName},'%')
</if>
```

- where 标签：可以帮我们在多个动态语句中，有效的去掉前面的多余的 and  或 or 之类的多余关键字。

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

- trim 标签：trim 可以动态在包含的语句前面和后面添加内容。也可以去掉前面或者后面给定的内容。

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

- choose when otherwise 可以执行多路选择判断，但是只会有一个分支会被执行。类似switch case 语句。

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

- set 语句：删除条件后的逗号。

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

- foreach 语句：类似于 select * from t_user where id in (1,3,4)。

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

#### 18、Mybatis 的 Xml 映射文件中，不同的 Xml 映射文件，id 是否可以重复？

不同的 Xml 映射文件，如果配置了 namespace，那么 id 可以重复；如果没有配置 namespace，那么 id 不能重复；

原因就是 namespace+id 是作为 Map<String, MapperStatement>的 key使用的，如果没有 namespace，就剩下 id，那么，id 重复会导致数据互相覆盖。有了 namespace，自然 id 就可以重复，namespace 不同，namespace+id 自然也就不同。

#### 19、一对多、多对一关联查询

#### 20、使用 MyBatis 的 mapper 接口调用时有哪些要求？

1. 对应的 Mapper 配置文件的 namespace 属性值必须是 Mapper 接口的全类名。比如，com.xuwei.mapper.UserMapper

2. Mapper 接口的方法名必须与 mapper 配置文件对应的 id 值相同。

3. Mapper 接口的方法参数类型必须与 mapper 配置文件中的 parameterType 类型匹配上。

4. Mapper接口的方法返回值类型必须与mapper配置文件中配置的 resultType 类型匹配上。

