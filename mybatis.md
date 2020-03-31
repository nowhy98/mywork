1

Mybatis 动态 SQL ，可以让我们在 XML 映射文件内，以 XML 标签的形 式编写动态 SQL ，完成逻辑判断和动态拼接 SQL 的功能。

主要的动态语句有<if/>、<foreach>、<choose/>、<trim/>、<when>、<set/>等

其执行原理为，使用 OGNL 的表达式，从 SQL 参数对象中计算表达式的值,根据表达式的值动态拼接 SQL ，以此来完成动态 SQL 的功能。

2

mybatis支持延迟加载，主要输通过动态代理实现的，通过代理拦截到制定方法，执行数据加载。

3

主要有simpleExecutor、reuseExecutor、BatchExecutor他们的父类是BaseExecutor,另外还有CachingExecutor，他的父类直接是Executor。

他们的主要区别是：

**SimpleExecutor**: 简单执行器，是MyBatis中默认使用的执行器，每执行一次update或select，就开启一个Statement对象，用完就直接关闭Statement对象(可以是Statement或者是PreparedStatment对象)。

**ReuseExecutor**: 可重用执行器，这里的重用指的是重复使用Statement，它会在内部使用一个Map把创建的Statement都缓存起来，每次执行SQL命令的时候，都会去判断是否存在基于该SQL的Statement对象，如果存在Statement对象并且对应的connection还没有关闭的情况下就继续使用之前的Statement对象，并将其缓存起来。因为每一个SqlSession都有一个新的Executor对象，所以我们缓存在ReuseExecutor上的Statement作用域是同一个SqlSession。

**BatchExecutor**: 批处理执行器，用于将多个SQL一次性输出到数据库

**CachingExecutor**: 缓存执行器，先从缓存中查询结果，如果存在，就返回；如果不存在，再委托给Executor delegate 去数据库中取，delegate可以是上面任何一个执行器

4

mybaits一级缓存是时sqlSession级别的，主要存在于本地缓存中，当进行增删改等操作时，一级缓存会被清除。不同sqlSession之间的缓存互不影响。

二级缓存是Mapper级别的，多个sqlSession 可以去使用Mapper查询是会公用一个缓存，二级缓存可以使用第三方工具，入redis等，mybatis中有相关的包。并且二级缓存存的是序列化后的对象，并不是原对象。

5

mybatis插件的实现原理类似javaweb中的拦截器，主要采用动态代理方式。主要拦截四大对象：Executor、StatementHandler、ParameterHandler和ResultSetHandler。通过包装对象添加额外内容，是对象更强大。

使用@Intercepts，@Signature注解并实现Interceptor来编写一个插件类，另外还要在sqlM.xml中添加拦截类的全路径

<plugins>

​	<plugin interceptor="全路径"></plugin>

</plugins>