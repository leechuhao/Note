[每个对象的常用方法](http://www.cnblogs.com/kelin1314/archive/2011/03/03/1969578.html)

JSP九大内置对象分为三类：

1. 输入输出对象：out对象、response对象、request对象
2. 通信控制对象：pageContext对象、session对象、application对象
3. Servlet对象：page对象、config对象
4. 错误处理对象：exception对象

内置对象特点：

1. 由JSP规范提供，不用编写者实例化。
2. 通过Web容器实现和管理
3. 所有JSP页面均可使用
4. 只有在脚本元素的表达式或代码段中才可使用（<%=使用内置对象%>或<%使用内置对象%>）

- Request(Javax.servlet.ServletRequest)它包含了有关浏览器请求的信息.通过该对象可以获得请求中的头信息、Cookie和请求参数。

- Response(Javax.servlet.ServletResponse)作为JSP页面处理结果返回给用户的响应存储在该对象中。并提供了设置响应内容、响应头以及重定向的方法(如cookies,头信息等)

- Out(Javax.servlet.jsp.JspWriter)用于将内容写入JSP页面实例的输出流中,提供了几个方法使你能用于向浏览器回送输出结果。

- pageContext(Javax.servlet.jsp.PageContext)描述了当前JSP页面的运行环境。可以返回JSP页面的其他隐式对象及其属性的访问,另外,它还实现将控制权从当前页面传输至其他页面的方法。

- Session(javax.servlet.http.HttpSession)会话对象存储有关此会话的信息,也可以将属性赋给一个会话,每个属性都有名称和值。会话对象主要用于存储和检索属性值。

- Application(javax.servle.ServletContext)存储了运行JSP页面的servlet以及在同一应用程序中的任何Web组件的上下文信息。

- Page(Java.lang.Object)表示当前JSP页面的servlet实例

- Config(javax.servlet.ServletConfig)该对象用于存取servlet实例的初始化参数。

- Exception(Javax.lang.Throwable)在某个页面抛出异常时,将转发至JSP错误页面,提供此对象是为了在JSP中处理错误。只有在错误页面中才可使用<%@page isErrorPage=“true”%>

| **Jsp****内置对象** | **功能**                                                     | **主要方法**                                                 |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| out                 | 向客户端输出数据                                             | print() println() flush() clear() isAutoFlush() getBufferSize()   close() ………… |
| request             | 向客户端请求数据                                             | getAttributeNames() getCookies() getParameter() getParameterValues() setAttribute() getServletPath() ………….. |
| response            | 封装了jsp产生的响应,然后被发送到客户端以响应客户的请求       | addCookie() sendRedirect() setContentType()flushBuffer() getBufferSize() getOutputStream()sendError() containsHeader()…………… |
| application         |                                                              |                                                              |
| config              | 表示Servlet的配置,当一个Servlet初始化时,容器把某些信息通过此对象传递给这个Servlet | getServletContext() getServletName() getInitParameter()   getInitParameterNames()…………… |
| page                | Jsp实现类的实例,它是jsp本身,通过这个可以对它进行访问         | flush()………                                                   |
| pagecontext         | 为JSP页面包装页面的上下文。管理对属于JSP中特殊可见部分中己经命名对象的该问 | forward() getAttribute() getException() getRequest() getResponse()   getServletConfig()getSession() getServletContext() setAttribute()removeAttribute() findAttribute() …………… |
| session             | 用来保存每个用户的信息,以便跟踪每个用户的操作状态            | getAttribute() getId()   getAttributeNames() getCreateTime() getMaxInactiveInterval()invalidate() isNew() |
| exception           | 反映运行的异常                                               | getMessage()…………                                             |

 