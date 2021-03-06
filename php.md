## php web相关

#### PHP的session机制

1. 当代码session_start()运行时，服务器产生一个session文件，同时也产生一个与之对应的session_id; 数据显序列化再卸乳session 文件中，客户端获取session信息时，将session_id传递给服务器，服务器根据session_id找到对应文件，读取信息然后对信息反序列化就能获取到数据。 注：PHP默认情况下，适用客户端的Cookie来保存session_id（session_start();之后，会自动将session_id存储在cookie中），但是必须注意，Session不一定必须依赖Cookie，这也就是Session相比于Cookie的高明之处。当客户端的Cookie被禁用或出现问题时，PHP会自动把session_id附着在URL中，这样再通过session_id就能实现跨页使用session变量了。但是这种附着也是有一定条件的，即php.ini文件中的“session.use_trans_sid=1”或者编译时打开了--enable-trans-sid选项。
2. 当session禁用时，需设置以下参数，session_id会自动追加到URL中

    session.use_trans_sid = 1
    session.use_only_cookies = 0
    
3. Session在大型网站中使用，需注意的问题 

      3.1 如何解决sessio文件过多，消耗IO性能 
  
    建设设置php.ini的sessio.save_handler参数设置为redis或者memcache等nosql数据库
    
      3.2 session 同步问题
  
    a. 设置php.ini的sessio.save_handler参数设置为redis或者memcache等nosql数据库 
    
    b. 在负载均衡层保持会话绑定到某个服务器上，例如：nginx设置为ip_hash

#### cookie与session的区别

1. cookie数据存放在客户的浏览器上，session数据放在服务器上。

2. cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗，考虑到安全应当使用session。

3. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用COOKIE。

4. 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

#### get和post的区别
1. get是从服务器上获取数据，post是向服务器传送数据。
2. get是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。post是通过HTTP post机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址。用户看不到这个过程。
3. get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。4.. get安全性非常低，post安全性较高。但是执行效率却比Post方法好。

#### php在储存session以什么形式存在

PHP为session的存储提供了三种方式: 文件/ 内存/ 自定义存储,默认是使用文件存储.在访问量大的网站上采用这种方式就不大合 适,因为这样会导致大量的输入输出的冗余.我们可以在php.ini更改配置文件或者php脚本中通过相应的函数来设置session文件的存储类型来改变session文件的存储形式

#### 什么是CSRF攻击？XSS攻击？如何防范？
CSRF：跨站请求伪造，可以通过通过判断来源和加 Token 的方式来防范。

XSS：跨站脚本攻击，可以通过对内容转义和过滤来防范,还有CSP
PHP函数: 
htmlentities()
把字符转换为 HTML 实体
htmlspecialchars()
把预定义的字符 "<" （小于）和 ">" （大于）转换为 HTML 实体

#### xss攻击怎么防止
  
XSS又称CSS，全称Cross SiteScript(跨站脚本攻击)， XSS攻击类似于SQL注入攻击，是Web程序中常见的漏洞，XSS属于被动式且用于客户端的攻击方式，所以容易被忽略其危害性。其原理是攻击者向有XSS漏洞的网站中输入(传入)恶意的HTML代码，当用户浏览该网站时，这段HTML代码会自动执行，从而达到攻击的目的。如，盗取用户Cookie信息、破坏页面结构

常见的恶意字符XSS输入：

1. XSS 输入通常包含 JavaScript 脚本，如弹出恶意警告框：
`<script>alert("XSS");</script>`

2. XSS 输入也可能是 HTML 代码段，譬如：

    (1) 网页不停地刷新 `<meta http-equiv="refresh" content="0;">`

    (2) 嵌入其它网站的链接，重定向到其它网站等。

方法：利用php htmlentities()函数

php防止XSS跨站脚本攻击的方法：是针对非法的HTML代码包括单双引号等，使用htmlspecialchars()函数。


#### [静态化如何实现的](https://blog.csdn.net/qq_39618306/article/details/79014438)
这里要说的静态化指的是页面静态化，也即生成实实在在的静态文件，也即不需要查询数据库就可以直接从文件中获取数据，指的是真静态。它的实现方式主要有两种：

- 一种是我们在添加信息入库的时候就生成的静态文件，也称为模板替换技术，这种主要用在后台，用于一些基本上很少变化的信息上，在添加信息的时候使用添加的信息来替换制定好的模板中的内容，达到生成静态文件的目的，这样在前台访问该信息时，可以直接从生成好的静态文件中获取信息，如一些CMS系统。

- 另外一种是用户在访问我们的页面时先判断是否有对应的缓存文件存在，如果存在就读缓存，不存在就读数据库，同时生成缓存文件。这种实现的主要原理是基于PHP中的ob缓冲技术来实现的，当没有静态文件时，从数据库中读取，读取的数据使用OB缓存，使用相关的函数从OB缓冲中读取数据，写入到文件中，形成静态文件。当然这个过程中要考虑静态文件的缓存周期问题，我们可以根据文件的最后修改时间和当前时间及设定的缓存时间来定时更新缓存文件。

#### 如何处理负载、高并发
从低成本、高性能和高扩张性的角度来说有如下处理方案：

1. HTML静态化
其实大家都知道，效率最高、消耗最小的就是纯静态化的html页面，所以我们尽可能使我们的 网站上的页面采用静态页面来实现，这个最简单的方法其实也是最有效的方法。

2. 图片服务器分离
把图片单独存储，尽量减少图片等大流量的开销，可以放在一些相关的平台上，如骑牛等

3. 数据库集群和库表散列及缓存
数据库的并发连接为100，一台数据库远远不够，可以从读写分离、主从复制，数据库集群方面来着手。另外尽量减少数据库的访问，可以使用缓存数据库如memcache、redis。

4. 镜像：
尽量减少下载，可以把不同的请求分发到多个镜像端。

5. 负载均衡：
Apache的最大并发连接为1500，只能增加服务器，可以从硬件上着手，如F5服务器。当然硬件的成本比较高，我们往往从软件方面着手。
**负载均衡**建立在现有网络结构之上，它提供了一种廉价有效透明的方法扩展网络设备和服务器的带宽、增加吞吐量、加强网络数据处理能力，同时能够提高网络的灵活性和可用性。目前使用最为广泛的负载均衡软件是Nginx、LVS、HAProxy。
- 知识点： [反向代理](https://www.cnblogs.com/Anker/p/6056540.html)


## php特性

#### PHP 的垃圾收集机制是怎样的
php作为脚本语言是页面结束即释放变量所占内存的。 当一个 PHP线程结束时，当前占用的所有内存空间都会被销毁，当前程序中所有对象同时被销毁。GC进程一般都跟着每起一个SESSION而开始运行的.gc目的是为了在session文件过期以后自动销毁删除这些文件.在PHP中，没有任何变量指向这个对象时，这个对象就成为垃圾。PHP会将其在内存中销毁；这是PHP的GC垃圾处理机制，防止内存溢出。 执行这些函数也可以起到回收作用__destruct /unset/mysql_close /fclose php对session有明确的gc处理时间设定session.gc_maxlifetime 如果说有垃圾，那就是整体的程序在框架使用中，会多次调用同一文件等等造成的非单件模式等。所以在出来的时候，必要的用_once引用，在声明类的时候使用单件模式。还有简化逻辑等等。

#### zval

[内存管理](https://www.jianshu.com/p/63a381a7f70c)

[垃圾回收机制](http://php.net/manual/zh/features.gc.php)

#### cgi、fastcgi、php-fpm

- **cgi**
早期的web server只可以处理简单的静态web文件，但是随着技术的发展出现动态语言如PHP，Python。PHP语言交给PHP解析器进行处理，但是处理之后如何和web server进行通信呢？
为了解决不同的语言处理器与web server之间的通讯，出现了CGI协议。只要按照CGI协议编写程序，就可以实现与语言解析器与web server之间的通讯。
CGI协议虽然解决了语言解析器和seb server之间通讯的问题，但是它的效率很低。因为web server每收到一个请求都会创建一个CGI进程，PHP解析器都会解析php.ini文件，初始化环境，请求结束的时候再关闭进程。对于每一个创建的CGI进程都会执行这些操作。所以效率很低。

- **FastCGI**
FastCGI是用来提高CGI性能的，FastCGI每次处理完请求之后不会关闭掉进程。而是保留这个进程，使这个进程可以处理多个请求。这样的话每个请求都不用再重新创建一个进程了。大大提升了处理效率。

- **PHP-FPM**
PHP-FPM(FastCGI Process Manager：FastCGI进程管理器)是一个实现了Fastcgi的程序，并且提供进程管理的功能。进程包括master进程和worker进程。master进程只有一个，负责监听端口，接受来自web server的请求。worker进程一般会有多个，每个进程中会嵌入一个PHP解析器，进程PHP代码的处理。

#### php.ini中的safe_mode 影响
**Warning
本特性已自 PHP 5.3.0 起废弃并将自 PHP 5.4.0 起移除。**

1)用户输入输出函数（fopen()file()require(),只能用于调用这些函数有相同脚本的拥有者）

2)创建新文件（限制用户只在该用户拥有目录下创建文件）

3)用户调用popen()systen()exec()等脚本，只有脚本处在safe_mode_exec_dir配置指令指定的目录中才可能

4)加强HTTP认证，认证脚本拥有者的UID的划入认证领域范围内，此外启用安全模式下，不会设置PHP_AUTH

5)mysql服务器所用的用户名必须与调用mysql_connect()的文件的拥有者用户名相同6)

受影响的函数变量以及配置命令达到40个

#### php的设计模式

1. **单例模式**
一个类在整个应用中，只有一个对象实例的设计模式
类必须自行创建这个实例
必须自行向整个系统提供这个实例
**三私**：私有静态成员变量、构造函数、克隆函数
**一公**：公共的静态方法

2. **工厂模式**
可以根据输入的参数或者应用程序配置的不同,创建一种专门用来实例化并返回其它类的实例的类

3. 观察者模式
观察者模式提供了组件之间紧密耦合的另一种方法。
该模式：一个对象通过添加一个方法（该方法允许另一个对象，即观察者注册自己）全本身变得可观察。当可观察的对象更改时，它会将消息发送到已注册的观察者。这些观察者使用该信息执行的操作与可观察的对象无关。

4. 命令链模式：
以松散耦合主题为基础，发送消息、命令和请求，或通过一组处理程序发送任意内容。每个处理程序都会自行判断自己能否处理请求，如果可以，该请求被处理，进程停止。

5. 策略模式：
此算法是从复杂类提取的，因而可以方便地替换。

## php语法

#### Include和require的区别

require函数通常放在PHP程序的最前面，在PHP程序执行之前，就会先读取require指定引入的文件，使它变成PHP程序网页的一部分。

include函数一般是放在流程控制的处理部分中。PHP程序在读到include的文件时，才将它读进来，这种方式可以把程序执行时的流程简单化。

他们两个的用途是一样的，不一定非要哪个放在最前面哪个放在中间，他们最根本的区别在于错误处理的方式不一样。

require一个文件存在错误的话，那么程序就会中断执行，并显示致命错误

而include一个文件存在错误的话，那么程序不会中断，会继续执行，并显示一个警告的错误

其它区别：include有返回值，而require没有。

#### PHP的变量类型

**四种标量类型**

1. boolean （布尔型）：这是最简单的类型，只有两种取值，可以为 TRUE/true 或 FALSE/false ，不区分大小写。详细请查看：PHP布尔类型（boolean）

2. integer （整型）：在32 位操作系统中它的有效范围是：-2 147 483 648~+2 147 483 647。整型值可以使用十进制，十六进制或八进制表示，前面可以加上可选的符号（- 或者 +）。八进制表示数字前必须加上 0（零），十六进制表示数字前必须加上 0x。

3. float （浮点型, 也称作 double)

4. string （字符串）：字符型变量不同于其他编程语言有字符与字符串之分，在PHP 中，统一使用字符型变量来定义字符或者字符串。

**两种复合类型**

1. array （数组）：数组型变量是一种比较特殊的变量类型，将在后续章节中详细说明。

2. object （对象）：对象也是一种特殊的数据类型。要创建object变量，请使用 new 关键字。详细请查看：PHP对象类型（object）

**两种特殊类型**：

1. resource（资源）：源是一种特殊变量，保存了到外部资源的一个引用。资源是通过专门的函数来建立和使用的。详情请查看：PHP资源类型（resource）

2. NULL（NULL）：表示一个变量没有值。NULL 类型唯一可能的值就是 NULL。

#### 单引号与双引号的区别
1. 单引号内部的变量不会执行， 双引号会执行

2. 单引号解析速度比双引号快。

3. 单引号只能解析部分特殊字符，双引号可以解析所有特殊字符。

## php扩展

#### GD库

图像处理扩展
[GD 和图像处理 函数](http://php.net/manual/zh/ref.image.php)

#### yaf
[Yet Another Framework](http://php.net/manual/zh/book.yaf.php)

#### curl

[Client URL](http://php.net/manual/zh/book.curl.php)

#### mysqli

[mysql增强版](http://php.net/manual/zh/book.mysqli.php)

## php7

[php7新特性](http://www.runoob.com/w3cnote/php7-new-features.html)

- ??运算符(NULL 合并运算符)

由于日常使用中存在大量同时使用三元表达式和 isset()的情况，NULL 合并运算符使得变量存在且值不为NULL， 它就会返回自身的值，否则返回它的第二个操作数。

```php
<?php
// 如果 $_GET['user'] 不存在返回 'nobody'，否则返回 $_GET['user'] 的值
$username = $_GET['user'] ?? 'nobody';
// 类似的三元运算符
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';
?>
```

- <=> 太空船操作符（组合比较符）

太空船操作符用于比较两个表达式。当a大于、等于或小于b时它分别返回-1、0或1。

```php
<?php
// 整型
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1

// 浮点型
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1
 
// 字符串
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1
?>
```

- 通过 define() 定义常量数组

```php
<?php
define('ANIMALS', [
    'dog',
    'cat',
    'bird'
]);

echo ANIMALS[1]; // 输出 "cat"
?>
```

- 整除

新增了整除函数 intdiv()

```php
<?php
var_dump(intdiv(10, 3));
// 输出结果： int(3)
?>
```

## php优化

[48条高效率的PHP优化写法](https://www.awaimai.com/1050.html)

## php对象基础

#### 写出一些php魔幻（术）方法;

__construct() 实例化类时自动调用。

__destruct() 类对象使用结束时自动调用。

__set() 在给未定义的属性赋值的时候调用。

__get() 调用未定义的属性时候调用。

__isset() 使用isset()或empty()函数时候会调用。

__unset() 使用unset()时候会调用。

__sleep() 使用serialize序列化时候调用。

__wakeup() 使用unserialize反序列化的时候调用。

__call() 调用一个不存在的方法的时候调用。

__callStatic()调用一个不存在的静态方法是调用。

__toString() 把对象转换成字符串的时候会调用。比如 echo。

__invoke() 当尝试把对象当方法调用时调用。

__set_state() 当使用var_export()函数时候调用。接受一个数组参数。

__clone() 当使用clone复制一个对象时候调用。
