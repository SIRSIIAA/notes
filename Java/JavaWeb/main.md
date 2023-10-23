# Java Web Main(From Zero)

## Little Little Little

1. deactivation,未激活状态
    1. activation
    2. frame deactivation,指当前窗口失去焦点
2. Facet,小平面，事物的一部分
    1. Facets
3. Let,指Java Applet,古早时Java的小应用程序,后延用
    1. Servlet，服务器端的小应用程序

4. Facade,虚假的外表、表面,在建筑学中还指建筑物的正面、立面、外观

5. Protocol,协议，协议即约定

## Tomcat

### Tips

1. Tomcat是容器
2. Tomcat的版本与JDK的版本相关联
3. Tomcat以C、Java写成
4. 环境变量配置

### Directory Architecture

1. bin—binary，可执行二进制脚本
2. conf—configuration，配置文件
3. lib—library，依赖
4. logs—运行日志
5. webapps—部署的项目文件
    1. webapps下的一层目录作为项目的context root（项目根目录）
6. work—已部署的项目运行时的工作目录

### Deployment

1. artifact，构建的产品，部署的软件单元
2. application context，Directory Architecture>5.>webapps下的context root

### Runtime

1. Tomcat Server 运行于localhost:8080，默认使用http协议
2. 可以通过Tomcat Server运行的路径后接不同的context root来访问不同的webapp，也可以被称为artifact
    1. localhost:8080/java-web-demo

### Init Java web Project with IDEA2023.2+

1. New Project>Build System:Itellij
2. Files>Project Structure>Modules>Web>Web:Create Artifact
3. Edit Configurations>Configure Tomcat & JRE
4. Project Structure>Libraries:Add Tomcat\lib

### Servlet

#### Questions

1. service要抛出的两个异常ServletException以及IOException都是什么意思？

#### Servlet、Tomcat Container

##### MyServlet—>HttpServlet—>GenericServlet—>Servlet

interface:javax.servlet.Servlet

Servlet的顶层接口，定义了Servlet应该具有的行为模式

Servlet聚合了三个其他的接口，ServletConfig以及ServletRequest、ServletResponse

```java
void init(ServletConfig var1);//一个Servlet需要以一个ServletConfig初始化实现
ServletConfig getServletConfig();//获取ServletConfig对象
void service(ServletRequest var1,ServletResponse var2) throws ServletException,IOException;//服务提供方法
String getServletInfo();//toString
void destory();//销毁
```

interface:javax.servlet.GenericServlet **extends** interface:javax.servlet.Servlet

abs-class:javax.servlet.HttpServlet **implements**interface:javax.servlet.GenericServlet

HttpServlet作为一个抽象类，为doPost等方法提供了默认实现，如果子类不重写，则调用这些默认方法，这些默认方法将默认返回405



##### Tomcat Container

Servlet生命周期：init、service、destory 

Tomcat容器通过反射的方式建立Servlet单例，其创建时机为，首次接受到Servlet映射的URL请求时，后续此Servlet单例将常驻容器内部（？）

web.xml配置`<load-on-startup>`标签可以更改Servlet的创建时机，minOccur为0，值越小，越早创建

Servlet实例的service方法未添加任何线程安全控制手段，因此线程不安全

#### Hyper Text Transfer Protocol

超文本传输协议

HTTP1.0以及HTTP1.1

##### Request

1. 请求行，描述请求方式、请求URL、HTTP协议版本
2. 请求头，告知服务器，客户端的信息，包括但不限于：Accept—客户端可以接受的媒体类型，
3. 请求体
