# Servlet和jsp学习指南要点
Java Servlet技术，或简称Servlet，是由Java中用于开发Web应用程序的基本技术。Sun公司于1996年发布了Java Servlet技术，于CGI（Common GateWay Interface，公共网关接口）形成竞争。之后，它成为在Web中生成动态内容的标准。与CGI的对比：
- CGI：它为每一个HTTP请求都创建一个新的进程。因为创建新的进程需要花费大量的CPU周期，这使得编写可扩展的CGI程序变得极为困难。  
- Servlet：Servlet程序比CGI程序运行得更快，这是因为Servlet执行完它的第一个请求之后，就会驻留在内存中，等待后续的请求。  
## 一、Servlet
### 1、Servlet API概述
　　Servlet API中有4个Java包，包括：  
　　- javax.servlet。包定义了Servlet与Servlet容器之间契约的类和接口。  
　　- javax.servlet.http。包定义Servlet与Servlet容器之间契约的类和接口。  
　　- javax.servlet.annotation。包含对Servlet、Filter和Listener进行标注的注解。它还为标注元件元件指定元数据。  
　　- javax.servlet.descriptor。包含为Web应用程序的配置信息提供编程式访问的类型。
　　**javax.servlet包**  
　　javax.servlet中的主要类型：  
　　接口：Servlet、ServletRequest、ServletResponse、ServletContext、ServletConfig、RequestDispatcher、Filter  
　　抽象类：GenericServlet（基础于Servlet）  
　　Servlet技术的核心是Servlet接口，这是所有Servlet类都必须直接或间接实现的一个接口。Servlet接口定义了Servlet与Servlet容器之间的一个契约。这个契约归结起来说，Servlet容器会把Servlet类加载到内存中，并在Servlet实例中调用特定的方法。在一个应用程序中，每个Servlet类型只能有一个实例。  
　　用户的请求会引发Servlet容器调用一个Servlet的service方法，并给这个方法传入一个ServletRequest实例和一个ServletResponse实例。ServletRequest封装当前的HTTP请求，以便Servlet的开发中不必解析和操作原始的HTTP数据。ServletResponse表示当前用户的HTTP响应，它的作用是使得将响应回传给用户更容易。  
　　Servlet容器还为每个应用程序创建一个ServletContext实例。每个Servlet实例还有一个封装Servlet配置信息的ServletConfig。  
### 2、Servlet
　　**Servlet接口中定义了以下5个方法：**  
```java
void init(ServletConfig config) throws ServletExcept;
void service(ServletRequest request, ServletResponse responce) throws ServletExcept, java.io.IOExcept;
void destory();
ServletConfig getServletConfig;
```
　　init、service和destroy方法属于Servlet生命周期方法。Servlet容器将根据以下原则调用这三个方法：  
　　- init：第一次请求Servlet时，Servlet容器就会调用这个方法。在后续的请求中，将不再调用这个方法，Servlet容器会传递一个ServletConfig。一般来说，会将ServletConfig赋给一个类级别变量，以便Servlet类中其他方法也可以使用这个对象。
　　- service：每次请求Servlet时，容器都会调用这个方法，必须在这里编写要Servlet完成的相应代码。第一次请求Servlet时，Servlet容器会调用init方法和service方法。对于后续的请求，则只调用service方法。  
　　- destroy：要销毁Servlet时，Servlet容器就会调用这个方法。它通常发生在卸载应用程序，或者关闭Servlet容器的时候。一般来说，可以在这个方法中编写一些资源清理相关的代码。
　　- getServletINFO：该方法返回Servlet的描述。可以返回可能有用的任意字符串，甚至可能是null。  
　　- getServletConfig：该方法返回由Servlet容器传给init方法的ServletConfig。但是，为了让getServletConfig返回非null值，你肯定已经为init方法的ServletConfig赋给了一个类级变量。  
　　必须注意的一点是线程安全性。一个应用程序中的所有用户将共用一个Servlet实例，因此不建议使用类级别变量，除非它们是只读的，或者是java.util.concurrent.atomic包中的成员    
