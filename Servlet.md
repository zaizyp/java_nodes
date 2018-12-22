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
### 3、ServletRequest
　　对于每一个HTTP请求，Servlet容器都会创建一个ServletRequest实例，并将它传给Servlet的service方法。ServletRequest封装有关请求的信息。  
　　下面是ServletRequest接口中的部分方法：
　　`public int getContextLength()`  
　　返回请求主体中的字节数。如果不知福字节的长度，该方法将返回-1  
　　`public java.lang.String getContentType()`  
　　返回请求主体的MIME类型，如果不知道类型，则返回null  
　　`public java.lang.String getProtocol()`
　　返回这个 HTTP请求的协议名称和版本号  
　　`public String getParameter(String name)`  
　　是ServletRequest中最常用的方法，该方法通常用来返回一个HTML表单域中的值。  
### 4、ServletRequest
　　javax.servlet.ServletResponse 接口表示一个Servlet响应。在调用一个Servlet的service方法之前，Servlet容器会先创建一个ServletResponse对象，并将它作为第二个参数传给service方法。ServletResponse隐藏了将响应发给浏览器的复杂性。  
　　ServletResponse中定义的一个getWriter 方法，它返回可以将文本传给客户端的java.io.PrintWriter。在默认情况下，PrintWriter对象采用的是ISO-8859-1编码。  
　　在将响应发送给客户端时，通常将它作为HTML发送。  
　　提示：还有一个方法可以用来将输出传送给浏览器：getOutputStream。但是这个方法是用来传输二进制数据的，因此在大多数时候，需要使用getWriter。  
　　在发送任何HTML标签之前，应该先通过调用setContextType方法来设置响应的内容类型，比如，将text/html作为参数传递，这是在告诉浏览器内容类型为HTML。如果没有设置内容类型，那么大多数浏览器将会默认以HTML的形式显示响应的内容。但是，如果没有设置响应的内容类型，有些浏览器则会将HTML标签显示为普通文本。  
### 5、ServletConfig
　　在Servlet容器初始化Servlet时，Servlet容器将ServletConfig传给Servlet的init方法。ServletConfig封装可以通过@WebServlet或者部署描述符传给一个Servlet的配置信息。以这种方式传递的每一条信息都称作初始参数。初始参数有两个部分组成：键和值。  
　　为了从一个Servlet内部获取某个初始参数的值，应该在由Servlet容器传给Servlet的init方法的ServletConfig中调用getInitParameter方法。getInitParameter方法的签名如下：  
　　`String getParameter(String name)`  
　　此外。getInitParameterNames 方法则返回所有初始参数名称的一个Enumeration：  
　　`Enumeration<String> getInitParameterNames()`  
　　除了getInitParameter和getInitParameterNames之外，ServletConfig还提供了另一个很有用的方法：getServletContext。可以利用这个方法从Servlet内部获取ServletContext。  
### 6、ServletContext
　　ServletContext表示Servlet应用程序。每个Web应用程序只有一个Context。在分布式环境中，一个应用程序同时部署到多个容器中，并且每台Java虚拟机都有一个ServletContext对象。  
　　在ServletConfig中调用getServletContext方法可以获得ServletContext。  
　　有了ServletContext之后，就可以共享能通过应用程序的所有资源访问的信息，促进Web对象的动态注册，前者是通过将一个内部Map中的对象保存在ServletContext中来实现的。保存在ServletContext中的对象称作属性（attribute）。  
　　ServletContext中的下列方法是用于处理属性的：  
```java
java.lang.Object getAttribute(java.lang.String name);
java.util.Enumeration<java.lang.String> getAttributeNames();
void setAttribute(java.lang.String name, java.lang.Object object);
void removeAttribute(java.lang.String);
```
### 7、GenericServlet
　　实现了Servlet和ServletConfig，并完成以下工作：  
　　- 将init方法中的ServletConfig赋给了一个类级变量，使它可以通过调用getServletConfig来获取。  
　　- 为Servlet接口中的所有方法提供默认实现。  
　　- 提供方法来包装ServletConfig中的方法。  
### 8、HTTPServlet
　　我们所编写的Servlet应用程序，大多数都要用到HTTP。javax.servlet.http包是Servlet  API中的第二个包，其包含了编写Servlet应用程序的类和接口。javax.servlet.http中的许多类型覆盖了javax.servlet中的类型。  
　　javax.servlet.http中的主要类型：  
　　HttpServlet、HttpServletRequest、HttpServletResponse、HttpSession、Cookie  
#### 8.1、HttpServlet
　　HttpServlet类继承了GenericServlet类。在使用HttpServlet时，还要使用HttpServletRequest和HttpServletResponse对象，他们分别表示Servlet请求和Servlet响应。它们分别继承ServletRequest和ServletResponse。
　　HttpServlet覆盖GenericServlet中的service方法，并且还使用`void service(HttpServletRequest request, HttpServletResponse responce)`重载了了该方法。  
　　Servlet容器还是会调用javax.servlet.Servlet中原始的service方法，HttpServlet中的service方法要如下这么写：
```java
public void service(ServletRequest req, ServletResponse res) throws ServletExcept, IOException {
    HttpServletRequest request;
    HttpServletResponse responce;
    try {
        request = (HttpServletRequest) req;
        responce = (HttpServletResponse) res;
    } catch (ClassCastException e) {
        throws new ServletException("non-HTTP request or responce");
    }
    service(request, responce);
}
```
　　原始的service方法将请求和响应对象进行向下转换，分别从Servlet容器转换成HttpServletRequest和HttpServletResponse，并调用新的service方法。向下转换总是会成功，因为在调用一个Servlet的service方法时，Servlet容器总会预计使用HTTP，所以传递一个HttpServletRequest和一个HttpServletResponse。即使正在实现javax.servlrt.Servlet接口或者基础java.servlet.GenericServlet，也可以将传给service方法的Servlet请求和Servlet响应，分别向下转换成HttpServletRequest和HttpServletResponse。
　　之后，HttpServlet中新的service方法会查看通常用来发送请求（通过Request.getMethod）的Http方法，并调用以下某个方法（doGet、doPost、doHead、doPut、doTrace、doOptions和doDelete），这7个方法各自表示一个HTTP方法。
