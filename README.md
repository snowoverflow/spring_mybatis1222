# spring_mybatis
spring和mybatis整合
# 1.整合思路 #
需要spring通过单例方式管理SqlSessionFactory。
spring和mybatis整合生成代理对象，使用SqlSessionFactory创建SqlSession。（spring和mybatis整合自动完成）
持久层的mapper都需要由spring进行管理。
# 2.sqlSessionFactory #
在applicationContext.xml配置sqlSessionFactory和数据源

sqlSessionFactory在mybatis和spring的整合包下。

    <!-- 加载配置文件 -->
	<context:property-placeholder location="classpath:db.properties" />

	<!-- 数据源，使用dbcp -->
	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName" value="${jdbc.driver}" />
		<property name="url" value="${jdbc.url}" />
		<property name="username" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
		<property name="maxActive" value="10" />
		<property name="maxIdle" value="5" />
	</bean>


	<!-- sqlSessinFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 加载mybatis的配置文件 -->
		<property name="configLocation" value="mybatis/SqlMapConfig.xml" />
		<!-- 数据源 -->
		<property name="dataSource" ref="dataSource" />
	</bean>
# 3.原始dao开发（和spring整合后） #
### User.xml ###

    <!-- namespace命名空间，作用就是对sql进行分类化管理，理解sql隔离 
注意：使用mapper代理方法开发，namespace有特殊重要的作用
-->
<mapper namespace="test">

	<!-- 在 映射文件中配置很多sql语句 -->
	<!-- 需求：通过id查询用户表的记录 -->
	<!-- 通过 select执行数据库查询
	id：标识 映射文件中的 sql
	将sql语句封装到mappedStatement对象中，所以将id称为statement的id
	parameterType：指定输入 参数的类型，这里指定int型 
	#{}表示一个占位符号
	#{id}：其中的id表示接收输入 的参数，参数名称就是id，如果输入 参数是简单类型，#{}中的参数名可以任意，可以value或其它名称
	
	resultType：指定sql输出结果 的所映射的java对象类型，select指定resultType表示将单条记录映射成的java对象。
	 -->
	<select id="findUserById" parameterType="int" resultType="cn.itcast.ssm.po.User">
		SELECT * FROM USER WHERE id=#{value}
	</select>
	
	
</mapper>

###  ###