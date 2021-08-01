# Http

## 什么是 HTTP 报文？

**定义**:

HTTP 全称 Hypertext Transfer Protocol，超文本传输协议，是应用层协议

HTTP 报文由从客户端到服务器的请求和从服务器到客户端的响应构成

**组成**:
· 起始行：报文描述
· 头部：报文属性
· 主体：报文数据

**分类**:
· 请求报文：客户端到服务端发送请求
	1.请求行：方法 + URL + HTTP协议版本
	2.通用信息头：Connection等
	3.请求头
	4.实体头：POST / PUT / PATCH 的Content-Type``Content-Length等
	5.报文主体
·响应报文：服务端到客户端返回数据
	1.状态行：状态码 + 原因
	2.通用信息头：Connection等
	3.响应头
	4.实体头：Content-Type Content-Length等
	5.报文主体

## 常见的 HTTP 状态码有哪些？

| 状态码 |   原因   | 说明     |
| ------ | ---- | ---- |
| 100-199 | **信息响应** |      |
| 100 | Continue | 已收到请求，客户端应继续 |
| 101 | Switching Protocol | 响应客户端Upgrade列出协议，服务端正在切换协议 |
| 102 | Processing | 服务端正在处理请求，无响应可用 |
| 103 | Early Hints | 与Link一起使用，客户端应在服务端继续响应前开始预加载资源 |
| 200-299 | **成功响应** |  |
| 200 | OK | 请求成功，常见于GET、HEAD、POST、TRACE |
| 201 | Created | 请求成功，新资源已创建，常见于POSTPUT |
| 202 | Accepted | 请求已收到，但未响应 |
| 203 | Non-Authoritative Information | 响应经过了代理服务器修改 |
| 204 | No Content | 请求已处理，无返回，客户端不更新视图 |
| 205 | Reset Content | 请求已处理，无返回，客户端应更新视图 |
| 206 | Partial Content | 请求已处理，返回部分内容，常见于视频点播、分段下载、断点续传 |
| 300-399 | **重定向** |  |
| 300 | Multiple Choice | 提供一系列地址供客户端选择重定向 |
| 301 | Moved Permanently | 永久重定向，默认可缓存，搜索引擎应更新链接 |
| 302 | Found | 临时重定向，默认不缓存，除非显示指定 |
| 303 | See Other | 临时重定向，必须GET请求 |
| 304 | Not Modified | 未修改，不含响应体 |
| 307 | Temporary Redirect | 临时重定向，默认不缓存，除非显示指定，不改变请求方法和请求体 |
| 308 | Permanent Redirect | 永久重定向，默认可缓存，搜索引擎应更新链接，不改变请求方法和请求体 |
| 400-499 | **客户端错误** |  |
| 400 | Bad Request | 请求语义或参数有误，不应重复请求 |
| 401 | Unauthorized | 请求需身份验证或验证失败 |
| 403 | Forbidden | 拒绝，不应重复请求 |
| 404 | Not Found | 未找到，无原因 |
| 405 | Method Not Allowed | 不允许的请求方法，并返回Allow允许的请求方法列表 |
| 406 | Not Acceptable | 无法根据请求条件，返回响应体 |
| 407 | Proxy Authentication Required | 请求需在代理服务器上身份验证 |
| 408 | Request Timeout | 请求超时 |
| 409 | Conflict | 请求冲突，响应应包含冲突原因 |
| 410 | Gone | 资源已被永久移除 |
| 411 | Length Required | 请求头需添加Content-Length |
| 412 | Precondition Failed | 非GETPOST请求外，If-Unmodified-Since或If-None-Match规定先决条件无法满足 |
| 413 | Payload Too Large | 请求体数据大小超过服务器处理范围 |
| 414 | URI Too Long | URL过长，查询字符串过长时，应使用POST请求 |
| 415 | Unsupported Media Type | 请求文件类型服务端不支持 |
| 416 | Range Not Satisfiable | 请求头Range与资源可用范围不重合 |
| 417 | Expectation Failed | 服务端无法满足客户端通过Expect设置的期望响应 |
| 421 | Misdirected Request | HTTP2下链接无法复用时返回 |
| 425 | Too Early | 请求有重放攻击风险 |
| 426 | Upgrade Required | 客端应按响应头Upgrade的协议列表中的协议重新请求 |
| 428 | Precondition Required | 没有符合If-Match的资源 |
| 429 | Too Many Requests | 请求频次超过服务端限制 |
| 431 | Request Header Fields Too Large | 请求头字段过大 |
| 451 | Unavailable For Legal Reasons | 因法律原因该资源不可用 |
| 500-511 | **服务端响应** |  |
| 500 | Internal Server Error | 服务端报错，通常是脚本错误 |
| 501 | Not Implemented | 请求方法不被服务器支持 |
| 502 | Bad Gateway | 网关无响应，通常是服务端环境配置错误 |
| 503 | Service Unavailable | 服务端临时不可用，建议返回Retry-After，搜索引擎爬虫应一段时间再次访问这个URL |
| 504 | Gateway Timeout | 网关超时，通常是服务端过载 |
| 505 | HTTP Version Not Supported | 请求的 HTTP 协议版本不被支持 |
| 506 | Variant Also Negotiates | 内部服务器配置错误 |
| 510 | Not Extended | 不支持 HTTP 扩展 |
| 511 | Network Authentication Required | 需要身份验证，常见于公用 WIFI |

## 常用的 HTTP 方法有哪些，GET 和 POST 的区别是什么？

**安全：**请求方法不会影响服务器上的资源
**幂等：**多次执行相同操作，结果相同
**显示声明缓存：**响应头包含Expires Cache-Control: max-age等时缓存

| **方法** | **描述** | **请求含主体** | **响应含主体** | **支持表单** | **安全** | **幂等** |**可缓存** |
| -------- | -------- | -------------- | -------------- | ------------ | -------- | -------- | ---- |
| GET | 请求资源 | 否 | 是 | 是 | 是 | 是 | 是 |
| HEAD | 请求资源头部 | 否 | 否 | 否 | 是 | 是 | 是 |
| POST | 发送数据，主体类型由Content-Type指定 | 是 | 是 | 是 | 否 | 否 | 显式声明缓存 |
| PUT | 发送负载创建或替换目标资源 | 是 | 否 | 否 | 否 | 是 | 否 |
| DELETE | 删除资源 | 不限 | 不限 | 否 | 否 | 是 | 否 |
| CONNECT | 创建点到点沟通隧道 | 否 | 是 | 否 | 否 | 否 | 否 |
| OPTIONS | 检测服务端支持方法 | 否 | 是 | 否 | 是 | 是 | 否 |
| TRACE | 消息环回测试，多用于路由检测 | 否 | 否 | 否 | 是 | 是 | 否 |
| PATCH | 部分修改资源 | 是 | 是 | 否 | 否 | 否 | 否 |

## 常见的 HTTP 请求头和响应头有哪些？

| **请求头**                | **描述**                                      | **示例**                                                     |
| ------------------------- | --------------------------------------------- | ------------------------------------------------------------ |
| Accept                    | 用户代理支持的MIME类型列表                    | Accept: text/html,application/xhtml+xml,application/xml;<br/>q=0.9,image/avif,image/webp,image/apng,/;<br/>q=0.8,application/signed-exchange;<br/>v=b3;<br/>q=0.9 |
| Accept-Encoding           | 用户代理支持的压缩方法（优先级）              | Accept-Encoding: br, gzip, deflate                           |
| Accept-Language           | 用户代理期望的语言（优先级）                  | Accept-Language: zh-CN,zh;q=0.9                              |
| Cache-Control             | 缓存机制                                      | Cache-Control: max-age=0                                     |
| Connection                | 是否持久连接                                  | Connection: keep-alive                                       |
| Cookie                    | HTTP cookies                                  | 服务器通过Set-Cookie存储到客户端的 Cookie                    |
| Host                      | 主机名 + 端口号                               | Host: 127.0.0.1:8080                                         |
| If-Match                  | 请求指定标识符资源                            | If-Match: "56a88df57772gt555gr5469a32ee75d65dcwq989"         |
| If-Modified-Since         | 请求指定时间修改过的资源                      | If-Modified-Since: Wed, 19 Oct 2020 17:32:00 GMT             |
| If-None-Match             | 请求非指定标识符资源                          | If-None-Match: "56a88df57772gt555gr5469a32ee75d65dcwq989"    |
| Upgrade-Insecure-Requests | 客户端优先接受加密和有身份验证的响应，支持CSP | Upgrade-Insecure-Requests: 1                                 |
| User-Agent                | 用户代理                                      | User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.150 Safari/537.36<br/> |
| Vary                      | 缓存策略                                      | Vary: User-Agent 常用于自适应缓存配置和 SEO                  |

| **响应头**       | **描述**                 | **示例**                                                     |
| ---------------- | ------------------------ | ------------------------------------------------------------ |
| Cache-Control    | 缓存机制                 | Cache-Control: public, max-age=3600                          |
| Connection       | 是否持久连接             | Connection: keep-alive                                       |
| Content-Encoding | 内容编码方式             | Content-Encoding: br                                         |
| Content-Type     | 内容的MIME类型           | Content-Type: text/html; charset=UTF-8                       |
| Date             | 报文创建时间             | Date: Sun, 28 Feb 2021 11:52:51 GMT                          |
| Expires          | 资源过期时间             | Expires: Sun, 28 Feb 2021 12:52:51 GMT                       |
| ETag             | 资源标识符               | ETag: "56a88df57772gt555gr5469a32ee75d65dcwq989"             |
| Set-Cookie       | 服务端向客户端发送Cookie | Set-Cookie: __yjs_duid=1_7a24a73cae0e4926e7604ec1fd9277eb1614513171854; expires=Tue, 30-Mar-21 11:52:51 GMT; Path=/; Domain=baidu.com; HttpOnly<br/><br/> |

## 什么是 HTTPS，与 HTTP 的区别？

### 什么是 HTTPS？

HTTPS 的全称是 Hyper Text Transfer Protocol over SecureSocket Layer，基于安全套接字协议 SSL，提供传输加密和身份认证保证传输的安全性，通过证书确认网站的真实性

HTTP2.0 和 HTTP3.0 都只用于https://网址

### HTTPS 三次握手

### ![image.png](https://pic.leetcode-cn.com/1616745009-Dqjpkm-image.png)

① Client Hello：客户端将支持 SSL 版本、加密算法、密钥交换算法等发送服务端

② Server Hello：服务端确定 SSL 版本、算法、会话 ID 发给客户端

③ Certificate：服务端将携带公钥的数字证书发给客户端

④ Server Hello Done：通知客户端版本和加密套件发完，准备交换密钥

⑤ Client Key Exchange：客户端验证证书合法性，随机生成premaster secret用公钥加密发给服务端

⑥ Change Cipher Spec：通知服务端后续报文采用协商好的密钥和加密套件

⑦ Finished：客户端用密钥和加密套件计算已交互消息的 Hash 值发给服务端。服务端进行同样计算，与收到的客户端消息解密比较，相同则协商成功

⑧ Change Cipher Spec：通知客户端后续报文采用协商好的密钥和加密套件

⑨ Finished：服务端用密钥和加密套件计算已交互消息的 Hash 值发给服务端。客户端进行同样计算，与收到的服务端消息解密比较，相同则协商成功

### HTTP 与 HTTPS 对比

| **项目**   | **HTTP**       | **HTTPS**      |
| ---------- | -------------- | -------------- |
| 默认端口   | 80             | 443            |
| HTTP 版本  | 1.0 - 1.1      | 2 - 3          |
| 传输       | 明文，易被劫持 | 加密，不易劫持 |
| 浏览器标识 | 不安全         | 安全           |
| CA 认证    | 不支持         | 支持           |
| SEO        | 无优待         | 优先           |

## 对比 HTTP1.0/HTTP1.1/HTTP2.0/HTTP3.0？

### HTTP1.0

无状态，无连接的应用层协议

- 无法复用连接
  每次发送请求，都要新建连接
- 队头阻塞
  下个请求必须在上个请求响应到达后发送。如果上个请求响应丢失，则后面请求被阻塞
  HTTP1.1
  HTTP1.1 继承了 HTTP1.0 简单，克服了 HTTP1.0 性能上的问题
- 长连接
新增Connection: keep-alive保持永久连接
- 管道化
支持管道化请求，请求可以并行传输，但响应顺序应与请求顺序相同。实际场景中，浏览器采用建立多个TCP会话的方式，实现真正的并行，通过域名限制最大会话数量
- 缓存处理
新增Cache-control，支持强缓存和协商缓存
- 断点续传
- 主机头
新增Host字段，使得一个服务器创建多个站点

### **HTTP2.0**

HTTP2.0进一步改善了传输性能

- 二进制分帧
在应用层和传输层间增加二进制分帧层
- 多路复用
建立双向字节流，帧头部包含所属流 ID，帧可以乱序发送，数据流可设优先级和依赖。从而实现一个 TCP 会话上进行任意数量的HTTP请求，真正的并行传输
- 头部压缩
压缩算法编码原来纯文本发送的请求头，通讯双方各自缓存一份头部元数据表，避免传输重复头
- 服务器推送
服务端可主动向客户端推送资源，无需客户端请求

### **HTTP3.0**

当一个 TCP 会丢包时，整个会话都要等待重传，后面数据都被阻塞。这是由于 TCP 本身的局限性导致的。HTTP3.0 基于 UDP 协议，解决 TCP 的局限性

- 0-RTT
缓存当前会话上下文，下次恢复会话时，只需要将之前缓存传递给服务器，验证通过，即可传输数据
- 多路复用
一个会话的多个流间不存在依赖，丢包只需要重发包，不需要重传整个连接
- 更好的移动端表现
移动端 IP 经常变化，影响 TCP 传输，HTTP3.0 通过 ID 识别连接，只要 ID 不变，就能快速连接
- 加密认证的根文
TCP 协议头没有加密和认证，HTTP3.0 的包中几乎所有报文都要经过认证，主体经过加密，有效防窃听，注入和篡改
- 向前纠错机制
每个包还包含其他数据包的数据，少量丢包可通过其他包的冗余数据直接组装而无需重传。数据发送上限降低，但有效减少了丢包重传所需时间

## Cookie 和 Session 的区别？

| **项目**   | **Cookie**               | **Session**                                                  |
| ---------- | ------------------------ | ------------------------------------------------------------ |
| 存取值类型 | 字符串                   | 大多数类型                                                   |
| 存取位置   | 客户端                   | 服务端，sessionId 非主动传参时，依赖 Cookie                  |
| 存取方式   | 文件                     | 文件、内存、关系或非关系型数据库等                           |
| 大小       | 受客户端限制             | 自行配置                                                     |
| 过期时间   | 写入时设置，用户可清除   | 自行配置，用户可清除对应Cookie，服务端自动清除过期 Session   |
| 兼容性     | 需浏览器开启，用户同意   | 不依赖 Cookie 时，通过 Get 或自定请求字段传入                |
| 作用范围   | 可设置跨子域，不可跨主域 | 用户身份唯一标识符不变时，可跨域，跨服务器。默认受限于 Cookie，仅限会话期间有效 |



# CSS

### CSS 中哪些选择器？

| **选择器**   | **示例**       |
| :----------- | :------------- |
| 通配符选择器 | *              |
| ID选择器     | #id            |
| 类选择器     | .class         |
| 元素选择器   | div            |
| 属性选择器   | [attr="value"] |
| 伪类选择器   | a:hover        |
| 伪元素选择器 | a::before      |



### CSS 选择器有哪些组合方式？

① 运算符组合选择器（关系选择器）

| **选择器**     | **示例** |
| :------------- | :------- |
| 后代选择器     | #id a    |
| 子代选择器     | #id > a  |
| 相邻兄弟选择器 | #id + a  |
| 通用兄弟选择器 | #id ~ a  |

② 属性组合选择器

| **选择器**       | **示例**              |
| :--------------- | :-------------------- |
| 标签属性选择器   | input[type="button"]  |
| 类属性选择器     | .class[type="button"] |
| 通配符属性选择器 | *[type="button"]      |

③ 群组选择器

| **选择器** | **示例**          |
| :--------- | :---------------- |
| 交集选择器 | .class#id, div#id |
| 并集选择器 | #id, .class, div  |

### 对比伪类，伪元素

伪类和伪元素都是选择器，适用于：

- 无法更改 HTML
- 要选择的元素不固定
- 要选择的元素状态或位置关系固定
  示例：
  `::first-line`可以选中块级元素的第一行。由于内容、设备宽度变化，第一行的内容也在变化。比起更改 HTML，直接定位某个元素的第一行在实际应用中更加方便。

两者区别如下：

#### 伪类

用于选择元素的**特定状态**。单冒号开头。分为：

- 位置关系伪类举例：

  | **选择器**     | **选取范围**           |
  | :------------- | :--------------------- |
  | :first-child   | 第一个元素             |
  | :last-child    | 最后一个元素           |
  | :first-of-type | 第一个指定类型的元素   |
  | :last-of-type  | 最后一个指定类型的元素 |

- 用户行为伪类举例：

  | **选择器** | **选取范围**   |
  | :--------- | :------------- |
  | :hover     | 被用户鼠标指向 |
  | :checked   | 被用户选中     |

- 其它状态伪类举例：

  | **选择器** | **选取范围**         |
  | :--------- | :------------------- |
  | :enabled   | 当前元素可用         |
  | :lang(en)  | 选取语言为英文的元素 |

#### 伪元素

用于选择元素的 **特定部分**。双冒号开头。举例：

| **选择器**     | **选取范围**         | **用途**          |
| :------------- | :------------------- | :---------------- |
| ::first-letter | 第1行的第1字母       | 首字下沉          |
| ::first-line   | 第1行                | 首行缩进          |
| ::before       | 元素的第一个子元素   | 配合 content 使用 |
| ::after        | 元素的最后一个子元素 | 配合 content 使用 |
| ::placeholder  | 表单默认文本         | 更改提示样式      |

为兼容早期浏览器，实际使用时，多将双冒号写作单冒号

原因：早期规范没有明确伪元素必须双冒号，当时浏览器多用单冒号，现代浏览器同时兼容单和双冒号

如果不考虑兼容，规范应使用双冒号



### CSS 选择器命名规则

大小写字母，数字，连字符（-），下划线（_）以及 ISO 10646 字符编码 U+00A1 及以上

- 不能以数字、连字符（-）开头
  - 类名或 ID 以数字、连字符（-）开头`.`或`#`后加`\3`
- ID 选择器以一个（#）开头
- 类选择器以一个（.）开头

### 

### 如何优化选择器，提高性能？

- CSS 选择器效率从高到底：

> ID > 类 > 类型（标签） > 相邻 > 子代 > 后代 > 通配符 > 属性 > 伪类

- CSS 选择器的读取顺序：**从右到左**，逐级匹配

示例：

> `.content * {}`，会先匹配到所有元素，再向上筛选出父元素 `className` 为 `content` 的子元素

根据效率和读取顺序，提高性能的方法如下：

1. 多使用高效率选择器
   工作中，多使用类名，少使用关系、伪类选择器，可读和可维护性也会提高

2. 减少选择器层级
   工作中，选择器层次不建议超过 4 级。用 SASS 或 LESS 时，应避免不必要的嵌套

3. 高效率选择器在前
   工作中，避免使用类、标签选择器限制 ID 选择器，避免使用标签选择器限制类选择器。
   低效率选择器在前，通常可以优化。示例 `.content #id {}` 可直接写成 `#id {}`，`div .content` 可以给 `div` 起类名

4. 避免使用通配符选择器
   通配符选择器效率低，且使用 `*` 的场景，通常有其他解决方案

5. 多用 **继承**
   示例：

   ```
   <style>
   div * { /* bad case */
   font-size: 12px;
   }
   div { /* good case */
   font-size: 12px;
   }
   </style>
    
   <div>
   <p></p>
   <p></p>
   <p></p>
   </div>
   ```

### 什么是继承？

CSS 属性分为非继承属性和 **继承属性**，继承属性的默认值为父元素的该属性的 **计算值**，非继承属性和根元素的继承属性的默认值为初始值。

对于非继承属性，可以显示的声明属性值为 `inherit`，让子元素的属性继承父元素。

常见的继承属性：

- 字体 `font` 系列
- 文本 `text-align` `text-ident` `line-height` `letter-spacing`
- 颜色 `color`
- 列表 `list-style`
- 可见性 `visibility`
- 光标 `cursor`

容易被误认为继承属性的 **非继承属性**：

- 透明度 `opacity`
- 背景 `background`系列

### 如何重置元素的属性值到初始值？

属性值`initial`可以将属性设为W3C**规范初始值**

属性`all`可以将元素的所有属性重置

在规范之外，浏览器还会为部分元素，如表单元素设置默认样式

属性的值来源于开发者定义，用户配置和浏览器默认

`all:initial`相当于清空了用户配置和浏览器默认样式

工作中，我们更希望重置到默认样式，而不是清空它们

`all:revert`属性还原。可以将子元素的属性重置按如下规则重置：

- 继承属性：重置到父元素的属性值
- 非继承属性或父元素继承属性都未设置：重置到用户配置和浏览器默认属性

示例：

```
<style>
button {
color: yellow;
border: 1px solid red;
background-color: red;
}
button:nth-of-type(2) {
all: initial; /* 清空按钮的样式 */
color: blue;
}
button:last-of-type {
all: revert; /* 保留按钮的默认样式 */
color: blue;
}
</style>
<button>按钮1</button>
<button>按钮2</button>
<button>按钮3</button>
```

### 为什么要定义 CSS 优先级？

同一元素可能会被多个 CSS 选择器选中。

选择器中可能包含对相同属性的不同设置值。

需要定义优先级规则，只让优先级最高的选择器的设置值生效。

### CSS 优先级规则是什么？

选择器与元素的**相关度越高，优先级越高**，具体规则如下：

- 开发者定义选择器 > 用户定义选择器 > 浏览器默认选择器

- 内联样式（`style=""`） > 内（`<style>`）、外部样式（`<link />`）

- ID 选择器 > 类选择器、属性选择器、伪类选择器 > 类型选择器、伪元素选择器

- 相同优先级，书写顺序后 > 前

- 同级选择器，复合选择器 > 单选择器

- 自身的选择器 > 继承自父级的选择器

- 用户配置 `!important` 声明 > 开发者 `!important` 声明 > 其它

### !important 的作用和弊端，如何避免？

- 作用
  `!important` 可以忽略选择器 CSS 选择器优先级，让声明的属性总是生效
- 弊端
  - 破坏原 CSS 级联规则，增加调试难度
  - 修改样式变得困难
  - 污染全局样式
- 避免
  - 用 CSS 选择器优先级解决样式冲突
  - 不在全局、会被复用的插件中使用 `!important`
  - 通过 CSS 命名或 Shadow DOM 限制 CSS 作用域

### 如何计算 CSS 选择器的优先级？

我们将 CSS 选择器优先级量化为权重，为不同类型的 CSS 选择器设置初始权重。

选择器的组合，即初始权重的累加。累加值越高，优先级越高。

初始权重：

| **选择器**           | **权重** |
| :------------------- | :------- |
| 行间 style=""属性    | 1000     |
| ID选择器             | 100      |
| 类、属性、伪类       | 10       |
| 类型（标签）、伪元素 | 1        |

可以**重复选择器，增加优先级，但累加结果*不进位***，示例：

```
<style>
div { /* 优先级 1 */
color: red;
}
div.c { /* 优先级 1 + 10 = 11 */
color: gray;
}
.c.c.c.c.c.c.c.c.c.c.c { /* 优先级 10 * 11 = 110，不进位，90 */
color: blue;
}
#id { /* 优先级 100 */
color: yellow
}
</style>
<div>红字</div>
<div class="c">蓝字</div>
<div id="id" class="c">黄字</div>
```

### 如何限制 CSS 选择器的作用域？

- 通过 CSS 命名限制
  通过后代选择器，将一组 CSS 选择器放入命名唯一的父级选择器中
  唯一命名，可以自己指定，依靠工程化工具，通过 hash 或页面路径等规则，自动生成
  CSS 变量的作用域由选择器，例如最常用的根选择器 `:root`

  示例：

  ```
  <style>
  .unique-name p {
  /* 这里设置的样式，不会影响其他p标签 */
  }
  </style>
  <div class="unique-name">
  <p></p>
  </div>
  <div>
  <p></p>
  </div>
  ```

- 通过 Shadow DOM 限制
  通过 `JS` 给已有元素创建影子 DOM，将样式通过 `<style>` 标签写入影子 DOM

  示例：

  ```
  <div></div>
  <p></p>
   
  <script>
  const shadowDom = document.querySelector('div').attachShadow({mode: 'open'})
  shadowDom.innerHTML = '<p></p>'
  shadowDom.innerHTML = `<style>p {
  /* 这里设置的样式，并不会影响原来的p标签 */
  }</style>
  </script>
  ```

- 通过 `@document` 限制
  通过 `CSS at-rule` 新增的 `@document` 限制 CSS 仅在被满足条件的 URL 生效
  可以限定 URL 的地址，前缀，域名和子域名或者根据正则匹配

- 通过 CSS Modules 限制
  将 CSS 作为资源引入，CSS Modules 会根据内容生成哈希字符串，字符串可以作为独一无二的类名

### CSS 中有哪些单位？

## 绝对长度单位

| **单位** | **名称** | **场景**   |
| :------- | :------- | :--------- |
| px       | 像素     | 屏幕       |
| pt       | 点       | 打印、UI稿 |

换算：1pt = 96 / 72 px

常见：9pt = 12px

此外，浏览器还支持打印常用单位

```
cm`、`mm`、`in`、`pc` 和 `Q
```

## 相对长度单位

| **单位** | **名称**                                               | **场景**   |
| :------- | :----------------------------------------------------- | :--------- |
| em       | font-size：相对父元素 width 等：相对于自身的 font-size | 自适应布局 |
| rem      | 相对于根元素的字体大小                                 | 移动端     |
| vw       | 视窗宽度1%                                             |            |
| vh       | 视窗高度1%                                             | 高度自适应 |

此外，浏览器还支持`ex`、`ch`、`lh`、`vmin` 和 `vmax`

### 百分比 % 相对于谁？

百分比总是相对于父元素，无论是设置 `font-size` 或 `width` 等。如果父元素的相应属性，经浏览器计算后，仍无绝对值，那么 % 的实际效果等同于 默认值，如 `height: 100%`

### 如何使用 rem 自适应布局？

```html
/* 假设UI稿的设计宽度是720px，默认字号是16px */
<style>
div { font-size: 1rem; }
</style>
<div>您好</div>
<script>
function rem () {
document.documentElement.style.fontSize = document.body.clientWidth / 720 * 16 + 'px'
}
window.onresize = rem
rem()
</script>

```

### 颜色值都有哪几种表示方法？

**关键字**

- 颜色关键字：`white`等
- 透明：`transparent`
- 当前颜色：`currentcolor`

**RGB color model**

- 十六进制 `RGB`：`#fffff` 简写：`#fff`
- 十六进制 `RGBA`：`#ffffffff` 简写:`#ffff`
- 函数 `rgb()`：`rgb(255, 255, 255)`
- 函数 `rgba()`：`rgba(255, 255, 255, 1)`

**HSL color model**

- 函数 `hsl()`：`hsl(0, 0%, 100%)`
- 函数 `hsla()`：`hsla(0, 0%, 100%, 1)`



### 什么是盒模型？

盒模型由内向外：内容 `content` + 内边距 `padding` + 边框 `border` + 外边距 `margin`

分为两类：

1. 标准盒模型(**默认**)：`border-sizing:content-box`
   `width` 和 `height` 设置内容 `content` 的宽和高
2. 替代盒模型：`border-sizing:border-box`
   `width` 和 `height` 设置内容 `content` + 内边距 `padding` + 边框 `border` 的宽和高

### 对比块、内联和内联块盒子

**块盒子**：`display:block`

- 换行
- `width`和`height`生效
- 竖直方向`padding`和`margin`生效

**内联盒子**：`display:inline`

- 不换行
- `width`和`height`无效
- 竖直方向`padding`和`margin`无效

**内联块盒子**：`display:inline-block`

- 不换行
- `width`和`height`生效
- 竖直方向`padding`和`margin`生效

### 浏览器默认是如何布局的？

`流布局`是浏览器布局的基本方式。包括**块布局**和**内联布局**。

浏览器根据书写顺序`writing-mode`，决定**块布局**和**内联布局**方向

`horizontal-tb`是`writing-mode`默认值，也是中英文的常用书写顺序

**块布局** 的方向从上到下，即块盒子从上到下换行，相邻块盒子的外边距折叠

**内联布局** 的方向由文本、列表水平对齐方向`direction`决定

- 左对齐`ltr`时，内联盒子在同一行，从左到右排列

- 右对齐`rtl`时，内联盒子在同一行，从右到左排列



### 弹性盒模型——flex

弹性盒模型基于盒模型，其宽度、高度、外边距都可以弹性变化，以适应弹性布局

可以给父元素设置 `display:flex` 或 `display:inline-flex`，让子元素成为弹性盒子

### 什么是弹性布局？

给父元素设置`display:flex`，父元素表现为块盒子，开启弹性布局。

给父元素设置`diisplay:inline-flex`，父元素表现为内联块盒子，开启弹性布局。

区别于默认布局，弹性布局中：

1. 子元素成为弹性盒子，宽度、高度、外边距可以弹性变化，自适应父元素的尺寸

2. 子元素可以在垂直、水平方向上，正向或反向排列

3. 父元素通过`justify-content`决定子元素在主轴的对齐方式

4. 父元素通过`align-items`决定子元素在交叉轴的对齐方式

5. 父元素通过`align-content`决定多行子元素整体在交叉轴的对齐方式

6. 子元素通过`align-self`决定自身在交叉轴的对齐方式

### 什么是块级格式上下文——BFC？

##### 定义

块级格式上下文，英文全称是 Block Formatting Context，简称 BFC

它声明了一块布局区域，浏览器对区域内盒子按照一定方式布局，包括默认布局、弹性布局、网格布局、表格布局等

- 默认布局时，区域高度包含浮动元素高度
- 不同区域间相互独立，区域内的盒子和区域外的盒子互不影响
- 不同区域不会发生外边距折叠

##### 创建

我们可以根据布局、溢出处理和有限布局，用不同方法创建块级格式上下文

- 根元素`<html>`
- **无副作用**：`display:flow-root`
- 默认布局
  - 绝对定位：`position:absolute`和`position:fixed`
  - 浮动：`float:left``float:right`
  - 行内块元素：`display: inline-block`
- 溢出处理
  - `overflow:hidden`隐藏滚动条，裁剪溢出内容
  - `overflow:scroll`显示滚动条，裁剪溢出内容
  - `overflow:auto`未溢出，隐藏滚动条。溢出，显示滚动条
- 有限布局
  
  - `contain`属性值不为`none`
  
    CSS [contain 属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain)允许开发者声明当前元素和它的内容尽可能的独立于 DOM 树的其他部分。
- 弹性布局
  - `display:flex`直接子元素
  - `display:inline-flex`直接子元素
- 网格布局
  - `display:gird`直接子元素
  - `display:inline-gird`直接子元素
- 多列布局（分栏布局）
  - `column-count`分栏数属性值不为`auto`
  - `column-width`分栏列宽属性值不为`auto`
  - `column-span:all`跨越所有列，表现为不分栏
- 表格布局
  - `display:table`表格
  - `display:inline-table`内联表格
  - `display:table-cell`单元格
  - `display:table-caption`表格标题
  - `display:table-row`行
  - `display:table-row-group`tbody
  - `display:table-header-group`thead
  - `display:table-footer-group`tfoot

##### 用途

通过创建块级格式上下文，我们可以：

- 清除浮动
- 解决外边距折叠
- 限定布局范围，提高渲染性能



### 什么是外边距折叠，如何避免？

`相邻块盒子的上下边距没有累加，而是重叠取其中最大值的现象，称为外边距折叠`

常见的外边距折叠：

1. 上下相邻块盒子的间距 = 上盒子下边距和下盒子上边距的最大值

   ```
   <style>
   div:first-of-type {
   margin-bottom: 10px;
   /* display: inline-block; 解决方法：将任意盒子转为内联块盒子 */
   }
   div:last-of-type {
   margin-top: 10px;
   }
   </style>
   <!-- 两个DIV的实际间距 10px -->
   <div>1</div>
   <div>2</div>
   ```

   避免：将其中一个盒子转为内联块盒子

2. 空块盒子

   其上边距 <= 上盒子下边距，其下边距 <= 下盒子的上边距，相当于不存在

   上下块盒子的间距 = 上盒子下边距 和 下盒子上边距的最大值

   ```
   <style>
   div:first-of-type {//等同于 :nth-of-type(1)。
   margin-bottom: 10px
   }
   div:nth-of-type(2) {
   margin-top: 10px;
   margin-bottom: 10px;
   }
   /* 解决方法之1：通过伪元素增加内容
   div:nth-of-type(2)::before {
   content: '内容';
   display: block;
   } */
   div:last-of-type {
   margin-top: 10px
   }
   </style>
   <!-- 中间的空DIV，只要上外边距<=10px，下外边距<=10px，都不影响1和3的实际间距 -->
   <div>1</div>
   <div><!-- 内容 解决方法之2：直接增加内容 --></div>
   <div>3</div>
   ```

   避免：增加内容，直接写入或使用伪元素

3. 没有触发BFC，边框，内边距和内容的父块盒子与子块盒子

   

   父块盒子的上边距 = 父块盒子的上边距 和 子块盒子的上边距的最大值

   父块盒子的下边距 = 父块盒子的下边距 和 子块盒子的下边距的最大值

   

   ```
   
   <style>
   .parent {
   margin-top: 10px;
   margin-bottom: 10px;
   /* overflow: hidden; 解决方法之1：触发BFC*/
   /* border: 1px solid black; 解决方法之2：设置边框*/
   /* paddding-top: 1px; 解决方法之3：设置内边距*/
   }
   .sub {
   margin-top: 10px;
   margin-bottom: 10px;
   }
   </style>
   <!-- 父块盒子和子块盒子的上外边距都是10px，下外边距都是10px -->
   <div class="parent">
   <!-- <div>内容</div> 解决方法之4：增加内容 -->
   <div class="sub"></div>
   </div>
   ```

   避免外边距重叠：

   - 设置边框
   - 设置内边距
   - 增加内容，直接写入或使用伪元素
   - 触发 BFC 块级格式上下文的方法都可以



### 有哪些定位方式？

(常见 “父相子绝”、”子绝父相“)

- 静态定位：`position:static`

- 相对定位：`position:relative`

  - 创建BFC和定位上下文
  - 当`z-index`不为`auto`时，创建层叠上下文

- 绝对定位：`position:absolute`

  - 创建BFC和定位上下文
  - 当`z-index`不为`auto`时，创建层叠上下文

- 固定定位：`position:fixed`

  ，相对`transfrom`、`perspective`和`filter`

  不为`none`的最近父元素，没有是**视窗**

  - 创建BFC和定位上下文
  - 创建层叠上下文

- 粘性定位：`position:sticky`

  - 创建定位上下文

  - 创建层叠上下文

  - 滚动最近`overflow`

    不为`visible`父元素

    - 未被卷曲：表现为未创建BFC的相对定位
    - 将被卷曲：表现为绝对定位



### 什么是定位上下文？

`position`不为`static`时，可以通过`top``right``bottom``left`设置元素位置偏移量，并且不会影响其它元素的位置

定位上下文，决定元素相对于哪个父元素偏移

非静态定位元素，设置偏移量后

- 相对于最近的非静态定位的父元素偏移
- 没有，则相对于根元素`<html>`的父级，即**视窗**偏移

可以给父元素设置`position`不为`static`改变定位上下文，决定子元素相对于谁偏移

其中`position:relative`对父元素的副作用最小

**子绝父相**常用于组件内部的绝对定位，而不影响组件外元素的位置关系



### 什么是层叠上下文？

##### 定义

层叠上下文是元素在Z轴上的层次关系集合并影响渲染顺序，设置`z-index`可改变`position`不为`static`的元素的层叠顺序

层叠上下文中父元素层级决定了子元素层级，兄弟元素间的层级由`z-index`影响

##### 创建

- 根元素`html`

- 无副作用：`isolation`不为`auto`

- 定位：

  ```
  position
  ```

  不

  ```
  static
  ```

  - `fixed`和`sticky`一定创建
  - `relative`和`abosulte`当`z-index`不为`auto`时创建

- 显示类型：弹性布局和网格布局的子元素，`z-index`不为`auto`时创建

- 透明元素：`opacity`< 1

- 变形：`transform`不为`none`

- 滤镜：`filter`不为`none`

- 性能优化：

  - `contain`包含`layout`或`paint`
  - `will-change`不为`auto`

##### 用途

理解层叠上下文，设置`z-index`，常用来：

- 改善兼容性
  - 解决遮挡问题
  - 解决滚动穿透问题
- 提升移动端体验
  - 如通过`-webkit-overflow-scrolling: touch`增加滚动回弹效果
- 性能优化
  - 将频繁变化的内容单独一层放置

### 什么是浮动，如何清除浮动？

##### 定义

在默认布局流中，浮动元素脱离文档流，沿内联布局的开始或结束位置放置

与绝对和固定定位不同，浮动元素的宽高、内外边距和边框，依然影响相邻元素的位置，相邻元素环绕浮动元素流式布局

##### 创建

- `float`不为`none`即可创建浮动元素
- 弹性布局的父元素不能浮动

##### 清除浮动

浮动元素脱离文档流，只包含浮动元素的父元素高度为`0`，带来问题

- 父元素高度不会随内容高度变化，内外边距、边框和背景无法自适应内容
- 父元素后的元素，与父元素内的浮动元素重叠
- 父元素后的元素，外边距属性无效

解决问题的思路：

- 设置高度

  - 通过`JS`将父元素高度设为获取浮动元素的最大高度
  - 通过`CSS`将父元素高度`height`设置固定值，然后设置溢出属性`overflow`，裁剪超出高度的部分或添加滚动条

- 清除浮动

  - 通过`HTML`在父元素内部末尾添加清除浮动的元素

    ```
    <style>
    .box > div {
    float: left;
    width: 33.33%;
    }
    .clearfix {
    clear: both;
    }
    .margin-top {
    margin-top: 10px;
    }
    </style>
    <div class="box">
    <div></div>
    <div></div>
    <div></div>
    <div class="clearfix"></div>
    </div>
    <div class="margin-top"></div>
    ```

    - 通过`CSS`在父元素内部末尾添加请出浮动的伪元素

      ```
      <style>
      .box::after {
      /** 清除浮动 **/
      content: '　';
      display: block;
      clear: both;
      /** 隐藏 **/
      visibility:hidden;
      width:0;
      height:0;
      /** 隐藏：兼容IE低版本 **/
      line-height: 0;
      }
      .margin-top {
      margin-top: 10px;
      }
      </style>
      <div class="box">
      <div></div>
      <div></div>
      <div></div>
      </div>
      <div class="margin-top"></div>
      ```

    - 通过`HTML`在父元素下边添加清除浮动的元素（解决外边距问题）

      ```
      <style>
      .box > div {
      float: left;
      width: 33.33%;
      }
      .clearfix {
      clear: both;
      }
      .margin-top {
      margin-top: 10px;
      }
      </style>
      <div class="box">
      <div></div>
      <div></div>
      <div></div>
      </div>
      <div class="clearfix"></div>
      <div class="margin-top"></div><!-- 清除浮动后，外边距生效 -->
      ```



### 什么是滚动穿透，如何解决？

滚动穿透是在移动端，尤其是 iOS 中，弹出模态框，用户模态框中的滚动/滑动操作，穿过弹窗，导致页面滚动的现象

滚动穿透不是 BUG，只是运行环境对溢出、滑动事件处理略有差异

```html
<style>
body {margin:0;}
.fixed {
overflow: auto;
margin: auto;
position: fixed;
width: 50vw;
height: 50vh;
top: 0;
right: 0;
bottom: 0;
left: 0;
background-color: #efefef;
}
.content, .list {
height: 100vh;
background-image: linear-gradient(to bottom, #efefef, #000);
}
.list {
height: 200vh;
}
</style>
<body>
<div class="list"></div>
<div class="fixed">
<div class="content"></div>
</div>
</body>

```

请在真实的移动浏览器中，滑动中间的弹窗，你可能会发现页面也跟随滚动
具体表现因浏览器而异

在 CSS 中，有两个属性：

- `pointer-events: none`禁止元素成为鼠标事件的目标对象
- `touch-action: none`禁止元素响应任何触摸事件
  它们都不能阻止滚动事件，冒泡到父级，让父级继续滚动

解决滚动穿透问题，比较的流行的方法是：

- 当模态框弹出时，通过`position:fixed`创建层叠上下文，让不该滚动的父级的脱离文档流，设置`width:100%`保留宽度，并将`body`的卷曲高度保存起来。
- 当模态框关闭时，通过`position:static`，让父级回归文档流，恢复之前的卷曲高度，即滚动位置
  让我们解决示例的滚动穿透问题：

```html
<style>
body {margin: 0;}
.modal {
display: none;
overflow: auto;
margin: auto;
position: fixed;
width: 50vw;
height: 50vh;
top: 0;
right: 0;
bottom: 0;
left: 0;
background-color: #efefef;
}
.content, .list {
height: 100vh;
background-image: linear-gradient(to bottom, #efefef, #000);
}
.list {
height: 200vh;
}
.open, .close {
position: fixed;
text-align: center;
}
</style>
<body>
<div class="open">打开</div>
<div class="list"></div>
<div class="modal">
<div class="close">关闭</div>
<div class="content"></div>
</div>
</body>
<script>
var openBt = document.querySelector('.open'),
closeBt = document.querySelector('.close'),
modal = document.querySelector('.modal'),
list = document.querySelector('.list'),
scrollTop = 0
openBt.onclick = function() {
scrollTop = document.body.scrollTop || document.documentElement.scrollTop
modal.style.display = 'block'
list.style.cssText = 'position: fixed; width: 100%; top: -' + scrollTop + 'px'
}
closeBt.onclick = function() {
modal.style.display = 'none'
list.style.cssText = 'position: static;'
window.scroll({ top: scrollTop })
}
</script>

```

### 多方法实现水平居中

- `text-align: center`适合内联或内联块元素的父元素设置

- `position:staic`

  - `margin: 0 auto`适合宽度已知元素
  - 块元素`display:block`宽度固定`width:100px`
    - 块元素`display:block`宽度自适应内容`width:fit-content`
  - 表格元素`display:table`
  
- `position:relative`/`position:absolute`/`position:fixed`/`position:sticky`脱离文档流后
- 偏移`left:50%`或`right:50%`
  
  - `margin-left`或`margin-right`设置宽度一半
    - `transform: translateX(-50%)`

  - 偏移`left:0`和`right:0`
  
  - 宽度固定`width:100px`或自适应`width:fit-content`
  - `margin: 0 auto`

- `display:flex`父元素
- `justify-content: center`每行内部元素沿主轴居中排列



### 多方法实现垂直居中

- 内联父元素

  - `line-height: 100px`行高固定值

- 内联块或块元素

  - `line-height`等于 元素`height`
  - `padding-top`等于`padding-bottom`

- 内联或内联块元素

  - `vertical-align: middle`元素与行的基线 + 字母x的高度的一半对齐

- 表格单元格元素：`display: table-cell`或`<td>`

  - `vertical-align: middle`内容或子元素垂直居中

- `position:staic`

  - `margin-top: 50%`适合宽度已知元素`transform: translateY(-50%)`

    - 块元素`display:block`高度固定`height:100px`
    - 块元素`display:block`宽度自适应内容`height:fit-content`
    - 表格元素`display:table`

- `position:relative`/`position:absolute`/`position:fixed`/`position:sticky`脱离文档流后

  - 偏移`top:50%`或`bottom:50%`

    - `margin-top`或`margin-bottom`设置高度一半
    - `transform: translateY(-50%)`

  - 偏移`top:0`和`bottom:0`

    - 宽度固定`height:100px`或自适应`height:fit-content`
    - `margin: auto 0`

- `display:flex`父元素

  - `align-content: center`多行内部元素整体沿交叉轴居中排列
  - `align-items: center`每行内部元素整体沿交叉轴居中排列
  - `align-self: center`单个内部元素沿交叉轴居中排列



### 多方法实现高度 100% 撑满视窗

如果视窗高度是变化的，纯CSS撑满视窗，可以用相对单位

- 百分比

  - 百分比相对于父元素设置，`height`默认为`auto`，即内容高度

    - 从根元素`html`向内到`body`，高度都设置为`height:100%`
```html
<style>
html, body {
height: 100%;
}
div {
  height: 100%;
  background-color: azure;
}
body { margin: 0; }
</style>
<div></div>
```

- vh，直接设置相对视窗高度

```html
<style>
    div {
        height: 100vh;
        background-color: azure;
    }
    body { margin: 0; }
</style>
<div></div>
```

如果对于内容高度，多用视窗高度减去固定元素高度（如导航栏，状态栏），可用函数`calc()`

- calc(100% - 50px)
- calc(100vh - 50px)



### 圣杯布局

在弹性布局以前，圣杯布局是通过浮动和定位实现三栏布局的一种方案之一

圣杯布局的特点：

- 上下为通栏，下通栏清浮动

- 中间为三栏布局，子元素按中左右的顺序浮动`float:left`

  - 宽度
    - 左边栏宽度固定 = 父元素的左内边距`padding-left`
    - 右边栏宽度固定 = 父元素的右内边距`padding-right`
    - 中间内容宽度 = 100%
  - 位置
    - 左右边栏相对定位`position:relative`
    - 左边栏左外边距`margin-left: -100%`，`right:`宽度
    - 右边栏左外边距`margin-left:`-宽度，`right:`-宽度
  - 注意
    - 需设置最小宽度`min-width`，避免视窗宽度过小，浮动错位
    

![image.png](https://pic.leetcode-cn.com/1617073125-zCVTaJ-image.png)

```html
<style>
body { margin: 0; }
div {
  text-align: center;
  color: #fff;
}
.header, .footer {
  height: 50px;
  background-color: pink;
  line-height: 50px;
}
.content {
  padding-left: 200px;
  padding-right: 150px;
  min-width: 500px;
  line-height: 500px;
}
.content > div {
  float: left;
  height: 500px;
}
.center {
  width: 100%;
  background-color: mediumturquoise;
}
.left, .right {
  position: relative;
}
.left {
  width: 200px;
  right: 200px;
  margin-left: -100%;
  background-color: skyblue;
}
.right {
  width: 150px;
  right: -150px;
  margin-left: -150px;
  background-color: lightsteelblue;
}
.footer {
cl  ear: both;
}
</style>
  <div class="header">header</div>
  <div class="content">
  <div class="center">center</div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
<div class="footer">footer</div>

```

### 双飞翼布局

双飞翼布局由淘宝UED发扬，是通过浮动和定位实现三栏布局的一种方案之一

双飞翼布局的特点：

- 上下为通栏，下通栏清浮动

- 中间为三栏布局，子元素按中左右的顺序浮动`float:left`

  - 宽度
    - 左边栏宽度固定 = 中间内容子元素左外边距`margin-left`
    - 右边栏宽度固定 = 中间内容子元素右外边距`margin-right`
    - 中间内容宽度 = 100%
  - 位置
    - 左边栏左外边距`margin-left: -100%`
    - 右边栏左外边距`margin-left:`-宽度

![image.png](https://pic.leetcode-cn.com/1617072875-ZXJcVU-image.png)

```html	
<style>
body { margin: 0; }
div {
  text-align: center;
  color: #fff;
}
.header, .footer {
  height: 50px;
  background-color: pink;
  line-height: 50px;
}
.content {
  padding-left: 200px;
  padding-right: 150px;
  min-width: 500px;
  line-height: 500px;
}
.content > div {
  float: left;
  height: 500px;
}
.center {
  width: 100%;
  background-color: mediumturquoise;
}
.left, .right {
  position: relative;
}
.left {
  width: 200px;
  right: 200px;
  margin-left: -100%;
  background-color: skyblue;
}
.right {
  width: 150px;
  right: -150px;
  margin-left: -150px;
  background-color: lightsteelblue;
}
.footer {
cl  ear: both;
}
</style>
  <div class="header">header</div>
  <div class="content">
    <div class="center">center</div>
    <div class="left">left</div>
    <div class="right">right</div>
  </div>
  <div class="footer">footer</div>

```

### 多方法实现三栏布局

按照出现顺序，三栏布局包括：

分栏布局、表格布局、流式布局、弹性布局、网格布局

- 分栏布局
  - `columns: 3`分成3列，`word-break: break-all;`强制换行
  - 内容满1列后，自动移到下一行

![image.png](https://pic.leetcode-cn.com/1617072931-wTGBZo-image.png)

[多方法实现水平居中](https://leetcode-cn.com/leetbook/read/html-css-interview/o72tmt/)[多方法实现垂直居中](https://leetcode-cn.com/leetbook/read/html-css-interview/o7nlpr/)[多方法实现高度 100% 撑满视窗](https://leetcode-cn.com/leetbook/read/html-css-interview/o7vdos/)[圣杯布局](https://leetcode-cn.com/leetbook/read/html-css-interview/o7wned/)[双飞翼布局](https://leetcode-cn.com/leetbook/read/html-css-interview/o7dm9h/)[多方法实现三栏布局](https://leetcode-cn.com/leetbook/read/html-css-interview/o70ox3/)[瀑布流布局](https://leetcode-cn.com/leetbook/read/html-css-interview/o775hm/)[品布局](https://leetcode-cn.com/leetbook/read/html-css-interview/o77jj1/)[实现简易计算器](https://leetcode-cn.com/leetbook/read/html-css-interview/o7lyb7/)[视差滚动](https://leetcode-cn.com/leetbook/read/html-css-interview/o7isc5/)

[动画](https://leetcode-cn.com/leetbook/read/html-css-interview/o021gg/)



[原理](https://leetcode-cn.com/leetbook/read/html-css-interview/o0nzkv/)



[设计](https://leetcode-cn.com/leetbook/read/html-css-interview/o0v6qj/)



[工程化](https://leetcode-cn.com/leetbook/read/html-css-interview/o0w83t/)



[方法论](https://leetcode-cn.com/leetbook/read/html-css-interview/o0di8r/)



[综合](https://leetcode-cn.com/leetbook/read/html-css-interview/o000us/)



### 多方法实现三栏布局

按照出现顺序，三栏布局包括：

分栏布局、表格布局、流式布局、弹性布局、网格布局

- 分栏布局
  - `columns: 3`分成3列，`word-break: break-all;`强制换行
  - 内容满1列后，自动移到下一行

![image.png](https://pic.leetcode-cn.com/1617072931-wTGBZo-image.png)

```html
<style>
.content {
  columns: 3;
  word-break: break-all;
  background-color: mediumturquoise;
  color: white;
}
</style>
 
<div class="content">contentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontentcontent</div>
```

- 表格布局
  - 宽度
    - 表格宽度 100%
    - 左单元格宽度固定
    - 右单元格宽度固定
  - 垂直居中
    - `vertical-align:middle`

![image.png](https://pic.leetcode-cn.com/1617072955-izDWJq-image.png)

```html
<style>
    .content {
        width: 100%;
        height: 500px;
        border-collapse: collapse;
        color: white;
        vertical-align: middle;
        text-align: center;
    }
    .left {
        width: 200px;
        background-color: skyblue;
    }
    .center {
        background-color: mediumturquoise;
    }
    .right {
        width: 150px;
        background-color: lightsteelblue;
    }
</style>
<table class="content">
    <tbody>
        <tr>
            <td class="left">left</td>
            <td class="center">center</td>
            <td class="right">right</td>
        </tr>
    </tbody>
</table>
```

- 流式布局
  - 圣杯布局（同上题）
  - 双飞翼布局（同上题）
- 弹性布局
  - 左/右边栏宽度固定：`flex-basis`初始宽度 或`width`宽度，前者优先级大于后者
  - 中间内容宽度：`flex-grow: 1`剩余空间分配比例 或 简写`flex: 1`

```html
<style>
    .content {
        width: 100%;
        height: 500px;
        border-collapse: collapse;
        color: white;
        vertical-align: middle;
        text-align: center;
    }
    .left {
        width: 200px;
        background-color: skyblue;
    }
    .center {
        background-color: mediumturquoise;
    }
    .right {
        width: 150px;
        background-color: lightsteelblue;
    }
</style>
<table class="content">
    <tbody>
        <tr>
            <td class="left">left</td>
            <td class="center">center</td>
            <td class="right">right</td>
        </tr>
    </tbody>
</table>
```

- 网格布局
  - 设置网格列的宽度`grid-template-columns: 200px auto 150px`或 简写`grid: auto-flow / 200px auto 150px`
```html
<style>
.content {
  display: grid;
  grid-template-columns: 200px auto 150px;
  height: 500px;
  line-height: 500px;
  text-align: center;
  color: #fff;
}
.left {
  background-color: skyblue;
}
.center {
  background-color: mediumturquoise;
}
.right {
  background-color: lightsteelblue;
}
</style>
<div>
  <div class="content">
  <div class="left">left</div>
  <div class="center">center</div>
  <div class="right">right</div>
</div>
```



# 动画

### 对比过渡和动画

过渡`transition`：元素在两个状态间切换时的过渡效果

动画`animation`：通过`@keyframe`设定动画关键帧，可以同时设定多组动画

根据定义，`transition`可以看最作`animation`中，一组`@keyframe`包含`from`（`0%`）和`to`（`100%`）两个关键帧（状态）的特殊情况

区别如下：

```
transitioin
```

- 事件驱动：需要访客或`JS`使状态发生变化后触发
- 一次性：想要循环，需要反复修改状态
- 定义两个状态：开始状态和结束状态
- 可指定唯一过渡属性：`transition-property:all`默认两个状态间所有可过渡属性，都会有补间动画。可以通过`transition-property`指定可以唯一过渡属性

```
animation
```

- 自动执行或事件驱动
- 循环或指定执行次数
- 定义多个状态
- 不可指定唯一过渡属性
- 可控制：暂停，播放等

### 如何优化 CSS 动画的性能？

一方面，减少不必要动画

- 设备：尽可能适应低配置设备
- 耗电量：减少耗电量
- 引发不适：避免闪烁、变化强烈、3D 眩晕动画
- 用户原因（喜好、疾病、工作等）已配置减少动画

综上，通过 CSS 媒体查询，在以下场景减少动画，始终避免引起不适的动画

- 移动设备
- `refers-reduced-motion`设置值为`reduce`
- 主体内容不应只有纯动画，适当的文字说明，利于读屏软件和 SEO

另一方面，提升动画体验

- 减少不必要动画元素和属性，避免使用`transition:all`
- 减少重排、重绘
  不同属性值引起改变，重新渲染有三种执行路径，尽量只使用合成属性

| **属性类型** | **执行路径**               | **属性举例**                                                 |
| :----------- | :------------------------- | :----------------------------------------------------------- |
| 重排属性     | layout > paint > composite | 盒模型：display、padding、margin、width、height、min-height、border、border-width<br>定位及浮动：position、top、bottom、left、right、float、clear<br>文字及溢出：font-family、font-size、font-weight、line-height、text-align、vertical-align、white-space、overflow、overflow-y |
| 重绘属性     | paint > composite          | color、border-style、border-radius、visibility、text-decoration、background、background-image、background-position、background-repeat、background-size、outline、outline-color、outline-style、outline-width、box-shadow |
| 合成属性     | composite                  | transform、opacity、backface-visibility、perspective、perspective-origin |

- 避免需要大量计算的属性：`box-shadow`、`filter`
- 开启硬件加速：`transform: translate3d(0, 0, 0)`、`transform: translateZ(0)`、`will-change:transform`可开启，内存占用可能增加
- 创建层叠上下文，动画元素脱离文档流，独立图层，例如：
  - `position`为`absolute`或`fixed`
  - `contain`包含`layout`或`paint`

Chrome 浏览器提供了 FPS 帧率显示工具，方便实时测量动画性能

![image.png](https://pic.leetcode-cn.com/1617073604-FSUbiL-image.png)

### 对比 CSS 动画和 JS 动画？

JS 动画：

- IE5.5 - 9支持
- 报错时，影响或被其他脚本影响。定时器不准确
- 复杂逻辑（条件判断、递归）和交互（监听事件）的动画
- JS在主线程的解析的环境中，阻塞或被阻塞其他操作。早期运行效率低。V8和现代架构浏览器已经改善

CSS 动画：

- `transition`、`animation`IE10+ 支持，早期私有属性多，不同环境效果略不同
- 不支持的环境，直接跳过
- 补间动画支持，设置关键帧，更容易。正在完善的事件和更细粒度`Animation()`支持
- 可通过合成属性，如`transform`在 compositor 线程实现动画

JS 动画依赖 CSS 的样式

- 通过 CSS 开启硬件加速
- 都应减少重排和重绘

CSS 动画的更细粒度控制

- 依赖 JS

综上：

- 原来用`JS`的定时器，绘制每一帧，监听悬浮、获取焦点事件实现的动画和交互，都可以通过纯`CSS`动画配合伪类实现
- 未来`JS`更专注于复杂逻辑动画或对CSS的动画更细粒度的交互

### 纯 CSS 实现打字机效果

CSS 动画默认带过渡效果，浏览器会自动完成两个状态（关键帧）的补间动画，平滑过渡

`step(num, start / end)`属性，可以从两个状态的补间动画中，平均取`num`个帧（过渡状态），让动画在这些帧间直接跳转

`step-start`是`setp(10, start)`的简写，动画从开始状态起

`step-end`是`step(10, end)`的简写，动画从结束状态起

实现打字机效果，思路：

给一个DIV添加动画，宽`width`从`0`到文字总长度变化

有多少字，就从补间动画平均取多少个过渡状态，每状态对应显示一个字

光标闪烁，只有显示和隐藏两个状态，期间不需要过渡效果，并且无限循环

```html
 <style>
div {
    border-right: 1px solid #ccc; /* 光标 */
    width: 5em; /* 5个字的宽度 */
    white-space: nowrap; /* 不换行 */
    overflow: hidden; /* 溢出内容裁剪 */
    /** step(5, end)，直接跳到5个居中状态，省略过渡 step-end 直接跳到最终状态 **/
    animation: type 3s steps(5, end), cursor .5s step-end infinite alternate;
  }
  @keyframes type {
    0% {
      width: 0;
    }
  }
  @keyframes cursor {
    50% {
      border-color: transparent;
    }
  }
</style>
<div>打字机效果</div>

```

### 纯 CSS 实现暗黑、夜间模式

匹配暗黑模式：`prefers-color-scheme:dark`表示当前系统的主题色为暗色

- CSS 媒体查询

```html
@media (prefers-color-scheme: dark) {
/** 暗黑模式的CSS **/
}
```

- Link 媒体查询

  ```html
  <link rel="stylesheet" href="dark.css" media="(prefers-color-scheme: dark)" />
  ```

- 实现暗黑模式

  - 变量：自己写具体样式

    ```html
    @media (prefers-color-scheme: dark) {
    :root {
    --color: white; /** 为暗黑模式配置变量值 **/
    }
    }
    html {
    color: var(--color);
    }
    ```

  - 混合模式：`* {mix-blend-mode: difference}`影响图片和背景

  - 反转 + 滤镜：`html, img {filter: invert(1) hue-rotate(180deg);}`



# 原理

### 对比 CSS 加载的方式？

四种加载方式：

- ```
  style
  ```

  属性（内联样式）

  - 定义的属性优先级高于所有选择器和其它加载方式
  - 无法复用
  - 覆盖样式只能使用`!important`

- ```
  <style></style>
  ```

  标签（嵌入样式）

  - 作用域：只对当前页面有效
  - 单页应用，减少请求，提高速度
  - 支持媒体查询

- ```
  <link>
  ```

  （外链样式）

  - CSS 和 HTML 分离，便于复用和维护
  - 利于缓存
  - 支持媒体查询

- ```
  @import
  ```

  （导入样式）

  - 模块化
  - 利于缓存
  - 支持媒体查询
  - 需先下载并解析引用 CSS，才可以继续下载导入 CSS。现代通常使用模块化工具在编译时合并 CSS



### 加载 CSS 是否会阻塞页面渲染？

- CSS 不阻塞 HTML 转 DOM树

HTML是超文本标记语言，`<元素名>`作起始标签，`</元素名>`结束标签或唯一标签

浏览器将元素和文本作为节点，将父元素作为父节点，将 HTML 转为 DOM 树。这个过程与 CSS 无关

- CSS 阻塞 CSS 树和 Render 树

CSS 会被转为CSS树，并与 DOM 树结合成 Render 树等待渲染

如果 CSS 下载很慢，那么 CSS 解析会被阻塞，后续的渲染也无法进行

通过媒体查询，可以设置`link`引用的 CSS 不阻塞渲染，但仍会下载

- CSS 阻塞 JS 执行
  JS 可能需要查询、修改 DOM 树和 CSS 样式，本身阻塞`DOM 解析`
  如果 JS 不依赖样式，可以放在 CSS 前，避免被阻塞

所以，通常情况下：

1. `CSS`和不依赖 DOM 的`JS`放`</head>`标签前
2. 依赖 DOM 的`JS`放在`</body>`标签前

### 浏览器是如何解析和渲染 CSS 的？

浏览器解析和渲染 CSS 的步骤：

- 解析

将 CSS 字符串转换为包括选择器、属性名、属性值的数据结构，长度单位被转换为像素，关键字被转换为具体值，需要计算的函数转为具体值

- 级联

相同元素相同属性的最终值基本由书写顺序，按先后决定，此外：

- !important > 其它
- style属性 > 其它
- 选择器优先级

> ID > 类 > 类型（标签） > 相邻 > 子代 > 后代 > 通配符 > 属性 > 伪类

- 开发者 > 用户配置 > 浏览器默认属性
- 层叠

根据`position`不为`static`等属性或弹性布局中的子元素等情况创建层叠上下文。根据`z-index`决定层的叠加顺序

- Render Tree

深度优先遍历之前解析 HTML 得到的 Dom Tree，匹配解析 CSS 得到的 CSSOM

计算元素的位置、宽高，将可见元素的内容和样式放入 Render Tree

- 布局

分层按照流式布局（块、内联、定位、浮动）、弹性布局、网格布局或表格布局等布置元素，按照尽可能多地展示内容的原则，处理溢出

- 绘制

分层绘制颜色、边框、背景、阴影

- 合成

将不同图层分格渲染出位图，可交由 GPU 线程处理

处理图层的透明度`opactiy`，和变形`transform`等

将所有图层合到一起

- 重新渲染

JS 更改 CSS 属性，CSS 动画以及伪类（如`hover`），内容变更等，可能会引起浏览器重新布局、绘制或者合成

### 对比获取 CSS 样式的接口

- ```
  style
  ```

  - 可读写
  - 属性名驼峰写法
  - 通过内联样式`style`读写属性

- ```
  currentStyle
  ```

  - 可读
  - 兼容连字符-写法
  - IE5.5 - IE8

- ```
  getComputedStyle
  ```

  - 可读
  - 兼容连字符-写法
  - IE9+
  - 来自`CSS Object Model`，计算后的属性
  - 支持伪类

- ```
  document.styleSheets
  ```

  - 可读
  - 获取规则列表
  - IE9+
  - 可写支持`insertRule``deleteRule`



### 什么是重排和重绘，更改哪些属性会触发重排和重绘，如何避免？

##### 什么是重排和重绘？

当DOM的样式或内容会被修改时，将触发重新渲染。除了属性值计算、单位换算外，渲染主要分为三个步骤：

- 布局：计算盒模型的位置，大小
- 绘制：填充盒模型的文字、颜色、图像、边框和阴影等可视效果
- 合并：所有图层绘制后，按层叠顺序合并为一个图层

重新渲染一般有三种执行路径：

- 重排：布局 → 绘制 → 合并
- 重绘：绘制 → 合并
- 合并
  不同属性的修改，会触发不同的渲染路径

##### 更改哪些属性会触发重排和重绘？

引起重排的属性，即布局类属性，包括：

| **类型**   | **属性名**                                                   |
| :--------- | :----------------------------------------------------------- |
| 盒模型     | displaypaddingmarginwidthheightmin-heightmax-heightborderborder-width |
| 定位和浮动 | positiontopbottomleftrightfloatclear                         |
| 文字及溢出 | font-familyfont-sizefont-weightline-heighttext-alignvertical-alignwhite-spaceoverflowoverflow-y |

引起重绘的属性，即绘制类属性，包括：

| **类型** | **属性名**                                                   |
| :------- | :----------------------------------------------------------- |
| 颜色     | color                                                        |
| 边框     | border-colorborder-styleborder-radius                        |
| 背景     | backgroundbackground-imagebackground-positionbackground-repeatbackground-size |
| 轮廓     | outlineoutline-coloroutline-styleoutline-width               |
| 可见性   | visibility                                                   |
| 文字方向 | text-decoration                                              |
| 发光     | box-shadow                                                   |

##### 如何避免重排和重绘？

- 尽量使用仅引起合成的属性

| **类型** | **属性名** |
| :------- | :--------- |
| 变形     | transform  |
| 透明度   | opacity    |

- 限制重新渲染区域

  - 使用`position:absolute`或`position:fixed`等方法创建层叠上下文
  - 使用`contain:layout`或`contain:paint`等属性值，让当前元素和内容独立于 DOM 树

- 减少使用`display:table`或`<table>`表格布局

- 利用浏览器自身优化

  引起回流、重绘的属性操作会放入队列，达到一定数量或时间，再一次渲染

  - 用变量缓存元素的属性值
  - 要设置的属性值减少依赖其它属性值
  - 避免频繁读取计算属性值

- 手动一次渲染
  强制使用`style.cssText`或`setAttribute('style', 样式)`将所有设置的属性，一次写入内联样式

- 优化 DOM 树

  - 使用文档碎片或`display:none`隐藏节点，缓存要插入的节点，之后将缓存结果一次性插入 DOM 树并显示
  - 使用`replaceChild``cloneNode`减少先删除、创建再插入 DOM 的场景



# 设计

### 对比多方法实现图标

| **实现方式** | **特殊符号**               | **PNG / GIF**             | **Sprites**                 | **Icon Font**                 | **SVG**      |
| :----------- | :------------------------- | :------------------------ | :-------------------------- | :---------------------------- | :----------- |
| 大小         | font-size                  | width height              | background-size             | font-size                     | width height |
| 无锯齿       | 受 font-family 影响        | 放大可能有锯齿            | 放大可能有锯齿              | Windows 字号较小时 可能有锯齿 | √            |
| 颜色         | 单色 用 color 设置         | 图片本身颜色 可用滤镜修改 | 图片本身颜色 可用滤镜修改   | 单色 用 color 设置            | 彩色 可修改  |
| 兼容性       | 不同浏览器显示效果可能不同 | IE6 不支持 PNG透明        | 改大小IE9+ SVG Sprites IE9+ |                               | IE9+         |
| 响应式       | -                          | √                         | √                           | -                             | √            |
| 透明         | √                          | √                         | √                           | √                             | √            |
| 场景         | 简单图标 + emoji           | 常用                      | HTTP1.1时合并请求           | 自定义字体                    | 可定制图标   |

### 多方法实现小三角

- 伪类 + 特殊符号

  ![image.png](https://pic.leetcode-cn.com/1617074535-yQfTrH-image.png)

  ```html
  <style>
  div {
  float: left;
  margin-right: 10px;
  }
  .top::before {
  content: '▲'
  }
  .right::before {
  content: '▶';
  }
  .bottom::before {
  content: '▼';
  }
  .left::before {
  content: '◀';
  }
  </style>
   
  <div class="top"></div>
  <div class="right"></div>
  <div class="bottom"></div>
  <div class="left"></div>
  ```

- 特殊符号 + 缩进 + 溢出 + 旋转

  ![image.png](https://pic.leetcode-cn.com/1617074515-LkFLwb-image.png)

  ```html
  <style>
  div {
  float: left;
  margin-right: 6px;
  font-size: 20px;
  font-family: '宋体';
  overflow: hidden;
  }
  .top {
  text-indent: -10px;
  transform: rotate(-90deg);
  }
  .right {
  text-indent: -10px;
  }
  .bottom {
  width: 10px;
  transform: rotate(-90deg);
  }
  .left {
  width: 10px;
  }
  </style>
   
  <div class="top">◆</div>
  <div class="right">◆</div>
  <div class="bottom">◆</div>
  <div class="left">◆</div>
  ```

- 宽高为 0 的边框

  ![image.png](https://pic.leetcode-cn.com/1617074495-KQxaOk-image.png)

  ```html
  <style>
  div {
  float: left;
  margin-right: 6px;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  }
  .top {
  border-top-color: black;
  }
  .right {
  border-right-color: black;
  }
  .bottom {
  border-bottom-color: black;
  }
  .left {
  border-left-color: black;
  }
  </style>
   
  <div class="top"></div>
  <div class="right"></div>
  <div class="bottom"></div>
  <div class="left"></div>
  ```



### 多方法隐藏元素

| **属性**                               | **不占位** | **读屏软件隐藏** | **用于隐藏主体内容 SEO**     |
| :------------------------------------- | :--------- | :--------------- | :--------------------------- |
| display: none                          | √          | √                | 不抓取                       |
| visibility: hidden                     | ×          | √                | 可能抓取                     |
| opacity: 0                             | ×          | ×                | 疑似作弊                     |
| input type = hidden                    | √          | √                | 不抓取                       |
| position:absolute / fixed              | √          | ×                | 疑似作弊                     |
| aria-hidden = true                     | ×          | √                | 未知                         |
| text-indent < 0                        | √          | ×                | 常用于在 Logo 处标识网站名称 |
| font-size: 0                           | √          | ×                | 疑似作弊                     |
| overflow: hidden                       | √（裁剪）  | ×                | 抓取                         |
| clip-path: polygon(0 0, 0 0, 0 0, 0 0) | √          | ×                | 未知                         |

### 对比常见图片格式和 base64 图片？

| **拓展名** | **MIME**      | **透明** | **动画** | **IE兼容性**  | **特点**                                                     |
| :--------- | :------------ | :------- | :------- | :------------ | :----------------------------------------------------------- |
| .ico       | image/x-icon  | √        | ×        | IE            | 早期浏览器只支持 favicon.ico                                 |
| .bmp       | image/bmp     | ×        | ×        | IE            | windows 系统原生支持，图片信息丰富                           |
| .jpg       | image/jpeg    | ×        | ×        | IE            | 可控制品质，模糊到清晰或渐进加载，常见于照片                 |
| .png       | image/png     | √        | ×        | IE6（不透明） | 可控制品质、位数，常见于绘画                                 |
| .gif       | image/gif     | √        | √        | IE            | 基于颜色透明，有毛边，256颜色上限，适合动画                  |
| .svg       | image/svg+xml | √        | ×        | IE9+          | 无损放大，可调色、修改，更常见于图标                         |
| .apng      | image/apng    | √        | √        | Edge          | Mozilla主导，兼容PNG浏览器即可显示第一帧，压缩率较高，无毛边 |
| .webp      | image/webp    | √        | √        | Edge          | Coogle主导，压缩率更高，无毛边，常见于微信公众号             |
| .avif      | image/avif    | √        | √        | ×             | Netflix主导，压缩率更高，无毛边，同压缩比效果有时超过 webp   |

可以将图片转为 Base64 编码，直接将编码放入 CSS 中，即可引入图片

编码后的图片通常比原图大 30% 或更多，但可以与 CSS 一起被 gzip 或 br 压缩

适用小图片和没有权限上传图片的场景，来减少请求，但也应设置代码编辑器不换行或折叠图片编码区域，避免影响 CSS可读性



# 工程化

### 对比 Less、Sass、Stylus、PostCSS？

Less、Sass 和 Stylus 是 CSS 预处理器，PostCSS 是转换 CSS 工具的平台

- ##### Less

  - 变量：`$`标识符变量，使用`{}`插值
  - 嵌套：支持`{` `}`大括号嵌套
  - 混入器：支持 选择器 混入 或 使用`.selector(@param)`创建纯混入器
  - 扩展 / 继承 / 运算符 / @import：支持
  - 流程：支持`if`条件判断，支持`when`递归模拟循环
  - 映射：支持`@`声明和使用 Map
  - 特有：提供 Less.js，直接在浏览器使用 Less

- ##### SaSS

  - 变量：支持`$`标识符变量，使用`#{}`插值
  - 嵌套：SCSS 支持`{` `}`大括号嵌套 SASS 支持缩进嵌套（SCSS 是 Sass 3 引入新的语法）
  - 混入器：`@mixin`创建`@include`使用
  - 扩展 / 继承 / 运算符 / @import：支持
  - 流程：支持`if` `else`条件判断，支持`for` `while` `each`循环
  - 映射：支持`$()`声明 Map，提供`map-get(map, key)` `map-keys(map)` `map-values(map)`等一系列方法操作 Map，支持遍历 Map
  - 特有：支持 compass ，内含 自动私有前缀 等一系列有用SASS模块，支持压缩输出

- ##### Stylus

  - 变量：支持`$`标识符变量 和 赋值表达式变量，使用`{}`插值
  - 嵌套：支持`{` `}`大括号嵌套 和 缩进嵌套
  - 混入器：像函数一样创建和使用
  - 扩展 / 继承 / 运算符 / @import：支持
  - 流程：支持`if` `else` `unless`三元 条件判断，支持`for`循环
  - 映射：像 JS 一样创建和使用对象
  - 特有：支持中间件，自动分配函数变量，提供 JS API。支持压缩输出

- PostCSS

  - 接受 CSS 文件，把 CSS 规则转换为抽象语法树
  - 提供 API 供插件调用，对 CSS 处理
  - 插件：支持 Autoprefixer 自动添加私有前缀、css-modules CSS 模块 stylelint CSS 语法检查等插件，PostCSSS 是工具的工具

### Webpack 处理 SASS 文件时，sass-loader, css-loader，style

##### 作用

- sass-loader
  - 将 SASS / SCSS 文件编译成 CSS
  - 调用`node-sass`，支持`options`选项向`node-sass`传参
- css-loader
  - 支持读取 CSS 文件，在 JS 中将 CSS 作为模块导入
  - 支持 CSS 模块 @规则`@import` `@import url()`
- style-loader
  - 将 CSS 以`<style>`标签的方式，插入 DOM

##### 配置顺序

Webpack中 loader 的加载顺序是从后往前

在处理 SASS / SCSS 文件时，三者的配置顺序 style-loader css-loader sass-loader

可以用插件 ExtractTextWebpackPlugin 替换 style-loader，生成新的 CSS 文件



### 如何压缩 CSS 大小，如何去除无用的 CSS？

压缩 CSS ，简单可分为 3 步骤：

① 去除 CSS 中的换行和空格

② 去除 每个选择器，最后一个属性值后的分号`;`

② 去除 注释，正则`/*[^*] [^/][^*]**+)*/`

此外，包括使用 缩写属性，`z-index`值的重新排序，也可以减少代码量。但通过工具进行时，结果不一定总是安全

常用的 CSS 压缩工具有：

- YUI Compressor
  基于 Java , 本来是 JS 压缩，也兼容 CSS 压缩，配置较少，但保障压缩安全
- clean-css
  基于 NodeJS , 可以根据 浏览器版本、具体的 CSS 标准模块，详细配置兼容性
- cssnano
  基于 PostCSS ，是 Webpack5 推荐的 CssMinimizerWebpackPlugin 默认的 CSS 压缩工具

如何去除无用的 CSS ：

- Chrome 开发者工具 Lighthouse
  打开 http / https 网址，勾选 Performance 选项，在性能报告中，会列出 unused CSS，可以人工去除
- UnCSS
  - 通过 jsdom 加载 HTML 并执行 JS
  - 通过 PostCSS 解析样式
  - 通过`document.querySelector`查找 HTML 中不存在的 CSS 选择器
  - 返回 剩余样式
- PurgeCSS
  - 可以对 HTML JS 和 VUE 等框架中 CSS 使用情况分析，并删除无用 CSS
  - 提供 Webpack Gulp Grunt 等工程化工具的插件
- cssnano
  - 提供`discardUnused`选项，用于删除与当前 CSS 无关的规则
  - 不支持内容分析
  - 同样支持工程化工具



### 什么是 CSS 模块化，有哪几种实现方式？

#### 什么是 CSS 模块化？

CSS 模块化是将 CSS 规则 拆分成相对独立的模块，便于开发者在项目中更有效率地组织 CSS：

- 管理层叠关系
- 避免命名冲突
- 提高复用度，减少冗余
- 便于维护或扩展

CSS 模块化的方式：

- 基于文件拆分
- 不拆分但设置作用域
- CSS in JS
- 内联样式、Shadow DOM 等

无论哪种方式，核心都是通过 保证CSS类命名唯一，或者 避免命名使用内联样式，来模拟出CSS模块作用域的效果

#### 基于文件的 CSS 模块的加载

- `<link>`
  将不同模块的 CSS 分文件存放，通过`<link>`标签按需引入
- `@import`
  @规则，将其它 CSS 嵌入当前 CSS
  除现代浏览器外，也得到了 CSS 预处理器 Less、Sass 和 Stylus 的支持
- `import`
  在 Webpack 中，将 CSS 作为资源引入，并可以通过 CSS Modules 为 CSS 生成独一无二的类名

#### CSS 模块化的实现方式

- 分层拆分

  将 CSS规则 分层存放，并约束不同层次间的命名规则

  - SMACSS：按功能分成 Base Layout Module State Theme 五层
  - ITCSS：按从通用到具体分成 Settings Tools Generic Base Objects Components 七层

- 分块拆分

  将页面中视觉可复用的块独立出来，仅使用类选择器，并确保类名唯一

  - OOCSS
    - 将盒模型规则与颜色、背景等主题规则拆分
    - 将视觉可复用的块类名与页面结构解耦
  - BEM
    - 将页面按 Block Element Modifier 划分
    - 类名规则 block-name__element-name--modifier-name

- 原子化拆分

  每个选择器只包含1个或少量属性，通过组合选择器定义复杂样式

  - ACSS：属性名像函数，属性值像函数参数，像写内联样式一样组合类名
  - Utility-First CSS：提供一套可配置的 CSS 实用类库，使用时按需编译

- CSS in JS

  - CSS Modules
    - 将 CSS 作为资源引入
    - 根据内容的哈希字符串，计算出独一无二的类名，CSS 规则 只对该类名下的元素生效
  - styled-components Aphrodite Emotion 等
    - 通过 JS 创建包含属性定义的组件
    - 生成独一无二的类名
  - Radium 等
    - 通过 JS 创建包含属性定义的组件
    - 生成内联样式

- Shadow DOM
  通过`attachShadow`给元素的影子DOM，附加`<style>`标签，其中规则不会影响外部元素。代表的框架有 Ionic 等



### 对比媒体查询与按需引入 CSS？

- 媒体查询
  - 允许我们通过设备、屏幕、使用场景、用户偏好来解析符合条件的 CSS，忽略不符合条件的 CSS
  - 即使通过 Link 标签附加媒体查询条件引入的 CSS，不符合条件时依然会被下载
- 按需引入
  按需引入，可以避免冗余下载的问题，允许我们按照目标环境引入CSS
  但条件没有媒体查询丰富，多数时，无法及时对运行环境的变化作出响应
- IE 浏览器中，可以通过 HTML 注释，附加 if 条件，阻止其中的 HTML 被解析，相应的 CSS 也不会被下载

```html
<!--[if lt IE 9]><link rel="stylesheet" href="ielt9.css" /><![endif]-->
```

- 工程化及ES6等模块化环境中，我们可以通过`import`或`require`只引入需要的`css`，通过`PurgeCSS`和`cssnano`去除未被使用的CSS
- 在`webpack`配置`DefinePlugin`自定义环境变量，在`uni-app`中 使用条件注释等利用打包工具提供的 条件编译 功能



# 方法论

### 列举 CSS3 新特性

##### 什么是 CSS3？

自 CSS2.1 后，CSS 标准被拆解成多个模块，每个模块有自己的版本并独立更新

CSS3 泛指这些模块的总和，作为 CSS 的第3版本的 CSS3 事实上已不存在

##### 什么是 CSS3 新特性？

CSS 标准的各个模块都在快速更新，其中已经进入到候选推荐、建议推荐和推荐 的模块被称为稳定模块

稳定模块中新增的特性大多已获得浏览器广泛支持，使用不需要加私有前缀

这里的新特性通常指这些模块中新增的标准

##### CSS3 新特性有哪些，举例说明？

- ##### 颜色模块

  - 新增`opacity`属性
  - 新增`hsl()` `hsla()` `rgb()` `rgba()`方法
  - 新增 颜色关键字 currentColor
  - 定义`transparent`为`rgb(0, 0, 0, 0.0)`

- 选择器模块

  - 新增 属性选择器：`[attribute^="value"]` `[attribute$="value"]` `[attribute*="value"]`
  - 新增 伪类：`:target` `:enabled` `:disabled` `:checked` `:indeterminate` `:root` `:nth-child` `:nth-last-child` `:nth-of-type` `:nth-last-of-type` `:last-child` `:first-of-type` `:last-of-type` `:only-child` `:only-of-type` `:empty` `:not`
  - 新增 普通兄弟选择器：`~`
  - 规范 伪元素表示为两个冒号：`::after` `::before` `::first-letter` `::first-line`

- 命名空间模块
  新增 @规则：`@namespace`

- 媒体查询模块

  - 支持更多媒体查询条件：`tv` `color` `resolution`等
  - 支持`<link>`标签的媒体查询

- 背景和边框模块

  - 支持渐变`linear-gradient`背景
  - 支持多背景图片
  - 新增`background-origin` `background-size` `background-clip`
  - 新增 圆角边框：`border-radius`
  - 新增 边框图片：`border-image`
  - 新增 边框阴影：`box-shadow`

- 多列布局模块

  - 支持多列布局：`columns` `column-count` `column-width`等

- 值和单位模块

  - 新增 想对长度单位：`rem` `ch` `vw` `vh` `vmax` `vmin`
  - 新增方法：`calc()`

- 弹性盒布局模块

  - 支持弹性布局：`dispaly:flex` `flex-direction` `flex-wrap`等

- 文本装饰模块

  - 新增 着重符号：`text-emphasis`
  - 新增 文本阴影：`text-shadow`

### 什么是 CSS 组件化和原子化？

##### 组件化

CSS 组件是样式页面样式可复用的最小元素组合
过去，前端常会按照页面结构，将公共部分提取成组件，方便在其它页面调用
好处：

- 无需重复编写 CSS，结构级别的复用
- 保持风格统一
- 一次修改，所有页面都生效
- 通过新建修饰符类的方式，个性化组件

缺陷：

- 限制设计师发挥，设计师可能需要先参考组件库已有组件的设计
- 需要个性化的属性较多时，修饰原有组件和新建组件，都可能产生新的代码冗余
- 组件库随项目越来越大，逐渐难以维护

##### 原子化

CSS 原子化的每个原子类，几乎只包含一种属性定义
这些原子类可以继续组合，生成属性更丰富的组件类
前端更关注不同元素间的相同属性，即使这些元素的结构完全不同

好处：

- 无需重复编写 CSS，属性级别的复用
- 保持风格统一
- 一次修改，所有页面都生效
- 更灵活，设计师可以发挥，前端可以自由组合
- 类名与业务松耦合，甚至可以为不同业务定制
- 组件的样式一目了然，容易维护

缺陷：

- 无统一标准，类名自己起，他人使用需要学习成本
- HTML 类名顺序，无法决定 CSS 优先级，HTML 在后面的类名，可能因为在 CSS 定义中在前，相同属性的值被 CSS 定义中后面的类名覆盖。违反心智模型
- 项目较小时，编写原子库不经济
- CSS-in-JS 改进原子化缺陷
  - 类名无统一标准，需要学习
    类名由 JS 根据内容生成，相同 CSS 的类名相同，不同 CSS 的类名不同且唯一，前端不用起类名
  - 使 HTML 类名顺序生效
    原子类只有一个属性，这意味着，我们把 CSS 类定义写进 JS，由JS根据 类名顺序，若属性名重复，则采用最后出现的属性值
  - 此外，CSS-in-JS 还将样式与组件放到一起，能够从根本上避免组件移除，而样式忘记移除的问题

原子化思想早已有之，Atomic CSS 和 tailwindcss 最有代表性

更多组件库并不是只用组件化或原子化，而是通过组件化提供易用的组件和有限的可定制接口，通过原子化提供响应式布局、边距、颜色、字体等细粒度控制类 在工作中，组件化和原子化都能满足绝大多数的业务场景，是否应用更多地受设计思想和开发习惯的影响

对于业务组件来说，使用公用原子类，影响组件的独立性。而仅在组件内部使用原子类，又会大大降低原子类的优势

开发业务类组件，使用组件化的 CSS 依然是首选，除非我们决定引入或定义一套原子类 CSS，作用于整个组件库

长期来看，原子化正越来越得到关注，结合 CSS-in-JS 的原子化未来可期



# 综合

### HTTP2.0 时代，CSS Sprites 还有用吗，为什么？

##### HTTP1.1 下的 CSS Sprites

HTTP1.1 自身的管道化并不完善。浏览器实际通过为同一域名下的资源同时建立多个 TCP 连接，来实现并行传输，并且限制了最大连接数

通过手动或者工程化的方式，将将小图片合并成大图，即 Sprites图（又称精灵图，雪碧图）。可以将多个对小图的请求，合并为对单张大图的请求，通过`background-position`定位在文档显示

优点：

- 避免大量小图请求阻塞其他资源下载
- 减少多连接的重复的从发起请求到首字节响应的时间
- 避免由于网络不稳定、客户端、服务端限制，导致部分小图丢失的情况
- 图标、界面一次显示全，提升用户体验

缺点：

- 修改、增加和删除图片、修改位置，颜色麻烦
- 请求图片的连接失败，整个界面图片效果丢失
- 宽度和高度通常固定，自适应需要额外设置
- 实现响应式图片时可能需要维护多张合成图，而且灵活度依然不够
- HTTP1.1 下的 SVG Sprites
- 合并 SVG 到一张 SVG

```
<svg>
  <symbol id="svg1"><!-- SVG① --></symbol>
  <symbol id="svg2"><!-- SVG② --></symbol>
  <symbol id="svg3"><!-- SVG③ --></symbol>
</svg>
```

- 在文档中通过`use`引用其中的一张 SVG

```
<svg>
  <use xlink:href="#svg1"></use>
</svg>
```

SVG 具有无锯齿放大，容易修改尺寸好颜色等优点

SVG Sprites 可以作为不考虑 IE8- 时，CSS Sprites替代

但开发者仍然需要 手工 或 工程化方式，合成 SVG Sprites

##### HTTP2.0 支持多路复用

HTTP2.0 下，一个 TCP 会话上可以进行任意数量的HTTP请求

理论上，所有图片资源几乎可以并行下载

CSS Sprites 最初要解决的问题，已经不复存在

基于 TCP 本身的局限性（丢包时，整个会话都要重传），考虑网络的不稳定

实际体验中，CSS Sprites 对比 HTTP2.0，通常仍会有轻微的速度提升

是否使用 CSS Sprites，或者升级到 SVG Sprites 可以结合维护复杂度代入具体项目决定



### 你在开发中都遇到过哪些 CSS 兼容性问题，你是如何解决的？

#### IE 兼容性问题

PC 时代，Trident 内核的 IE 在 5.5 - 8 时占据主导地位

- 问题：IE6 PNG 图片不透明

  - 答案：使用 `progid:DXImageTransform.Microsoft.AlphaImageLoader` 滤镜重复加载 PNG

- 问题：IE6 浮动元素设置 `margin`，边距变 2 倍

  - 答案：设置浮动元素`display: inline`

- 问题：IE6 不支持绝对定位 `position: fixed`

  - 答案：

    ```
    position: absolute;
    top: expression(((t = document.documentElement.scrollTop) ? t : document.body.scrollTop) + 'px'); 
    ```

- 问题：IE8 有链接的图片四周由黑边框

  - 答案：`img { border: none }`

#### 测试

- IEtester 比 IE9+ 自带 Simulater 更接近真实效果

- 使用装有 低版本IE 的虚拟机镜像，模拟完全真实的低版本 IE 环境

  - 微软甚至专门提供了一套90天激活的 win7 镜像

- 非标与标准浏览器兼容性问题
  标准属性不被支持，或者实现不同

- 问题：IE兼容模式 与 标准浏览器 宽度和高度表现不同

  - 答案：IE 兼容模式默认 `box-sizing: border-box` 而不是 `box-sizing: content-box`，可以统一声明为 前者，或者用 `CSS hacks` 设置值对 IE 兼容模式生效的宽高或边距

- 问题：IE9- 不支持 opacity

  - 答案：

    ```
    opacity: .8;
    filter: alpha(opacity = 80);
    filter: progid:DXImageTransform.Microsoft.Alpha(style = 0, opacity = 80);
    ```

- 问题：IE9- 不支持 background-size 等 CSS3 新特性

  - 答案：IE6+ 支持`behavior`，可以设置或检索对象的 DHTML 行为。引入 css3pie 其用这个属性让 IE 支持部分 CSS3 新特性

- 问题：`cursor: hand`在标准浏览器无效

  - 答案：用 IE 也支持的标准属性值`cursor:pointer`替代

##### CSS3 新特性兼容性问题

CSS3 起，标准被模块化，每个模块相对独立。浏览器可能会将处于 **非推荐** 的阶段属性实验性实现，或者某个处于 **候选推荐** 阶段的标准又被否决。导致不同浏览器，以及同一浏览器的不同版本，对于新特性的实现差异。
解决：

- 人工或使用混入器添加私有前缀
  - 工作量大，开发阶段冗余代码多
  - 同一属性，有无私有前缀，后面接收的属性值类型、顺序等可能不同
  - 部分私有前缀，需要加到属性值，而不是属性名上
- PostCSS 的 autoprefixer 可以配置 Browserslist，按需对浏览器及其不同版本添加私有前缀

##### 移动端兼容问题

主要是 iOS 和 Android 下移动浏览器兼容以及使用 Webview 开发的 APP 兼容问题
注意：以下这些问题只会在特定版本的系统下出现

- 问题：溢出含滚动条元素怒在 iOS 滑动体验卡顿
  - 答案：给溢出滚动元素设置 `-webkit-overflow-scrolling:touch`
- 问题：iOS 禁用按钮后，按钮样式异常
  - 答案：给按钮设置 `opacity: 1`
- 问题：iOS 监听`<span>` `<label>`点击无响应
  - 答案：给元素怒设置 `cursor:pointer`
- 问题：Android 输入框显示语音输入按钮
  - 答案：隐藏语音输入按钮 `input::-webkit-input-speech-button { display: none }`

想要让 APP 用户获得体验更一致，减少兼容性问题：

- 将渲染引擎与源码一起打包，比如使用腾讯基于 X5 内核的浏览服务，安装包会变大
- 采用 React Native Flutter UniApp + Weex 等使用 HTML + CSS + JS 开发，原生渲染的方案



### 当你忘记一个 CSS 属性时，你一般如何做？

- Emmet
  Emmet 能够提高 HTML 和 CSS 的书写效率，我们只需键入缩写，代码提示就会帮助我们找到相应属性名或属性值
  IDE 如 VsCode 集成了 Emmemt，文本编辑器 Sublime Notepad++ VIM 等都有相应插件可以安装
  ![image.png](https://pic.leetcode-cn.com/1617076491-DyVDDV-image.png)
- 查看说明
  IDE 如 VsCode 支持鼠标悬停属性名 或 属性值，查看说明
  点击其中的链接，可以直达 MDN 对应文档
  ![image.png](https://pic.leetcode-cn.com/1617076471-XRSmyr-image.png)
- 搜索
  MDN，里面有属性的用法，在浏览器实现的说明及对应规范的链接
  Can I use，里面有属性在各个运行环境的兼容性情况，移动端版本数据想丢丰富
- 记忆
  重新手打一遍遗忘的属性和属性名，注释搜索中发现的信息或链接，在后续重构中思考最佳实践，这样能收获更多



### 结合项目谈谈你是如何学习 CSS 的？

- 发现问题

  思考你在项目中遇到过哪些问题，你是否如何解决的

  搜索，查文档、查规范是比请教他人更快速的问题解决途径

  - Bad：同一问题，同一属性，每次都要搜索
  - Good：高频的属性用法，尽量记住
  - Bad：反复复制粘贴代码
  - Good：找出代码中真正解决问题的部分，自己敲一遍
  - Great：明白问题的原因和解决问题的思路

- 提升效率、规范

  思考项目有哪些可以改进的地方，你是如何改进的，成果怎样

  - 效率：是否可以使用工具、脚本，将原有 CSS及相关的图片编译、压缩、上传、部署 等流程化、自动化
  - 规范：是否可以改进 CSS 文件、规则的组织形式，是否可以参考或者根据业务，拟定一套利于团队协作的开发规范，并通过工程化的方式推广规范（比如，在Commit前，对CSS代码进行规范性校验和自动更正）

- 技术分享

  - 回顾一次你做的技术分享，你通过哪些渠道获取了权威的信息，是否手写代码验证了这些信息，是否有通过 脑图、动画、PPT 等提高信息分享的效率 - 回顾一次你与同事或同学的争论，你是如何证明自己观点是正确的，是拿出了权威的资料，请权威的人帮你说服，还是通过实际测试后，用数据说话

- 学无止境

  - CSS 最近有哪些新鲜事，可以新的草案，新的工具，新的方法论等，简单分享你的观点
  - 项目中所用的技术栈，最近更新的内容是什么，比过去改进了什么，解决了什么问题，怎样解决的？

你可以根据上面的提纲自由发挥，每个问题举 1 个例子即可，避免长篇大论，不要谈太多感受，重点体现方法，结果，涉及的知识点到为止















































































































































































































































































