Java 面试
====

## 一.java基础知识

### ***1.面向对象***
以对象作为基本单位 用类作为模板来描述他的属性和行为 一共有三个特性 继承 封装和多态
#####
 1.继承 java中的类只能单继承 一个子类只能有一个父类 一个父类能有多个子类 接口不属于继承范畴 一个类可以实现多个接口
#####
 2.封装 通过封装类的某些属性 来保护这些属性 让其他人想修改其中属性时只能通过set/get方法 我们通过定义 这两个方法来控制该属性的访问权限
#####
 3.多态 主要有三种表现形式 方法重载 重写（编译时多态） 向上转型 （运行时多态）

### ***2.JDK JRE JVM***
###
1.JDK Java开发工具包 包含运行环境的JRE java的基础类库 java的一些工具包
###
2.JRE JAVA程序的运行环境 所有的程序必须依赖jre才能运行
###
3.JVM jre的一部分 只关心 Java源程序编译出的字节码文件 jvm是程序实现跨平台的核心 

***3.==和equals***
==是运算符 如果是基本数据类型 则会比较数据存储的值 如果是引用类型 则比较地址值
###
equals是Object方法 比较的是对象的地址值 一般会重写 比如String类
（源码需要分析）

### ***4.String***
String s = new String ("abc")
###
一共创建了两个对象 "abc" 和 new String 存在两层引用关系
###
String 底层是char数组 可以直接赋值字符串
###
String Buffer,String builder必须通过new对象的形式装值
###
String 底层是char数组 可以直接赋值字符串
###
String 拼接直接通过+，并且拼接操作是生成了新的字符串，原有的字符串还在。
###
String Buffer,String builder必须通过append，不会产生新的字符串，优势在于不会额外占据新的内存空间 节约内存
###
String Buffer 线程安全 执行效率低 底层是以synchronized锁修饰
###
String builder 线程不安全 执行效率高

### ***4.接口和抽象类***
###
(1). 结构不同：
###
接口四大结构：常量、抽象方法、静态方法、默认方法
###
抽象类六大结构：成员变量、方法、构造方法、代码块、内部类、抽象方法
###
(2). 实现形式不同：
###
接口：被实现类多实现，并重写抽象方法实现具体功能(功能扩展性更强)
###
抽象类：被子类单继承，并重写抽象方法实现具体功能


###
 (3). 代码简化形式不同：
###
接口：接口中的结构可以适当的进行简化编写
###
抽象类：抽象类中的结构不可以简化编写
###
### ***5.ArrayList和LinkedList区别***
###
1.ArrayList的底层是数组 LinkedList底层是双端链式结构 对头部和尾部都可以操作
###
2.ArrayList 向集合中间插入或删除数据时效率比较低 查询效率比较高（因为数组是一块连续的空间 程序先找到数组的首地址 然后根据下标算出该下标元素在内存中的位置 所以不需要一个个去遍历）
###
LinkedList 向集合中间插入或者删除数据时效率比较高（只需要将上一个元素的指针指向下一个元素即可） 查询的效率比较低（由于链表中的元素不是连续存储的 要想访问链表的某个元素 需要从头节点依次遍历）
### ***6.List和Set的区别***
###
1.List集合：数据可重复存储，数据有序(可利用索引操作数据)(Collection接口的子接口)
###
2.Set集合：数据不可重复存储，数据无序(Collection接口的子接口)
### ***7.HashMap和HashTabe的区别***
###
1.从功能角度来说
###
1.HashTable线程安全（采用全局锁）性能稍差  HashMap线程不安全 性能好
###
2.从内部实现角度来说
###
HashTable 使用数组加链表、HashMap 采用了数组+链表+红黑树
###
HashMap 初始容量是 16、HashTable 初始容量是 11
###
HashMap 可以使用 null 作为 key，HashMap 会把 null 转化为 0 进行存储，而 Hashtable 不允许
###
最后，他们两个的 key 的散列算法不同，HashTable 直接是使用 key 的 hashcode 对数组长度做取模
###
而 HashMap 对 key 的 hashcode 做了二次散列，从而避免 key 的分布不均匀问题影响到查询性能
###
### ***8.concurrentHashMap的整体架构***
1.由数组 链表 红黑树组成
###
2.初始化实例时 会初始话一个默认长度为16的数组 
###
3.采用链式寻址法来解决hash冲突
###
4.冲突比较多的时候造成链表长度较长 最后变成O(n)
###
5.当数组长度大于64并且链表长度大于等于8的时候 就会转换为红黑树
###
6.但是如果动态扩容之后链表长度小于8红黑树会退化成单向链表
###
7.功能和hashmap一样 在基础上提供了并发安全的实现（对指定的node节点加锁）
###
8. jdk1.8 锁的粒度是一个节点 而1.7版本是锁一个范围segment 性能上更低
###
9.引入红黑树之后 数据查询的时间复杂度变成了O（logn）
###
10.数组长度不够时（什么时候数组长度会不够？） 需要扩容，concurrentHashMap引入了多线程并发扩容机制，多个线程对原始数组进行分片后，每个线程负责一个分片的数据迁移，从而提升了扩容过程数据迁移的效率
###
11.在多线程并发场景中 保证原子性的前提下实现元素个数的增加 性能是非常低的
优化 ： 线程竞争不激烈时 直接采用CAS来实现元素个数的原子递增  线程竞争激烈时 使用一个数组来维护元素个数，如果要增加总的元素个数，则直接从数组中随机选择一个 再通过CAS实现原子递增
### ***9.CAS***
###
1.java中unsafe类里面的方法 全称是CompareAndSwap 比较并变换
###
2.  多个线程同时对一个数进行修改 就会出现原子性的问题（多个插入和删除同时进行就有可能丢失数据）
###
3.加同步锁可能会带来性能上的损耗
调用compareAndSwapInt()方法 比较内存地址偏移量对应的值和传入的预期值是否相等 如果相等 则直接修改内存中的值 否则返回false
###
4.但是这种情况仍然会出现原子性问题 先取值 再比较 最后修改
###
5.所以在底层 会增加Lock指令对缓存或总线加锁 保证原子性
### ***9.CAS***
### ***10.final***
被final修饰的类不可以被继承
被final修饰的方法不可以被重写
被final修饰的变量不可以被改变，被final修饰不可变的是变量的引用，而不是引用指向的内容，
引用指向的内容是可以改变的
### ***11.break ,continue ,return 的区别及作用***
break 跳出总上一层循环，不再执行循环(结束当前的循环体)
continue 跳出本次循环，继续执行下次循环(结束正在执行的循环 进入下一个循环条件)
return 程序返回，不再执行下面的代码(结束当前的方法 直接返回)
### ***12.java 中 IO 流分为几种***
按照流的流向分，可以分为输入流和输出流；
按照操作单元划分，可以划分为字节流和字符流；
按照流的角色划分为节点流和处理流
### ***13.BIO,NIO,AIO 有什么区别***
BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方
便，并发处理能力低。
NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。NIO中的N可以理解为Non-
blocking，不单纯是New。它支持面向缓冲的，基于通道的I/O操作方法。 NIO提供了与传统
BIO模型中的 Socket 和 ServerSocket 相对应的 SocketChannel 和
ServerSocketChannel 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模
式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模
式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞I/O来提升开发速率和
更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发
AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。
### ***13.Files的常用方法都有哪些***
Files. exists()：检测文件路径是否存在。
Files. createFile()：创建文件。
Files. createDirectory()：创建文件夹。
Files. delete()：删除一个文件或目录。
Files. copy()：复制文件。
Files. move()：移动文件。
Files. size()：查看文件个数。
Files. read()：读取文件。
Files. write()：写入文件。
### ***14.反射***
比如我封装一个select方法 用户可以通过这个方法查数据库里的任意一张表 但是用户在使用这个方法之前并不确定去查某一个表，比如用户想查商品表，这个方法就会传进一个标识，根据这个标识去创建一个查询商品表对象去查询商品表，就像我们目前项目的RFC APPLICATION 上游指定调用某个RFC 然后我们这边动态去创建该对象去完成业务操作。告诉我你要什么，我们就可以通过反射去创建每个类其实都有与其对应的class对象 ，正常创建一个对象是要通过这个对象去完成某个业务，而class对象（也就是说镜像对象）是为了得到某个类的数据成员名，方法和构造器以及这个类到底实现了哪些接口，当我们获取到类的镜像对象之后，我们就获得了这个类的所有东西 就能够通过这个对象去new新的对象反射对象获取非私有的属性可以通过getFileds 如果想获取私有的属性可以通过getDeclaredFileds
Java获取反射的三种方法
1.通过new对象实现反射机制 stu.getClass()
2.通过路径实现反射机制 Class.forName
3.通过类名实现反射机制 Student.class
### ***15.Integer a= 127 与 Integer b = 127相等吗***
对于对象引用类型：==比较的是对象的内存地址。
对于基本数据类型：==比较的是值
### ***16.HashMap的插入算法***
1.首先判断存放key的数组是否为空，如果为空 则初始化该数组
2.如果传进来的key不是空 先算hashcode 再通过数组长度和hashcode算出数组的下标 &运算 （h&length-1）保证数组的长度不会越界的同时 增加随机性
3.hashmap在put的时候 如果出现相同的key覆盖的情况下 覆盖操作会返回之前该key对应的value
4.如果没有相同key 再插入 头插法（需要控制当前链表的头节点元素是数组当前位置上的元素，因此插入后全体下移 ) 先把新节点的next指向原来的头节点 然后把新元素的引用地址赋值给i
5.hashmap最初的容量是16 因为16（0001 0000）&之后不会浪费数组的空间 比如17 算出的值总是0或16 扩容一定是2的幂次方 默认扩容阈值是12

## 2.Java多线程
######
### ***1. Java 程序中怎么保证多线程的运行安全***
出现线程安全问题的原因一般都是三个原因：
线程切换带来的原子性问题 解决办法：使用多线程之间同步synchronized或使用锁(lock)。
缓存导致的可见性问题 解决办法：synchronized、volatile、LOCK，可以解决可见性问题
编译优化带来的有序性问题 解决办法：Happens-Before 规则可以解决有序性问题
### ***2. Java 线程创建的方法***
1.继承Thread类（本身就实现runnable接口） 重写run方法
2.实现runnable接口 重写run方法
3.实现Callable 重写call方法 配合FearureTask（也是实现runnable接口） 将其放入thread内
4.基于线程池 线程池的worker类底层也是实现了runnable接口 构建线程
### ***3. Java 线程的状态***
1.NEW:Thread对象被创建出来 但是还没有执行start方法
2.RUNNABLE：调用了start方法 
3.BLOCKED：synchronized没有拿到同步锁 被阻塞的情况
4.WAITING：调用了wait方法 需要被手动唤醒
5.TIMED_WAITING：调用sleep或join方法 会被自动唤醒 无需手动唤醒
6.TERMINATED：run方法执行完毕 线程声明周期结束
### ***4. Java 终止线程***
1.stop方法
2.使用共享变量
3.interrupt
线程默认情况下 interrupt的标记位为false 可以使用isInterrupted来判断标记位状态 然后使用.interrpt
或者直接使用interrupted 先归位为true 后标记为false
 







## 3.Spring
######
### ***1. Spring如何创建一个bean对象***
1.class文件 –》 推断构造方法（一个类里有多个构造方法 spring优先使用无参的构造方法，其次可以用注解@autowired指定使用哪个构造方法 传对象的时候 先bytype 再byname） –》普通对象 –》依赖注入（spring会将普通对象加了@auto wired注解的属性赋值）—》初始化（执行afterPropertiesSet）—》初始化后 AOP—》代理对象—》放入Map<beanName,bean对象（代理对象）> 单例池—》bean对象
### ***2. Spring的设计模式***
1.单例模式 
是以bean的名字是单例bean 默认在一个JVM里 对象只创建一次到不用了再销毁避免过程中频繁创建对象
Spring的依赖注入 包括懒加载方式 都是发生在AbstractBeanFactory的getBean里，getBean方法调用getSingleton进行bean创建 
单例模式定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点
Spring中的单例模式完成了后半句话，即提供了全局的访问点BeanFactory
我们可以通过@bean 再次向容器里添加该类型的bean 所以他是传入的名字一样 查到的对象就是一样的
2.工厂模式
工厂模式  bean的创建交给工厂去实现 而Spring容器管理这个工厂 beanFactory ,ApplicationContext 去get一个bean
3.代理模式
实现方式：AOP底层，就是动态代理模式的实现。
         动态代理：在内存中构建的，不需要手动编写代理类  
         静态代理：需要手工编写代理类，代理类引用被代理对象。
切面在应用运行的时刻被织入。一般情况下，在织入切面时，AOP容器会为目标对象创建动态的创建一个代理对象。SpringAOP就是以这种方式织入切面的
4.模板模式 
整合第三方框架 Spring模板方法模式实质：是模板方法模式和回调模式的结合，是Template Method不需要继承的另一种实现方式。Spring几乎所有的外接扩展都采用这种模式。
###
具体实现：JDBC的抽象和对Hibernate的集成，都采用了一种理念或者处理方式，那就是模板方法模式与相应的Callback接口相结合。
采用模板方法模式是为了以一种统一而集中的方式来处理资源的获取和释放
父类定义了骨架（调用哪些方法及顺序），某些特定方法由子类实现。
最大的好处：代码复用，减少重复代码。除了子类要实现的特定方法，其他方法及方法调用顺序都在父类中预先写好了。
###
5.策略模式 加载不同地方的文件  抽取了resource接口
###
6.适配器模式
实现方式：SpringMVC中的适配器HandlerAdatper，HandlerAdatper根据Handler规则执行不同的Handler	     
实现意义：HandlerAdatper使得Handler的扩展变得容易，只需要增加一个新的Handler和一个对应的HandlerAdapter即可。因此Spring定义了一个适配接口，使得每一种Controller有一种对应的适配器实现类，让适配器代替controller执行相应的方法。这样在扩展Controller时，只需要增加一个适配器类就完成了SpringMVC的扩展了
###
7.装饰器模式
将原生类的对象作为参数传到装饰类里进行进一步处理
实现方式：Spring中用到的包装器模式在类名上有两种表现：一种是类名中含有Wrapper（querrywrapper动态条件查询），另一种是类名中含有Decorator
通过AOP 来为bean添加额外的功能
###
 8.观察者模式
实现方式：Spring的事件驱动模型使用的是观察者模式 ，Spring中Observer模式常用的地方是listener的实现
具体实现：事件机制的实现需要三个部分，即：事件源、事件、事件监听器
1.ApplicationEvent抽象类[事件]通过继承它，实现自定义事件。另外，通过它的 source 属性可以获取事件源，timestamp 属性可以获得发生时间
2.ApplicationListener接口[事件监听器]
3.ApplicationContext接口[事件源]通过实现它，来监听指定类型事件并响应动作
###
9.MVC view 收集和展示数据
      Controller 接收和处理用户请求
          Model 数据访问业务处理
### ***3.@Autowired 和 @Resource的区别 ***
### **** 共同点 ****
1.从容器中获取相关对象
###
2.添加在属性上面后可以自动将容器中相关对象注入到该成员变量上
###
3.两者都可以写在字段和setter方法上
### **** 不同点 ****
1.@Autowired 
###
Spring平台提供
###
需要导入依赖 只能按照bytype注入
###
如果要按byName注入的话需要与@Qualififier（名称）结合
###
2.@Resource
###
J2EE提供
###
按照名称自动注入
###
Spring 将@Resource注解的name属性解析为bean的名字 而type属性则解析为bean的类型 
###
所以如果使用name属性则使用byName的自动注入策略
###
如果使用type属性 则使用byType自动注入策略 如果既不指定name 也不指定type 则通过反射机制使用byName
###
装配顺序
###
1.如果同时指定了name和type 则从Spring上下文中找到唯一的bean去进行装配 找不到报异常
###
2.如果指定了name 则在上下文中查找名称 id匹配的bean进行装配 找不到报出异常
###
3.如果指定了type则从上下文中找出类似匹配的唯一bean进行装配 如果找不到或者找到多个 则报出异常
###
4.如果既没有指定name 又没有指定type 则按照byName方式进行装配 如果没有匹配 则会退回一个原始类型进行匹配 如果匹配则自动装配
### ***4.常用注解 ***
1.@Controller @Service @RestController
###
2.@RequestBody
###
3.@Indexed
###
4.@Import
### ***5.循环依赖 ***
1.A对象 里又有一个A对象
2.A依赖于B B依赖于A
3.A依赖于B B依赖于C C依赖于A


## 4.MySQL
######
### ***1. 分类***
1.DDL 数据定义语言 定义数据库对象（数据库 表 字段）
2.DML 数据操作语言 对数据库表中进行增删改
3.DQL 数据查询语言 查询数据库中表的记录
4.DCL 数据控制语言 创建数据库用户 控制数据库的访问权限
### ***2. DQL执行顺序***
1.编写顺序 SELECT 字段列表 FROM 表名列表 WHERE 条件列表 GROUP BY 分组字段列表  HAVING 分组后条件列表 ORDER BY 排序字段列表 LIMIT 分页参数
###
2.执行顺序 1.FROM 2.WHERE 3.GROUP BY 4.HAVING 5.SELECT 6.ORDER BY 7.LIMIT
### ***3. DCL 管理用户***
1.查询用户 
USE mysql  
SELECT * FROM user
###
2.创建用户
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码'（如果选择在任意主机上访问数据库 主机名变成%）
###
3.修改用户的密码
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password by '密码'
###
4.删除用户名
DELETE USER '用户名'@'主机名'
### ***4. DCL 权限控制***
权限表 
   ALL
   SELECT
   INSERT
   UPDATE 
   DELETE
   ALTER
   DROP
   CREATE
   ###
1.查询权限
SHOW GRANTS FOR '用户名'@'主机名'
###
2.授予权限
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名'
###
3.撤销权限
REOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名'
### ***5. 字符串函数***
1.CONCAT(S1,S2,...,SN) 字符串拼接
###
2.LOWER(str) 将字符串str全部转为小写
###
3.UPPER(str) 将字符串str全部转为大写
###
4.LPAD(str,n,pad) 左填充 用字符串pad对ste的左边进行填充 达到n个字符串长度
###
5.RPAD(str,n,pad) 右填充 用字符串pad对ste的右边进行填充 达到n个字符串长度
比如将不足五位数的id全部在前面补0
UPDATE EMP SET ID = LPAD(ID,5,'0') 
###
6.TRIM(str) 去掉字符串头部和尾部的空格
如果要去除字符串中间的空格 可以使用REPLACE
###
7.SUBSTRING(str,start,len) 返回从字符串str从start位置起len个长度的字符串
### ***6. 数值函数***
1.CEIL(x) 向上取整
###
2.FLOOR(X) 向下取整
###
3.MOD(X,Y) 返回X/Y的模
###
4.RAND() 返回0-1内的随机数
###
5.ROUND(X,Y) 求参数x的四舍五入值 保留y位小数
### ***7. 日期函数***
NOW()	返回当前的日期和时间
CURDATE()	返回当前的日期
CURTIME()	返回当前的时间
DATE()	提取日期或日期/时间表达式的日期部分
EXTRACT()	返回日期/时间按的单独部分
DATE_ADD()	给日期添加指定的时间间隔
DATE_SUB()	从日期减去指定的时间间隔
DATEDIFF()	返回两个日期之间的天数
比如查询员工的入职天数 并按天数进行排序
SELECT NAME ,DATEDIFF(curdate(),entrydate) a from emp order by a
DATE_FORMAT()	用不同的格式显示日期/时间
### ***8. 流程函数***
![image](https://github.com/fushouhao/Java-InterView/assets/94434988/1ee7bc15-ecae-4607-a3ef-fb39b9ee7e1b)


