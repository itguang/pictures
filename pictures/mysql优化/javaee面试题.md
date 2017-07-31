# **Spring**

### **Spring是什么?**
**Spring是一个轻量级的IOC和AOP容器框架：**

* a,轻量级：程序实现不是很复杂，代码不是很多，占用资源不是很多，没有侵入性；

* b，IOC（Inversion of Control 控制反转）：对象创建责任的反转（重点，核心）；

* c, Aop(Aspect Oriented Programming):一种面向横切面编程的思想方式，可以进行功能性扩展，看前边的一篇转载的博客：面向横切面（AOP）编程
* d，容器：可以容纳对象，并且可以控制对象的生命周期；

## **1,Spring框架的优点都有什么？**

* 1,Spring是分层的架构，你可以选择使用你需要的层而不用管不需要的部分
* 2,Spring是POJO编程，POJO编程使得可持续构建和可测试能力提高依赖注入和IoC使得JDBC操作简单化
* 3,Spring是开源的免费的
* 4,Spring使得对象管理集中化合简单化

**Sping框架的优缺点：**

**优点：**

* 轻量级的容器框架，没有侵入性

* IoC更加容易组合对象之间的关系，通过面向接口进行编程，可以低耦合开发。

* 易于本地测试(Junit单元测试，不用部署服务器)

* AOP可以更加容易的进行功能扩展，遵循OCP开发原则。

* Spring默认对象的创建为单例的，我们不需要再使用单例的设计模式来开发单体类。

* Spring的集成很强大，另外可以对其他框架的配置进行一元化管理。

* Spring的声明式事务的方便使用。

[ Spring(一)——总体介绍](http://blog.csdn.net/liujiahan629629/article/details/20735407)

[]()

**如何实现AOP编程思想呢？**

实现这种编程思想的一个重要手段就是代理模式或者说模仿代理模式的运用。

尤其是其中动态代理模式，JDK提供的Proxy的使用
 而这种动态代理是基于接口的，也就是说代理对象和目标对象实现了同一个接口。而假如我们应用中没有使用接口，就无法使用Proxy了。但是不要着急，CGLIB这个组件解决了这个问题，它正是弥补jdk中的不足，专门基于继承来实现动态代理，其中代理对象是继承目标对象进行扩展的。



**Spring MVC工作流程描述:**

 1. 用户向服务器发送请求，请求被Spring 前端控制Servelt DispatcherServlet捕获；
      
 2. DispatcherServlet对请求URL进行解析，得到请求资源标识符（URI）。然后根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain对象的形式返回；
      
  3. DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter。（附注：如果成功获得HandlerAdapter后，此时将开始执行拦截器的preHandler(...)方法）
       
4.  提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。 在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：
      HttpMessageConveter： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息
      数据转换：对请求消息进行数据转换。如String转换成Integer、Double等
      数据根式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等
      数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中

5.  Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象；
      
6.  根据返回的ModelAndView，选择一个适合的ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给DispatcherServlet ；
      
7. ViewResolver 结合Model和View，来渲染视图
      
8. 将渲染结果返回给客户端。

# **Spring MVC**

## **spring MVC原理**

http://blog.csdn.net/xtu_xiaoxin/article/details/8796499

# **Struts2+Spring集成合并**
http://blog.csdn.net/liujiahan629629/article/details/20737593


# **Struct2**

## **Struts2(一)——总体介绍**

http://blog.csdn.net/liujiahan629629/article/details/20564637

> **根据解析请求路径，获取action的名称，到配置文件中查找到action的完整类名，反射创建。
Action类设置要求：必须提供public的无参数的构造方法。Action对象是每次请求都创建一个新的实例，所以action是多例的，它解决的线程安全问题。（但是性能不高）这些都是基于框架的源码的设置。**


# **Hibernate**

## **Hibernate框架（一）——总体介绍**

http://blog.csdn.net/liujiahan629629/article/details/21442607


## **Hibernate(二)——POJO对象的操作**

http://blog.csdn.net/liujiahan629629/article/details/21474525

## **Hibernate(四)——缓存策略+lazy**


http://blog.csdn.net/liujiahan629629/article/details/21488239


# **Struts2+Spring+Hibernate 三大框架的合并集成**

http://blog.csdn.net/liujiahan629629/article/details/21562597