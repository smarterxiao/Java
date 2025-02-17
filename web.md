#1. 实际的 Web 服务器会做些什么

![](/assets/屏幕截图 2017-02-21 17:41:17.png)

* 建立连接——接受一个客户端连接，或者如果不希望与这个客户端建立连接，就将其关闭。
* 接收请求——从网络中读取一条 HTTP 请求报文。
* 处理请求——对请求报文进行解释，并采取行动。
* 访问资源——访问报文中指定的资源。
* 构建响应——创建带有正确首部的 HTTP 响应报文。
* 发送响应——将响应回送给客户端。
* 记录事务处理过程——将与已完成事务有关的内容记录在一个日志文件中。
##第一步——接受客户端连接
### 处理新连接
客户端请求一条到 Web 服务器的 TCP 连接时，Web 服务器会建立连接，判断连接的另一端是哪个客户端，从 TCP 连接中将 IP 地址解析出来。1 一旦新连接建立起来并被接受，服务器就会将新连接添加到其现存 Web 服务器连接列表中，做好监视连接上数据传输的准备
###客户端主机名识别
###通过ident确定客户端用户
##第二步——接收请求报文
![](/assets/屏幕截图 2017-02-21 18:00:18.png)
![](/assets/屏幕截图 2017-02-21 18:01:57.png)
## 第三步——处理请求
## 第四步——对资源的映射及访问

![](/assets/屏幕截图 2017-02-21 18:04:22.png)
### 1.虚拟托管的docroot
虚拟托管的 Web 服务器会在同一台 Web 服务器上提供多个 Web 站点，每个站点在服务器上都有自己独有的文档根目录。虚拟托管 Web 服务器会根据 URI 或 Host 首部的 IP 地址或主机名来识别要使用的正确文档根目录。通过这种方式，即使请求 URI 完全相同，托管在同一 Web 服务器上的两个 Web 站点也可以拥有完全不同的内容了
![](/assets/屏幕截图 2017-02-21 18:06:28.png)
![](/assets/屏幕截图 2017-02-21 18:06:59.png)
###目录列表
Web 服务器可以接收对目录 URL 的请求，其路径可以解析为一个目录，而不是文件。我们可以对大多数 Web 服务器进行配置，使其在客户端请求目录 URL 时采取不同的动作。
###动态内容资源的映射
Web 服务器还可以将 URI 映射为动态资源——也就是说，映射到按需动态生成内容的程序上去（参见图 5-11）。实际上，有一大类名为应用程序服务器的 Web 服务器会将 Web 服务器连接到复杂的后端应用程序上去。Web 服务器要能够分辨出资源什么时候是动态的，动态内容生成程序位于何处，以及如何运行那个程序。大多数 Web 服务器都提供了一些基本的机制以识别和映射动态资源。
![](/assets/屏幕截图 2017-02-21 18:16:17.png)
Apache 允许用户将 URI 路径名组件映射为可执行文件目录。服务器收到一条带有可执行路径组件的对 URI 的请求时，会试着去执行相应服务器目录中的程序。例如，下列 Apache 配置指令就表明所有路径以 /cgi-bin/ 开头的 URI 都应该执行在目录 /usr/local/etc/httpd/cgi-programs/ 中找到的相应文件：
ScriptAlias /cgi-bin/ /usr/local/etc/httpd/cgi-programs/

Apache 还允许用户用一个特殊的文件扩展名来标识可执行文件。通过这种方式就可以将可执行脚本放在任意目录中了。下面的 Apache 配置指令说明要执行所有以 .cgi 结尾的 Web 资源：


```
AddHandler cgi-script .cgi
```
##第五步——构建响应
###响应实体
如果事务处理产生了响应主体，就将内容放在响应报文中回送过去。如果有响应主体的话，响应报文中通常包括：
 
* 描述了响应主体 MIME 类型的 Content-Type 首部；
* 描述了响应主体长度的 Content-Length 首部；
* 实际报文的主体内容。
###MIME类型
* MIME 类型（mime.types）

    Web 服务器可以用文件的扩展名来说明 MIME 类型。Web 服务器会为每个资源扫描一个包含了所有扩展名的 MIME 类型的文件，以确定其 MIME 类型。这种基于扩展名的类型相关是最常见的
    
    ![](/assets/屏幕截图 2017-02-21 18:21:22.png)
* 魔法分类（Magic typing）

    Apache Web 服务器可以扫描每个资源的内容，并将其与一个已知模式表（被称为魔法文件）进行匹配，以决定每个文件的 MIME 类型。这样做可能比较慢，但很方便，尤其是文件没有标准扩展名的时候。
* 显式分类（Explicit typing）

    可以对 Web 服务器进行配置，使其不考虑文件的扩展名或内容，强制特定文件或目录内容拥有某个 MIME 类型。
* 类型协商

    有些 Web 服务器经过配置，可以以多种文档格式来存储资源。在这种情况下，可以配置 Web 服务器，使其可以通过与用户的协商来决定使用哪种格式（及相关的 MIME 类型）“最好”。这个问题将在第 17 章讨论。
###重定向
Web 服务器有时会返回重定向响应而不是成功的报文。 Web 服务器可以将浏览器重定向到其他地方来执行请求。重定向响应由返回码 3XX 说明。Location 响应首部包含了内容的新地址或优选地址的 URI。重定向可用于下列情况。
##第六步——发送响应
##第七步——记录日志
