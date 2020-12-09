---
title: TOMCAT
---
# Servlet生命周期
Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：

## Servlet 通过调用 init () 方法进行初始化。  

    public void init() throws ServletException {
    // 初始化代码...
    }
init 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用。因此，它是用于一次性初始化，就像 Applet 的 init 方法一样。

Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时，但是您也可以指定 Servlet 在服务器第一次启动时被加载。

当用户调用一个 Servlet 时，就会创建一个 Servlet 实例，每一个用户请求都会产生一个新的线程，适当的时候移交给 doGet 或 doPost 方法。init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期。
>Servlet.java

![avatar](https://stepimagewm.how2j.cn/1594.png)
![avatar](https://stepimagewm.how2j.cn/1595.png)
## Servlet 调用 service() 方法来处理客户端的请求。  

    public void service(ServletRequest request, 
                    ServletResponse response) 
      throws ServletException, IOException{
    }
service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。

每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。

service() 方法由容器调用，service 方法在适当的时候调用 doGet、doPost、doPut、doDelete 等方法。所以，您不用对 service() 方法做任何动作，您只需要根据来自客户端的请求类型来重载 doGet() 或 doPost() 即可。
### -- doGet() 方法

    public void doGet(HttpServletRequest request,
                  HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
    }
GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理。
### -- doPost() 方法

    public void doPost(HttpServletRequest request,
                   HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
    }
POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 doPost() 方法处理。
## destroy() 方法

    public void destroy() {
    // 终止化代码...
    }
destroy() 方法只会被调用一次，在 Servlet 生命周期结束时被调用。destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动。
在调用 destroy() 方法之后，servlet 对象被标记为垃圾回收。

最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。  

# Servlet 表单数据
很多情况下，需要传递一些信息，从浏览器到 Web 服务器，最终到后台程序。浏览器使用两种方法可将这些信息传递到 Web 服务器，分别为 GET 方法和 POST 方法。

## GET 方法
GET 方法是默认的从浏览器向 Web 服务器传递信息的方法，它会产生一个很长的字符串，出现在浏览器的地址栏中。如果您要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法。GET 方法有大小限制：请求字符串中最多只能有 1024 个字符。

这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问，Servlet 使用 doGet() 方法处理这种类型的请求。

    http://www.test.com/hello?key1=value1&key2=value2

## POST 方法
另一个向后台程序传递信息的比较可靠的方法是 POST 方法。POST 方法打包信息的方式与 GET 方法基本相同，但是 POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，而是把这些信息作为一个单独的消息。消息以标准输出的形式传到后台程序，您可以解析和使用这些标准输出。Servlet 使用 doPost() 方法处理这种类型的请求。

## 使用 Servlet 读取表单数据
Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析：

getParameter()：您可以调用 request.getParameter() 方法来获取表单参数的值。  
getParameterValues()：如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框。  
getParameterNames()：如果您想要得到当前请求中的所有参数的完整列表，则调用该方法。  

### Get和Post方法传递表单数据
>HelloForm.java  
Hello.html

### 复选框
>CheckBox.java  
CheckBox.html

## 读取所有的表单参数
使用 HttpServletRequest 的 getParameterNames() 方法读取所有可用的表单参数。该方法返回一个枚举，其中包含未指定顺序的参数名。

一旦我们有一个枚举，我们可以以标准方式循环枚举，使用 hasMoreElements() 方法来确定何时停止，使用 nextElement() 方法来获取每个参数的名称。

# 如何进行服务端跳转和客户端跳转
首先在web目录下准备两个页面 success.html,fail.html
分别用于显示登录成功 或者登录失败

如果登录成功了，就服务端跳转到success.html
如果登录失败了，就客户端跳转到fail.html
>SkipSuccess.html  
SkipFail.html

## 服务端跳转

    request.getRequestDispatcher("success.html").forward(request, response);
服务端跳转可以看到浏览器的地址依然是/login 路径，并不会变成success.html
>![avatar](https://stepimagewm.how2j.cn/1568.png)

## 客户端跳转

    response.sendRedirect("fail.html");
可以观察到，浏览器地址发生了变化
>![avatar](https://stepimagewm.how2j.cn/1567.png)

# 自启动配置
    //在web.xml中，配置Hello Servlet的地方，增加一句
    <load-on-startup>10</load-on-startup>
取值范围是1-99，即表明该Servlet会随着Tomcat的启动而初始化。

同时，为HelloServlet提供一个init(ServletConfig) 方法，验证自启动  
在tomcat完全启动之前，就打印了init of HelloServlet
<load-on-startup>10</load-on-startup> 中的10表示启动顺序
如果有多个Servlet都配置了自动启动，数字越小，启动的优先级越高

# REQUEST常见方法
request.getRequestURL(): 浏览器发出请求时的完整URL，包括协议 主机名 端口(如果有)"
request.getRequestURI(): 浏览器发出请求的资源名部分，去掉了协议和主机名"
request.getQueryString(): 请求行中的参数部分，只能显示以get方式发出的参数，post方式的看不到
request.getRemoteAddr(): 浏览器所处于的客户机的IP地址
request.getRemoteHost(): 浏览器所处于的客户机的主机名
request.getRemotePort(): 浏览器所处于的客户机使用的网络端口
request.getLocalAddr(): 服务器的IP地址
request.getLocalName(): 服务器的主机名
request.getMethod(): 得到客户机请求方式一般是GET或者POST

## 获取参数
request.getParameter(): 是常见的方法，用于获取单值的参数
request.getParameterValues(): 用于获取具有多值的参数，比如注册时候提交的 "hobits"，可以是多选的。
request.getParameterMap(): 用于遍历所有的参数，并返回Map类型。
>ReadParams.html  
ReadParams.java  
结果在服务端显示

## 获取头信息
request.getHeader() 获取浏览器传递过来的头信息。
比如getHeader("user-agent") 可以获取浏览器的基本资料，这样就能判断是firefox、IE、chrome、或者是safari浏览器
request.getHeaderNames() 获取浏览器所有的头信息名称，根据头信息名称就能遍历出所有的头信息

头信息:
host: 主机地址  
user-agent: 浏览器基本资料  
accept: 表示浏览器接受的数据类型  
accept-language: 表示浏览器接受的语言  
accept-encoding: 表示浏览器接受的压缩方式，是压缩方式，并非编码  
connection: 是否保持连接  
cache-control: 缓存时限  
>GetHeader.java

# RESPONSE常见用法
## 设置响应内容
通过response.getWriter(); 获取一个PrintWriter 对象
可以使用println(),append(),write(),format()等等方法设置返回给浏览器的html内容。

    //doGet()
            PrintWriter pw= response.getWriter();
            pw.println("<h1>Hello Servlet</h1>");

## 设置响应格式
"text/html" 是即格式 ，在request获取头信息 中对应的request.getHeader("accept").
"text/html" 是存在的，表示浏览器可以识别这种格式，如果换一个其他的格式， 比如 "text/lol" ，浏览器不能识别，那么打开此servlet就会弹出一个下载的对话框。

    //response.setContentType("text/html");
    response.setContentType("text/lol");

## 设置响应编码
设置响应编码有两种方式:
    
    //不仅发送到浏览器的内容会使用UTF-8编码，而且还通知浏览器使用UTF-8编码方式进行显示。所以总能正常显示中文
    response.setContentType("text/html; charset=UTF-8");

    //仅仅是发送的浏览器的内容是UTF-8编码的，至于浏览器是用哪种编码方式显示不管。 所以当浏览器的显示编码方式不是UTF-8的时候，就会看到乱码，需要手动再进行一次设置。
    response.setCharacterEncoding("UTF-8");

## 301或者302客户端跳转

    //302 表示临时跳转,302就是前面在客户端跳转章节用到过的
    response.sendRedirect("fail.html");

    //301 表示永久性跳转,301要使用另外的手段：
    response.setStatus(301);
    response.setHeader("Location", "fail.html");

## 设置不使用缓存
使用缓存可以加快页面的加载，降低服务端的负担。但是也可能看到过时的信息，可以通过如下手段通知浏览器不要使用缓存

    //doGet()
    response.setDateHeader("Expires",0 );
    response.setHeader("Cache-Control","no-cache");
    response.setHeader("pragma","no-cache");

# 上传文件
## 上传页面 upload.html

    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <form action="uploadPhoto" method="post" enctype="multipart/form-data">
    ……
    </form>

## 在UploadPhotoServlet进行上传的功能开发。
前部分代码是固定写法，用来做一些准备工作。 直到遍历出Item,一个Item就是对应一个浏览器提交的数据，通过item.getInputStream可以打开浏览器上传的文件的输入流。

客户提交的文件名有可能是一样的，所以在服务端保存文件的时候，不能使用客户提交的文件名。这里使用的是一种粗糙的解决文件名重复的办法，即使用时间戳。

读取输入流中的数据，保存在服务端的目录下，这个目录是通过getRealPath获取到的。 如果项目部署在其他地方，那么会自动做相应的变化。
注： 为什么要放这里？ 因为后续网页上显示的时候是通过http://127.0.0.1/uploaded/xxx.jpg 路径来获取图片。

根据临时生成的文件名，创建一个html img元素，然后通过response返回浏览器
