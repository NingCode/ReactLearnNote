# HTTP请求头详解

1. Accept-Charset 请求头用来告知（服务器）客户端可以处理的字符集类型。 借助内容协商机制，服务器可以从诸多备选项中选择一项进行应用， 并使用Content-Type 应答头通知客户端它的选择。浏览器通常不会设置此项值，因为每种内容类型的默认值通常都是正确的，但是发送它会更有利于识别。
```
Accept-Charset: utf-8, iso-8859-1;q=0.5
```

2. Accept-Encoding 会将客户端能够理解的内容编码方式——通常是某种压缩算法——进行通知（给服务端）。通过内容协商的方式，服务端会选择一个客户端提议的方式，使用并在响应头 Content-Encoding 中通知客户端该选择。
```
Accept-Encoding: gzip

Accept-Encoding: gzip, compress, br

Accept-Encoding: br;q=1.0, gzip;q=0.8, *;q=0.1
```
3. Accept-Language 请求头允许客户端声明它可以理解的自然语言，以及优先选择的区域方言。借助内容协商机制，服务器可以从诸多备选项中选择一项进行应用， 并使用 Content-Language 应答头通知客户端它的选择。
```
Accept-Language: de

Accept-Language: de-CH

Accept-Language: en-US,en;q=0.5

Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
```
4. Accept-Patch 服务器使用 HTTP 响应头 Accept-Patch 通知浏览器请求的媒体类型(media-type)可以被服务器理解
```
Accept-Patch: application/example, text/example
Accept-Patch: text/example;charset=utf-8
Accept-Patch: application/merge-patch+json
```
5. The Accept-Post response HTTP header advertises which media types are accepted by the server for HTTP post requests.
    翻译：Accept-Post 请求头允许客户端声明它可以理解哪些媒体类型。
```
Accept-Post: application/example, text/example
Accept-Post: image/webp
Accept-Post: */*
```
6. Accept-Ranges 响应头表示是否可以将对请求的响应暴露给页面。返回true则可以，其他值均不可以。
```
Access-Control-Allow-Credentials: true
```
    
7. 响应首部 Access-Control-Allow-Headers 用于 preflight request （预检请求）中，列出了将会在正式请求的 Access-Control-Request-Headers 字段中出现的首部信息。
```
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Allow-Headers: X-Custom-Header, Upgrade-Insecure-Requests
```
8. Access-Control-Allow-Methods 在对 preflight request.（预检请求）的应答中明确了客户端所要访问的资源允许使用的方法或方法列表。
```
Access-Control-Allow-Methods: POST, GET, OPTIONS
```
9. Access-Control-Allow-Origin 响应头指定了该响应的资源是否被允许与给定的origin共享。
```
如需允许所有资源都可以访问您的资源，您可以如此设置：

Access-Control-Allow-Origin: *
如需允许https://developer.mozilla.org访问您的资源，您可以设置：

Access-Control-Allow-Origin: https://developer.mozilla.org
CORS和缓存
如果服务器未使用“*”，而是指定了一个域，那么为了向客户端表明服务器的返回会根据Origin请求头而有所不同，必须在Vary响应头中包含Origin。

Access-Control-Allow-Origin: https://developer.mozilla.org
Vary: Origin
```
10. Access-Control-Expose-Headers 列出了哪些首部可以作为响应的一部分暴露给外部。
```
Access-Control-Expose-Headers: Content-Length, X-Kuma-Revision
```
11. Access-Control-Max-Age 这个响应头表示 preflight request  （预检请求）的返回结果（即 Access-Control-Allow-Methods 和Access-Control-Allow-Headers 提供的信息） 可以被缓存多久。
```
在 Firefox 中，上限是24小时 （即 86400 秒）。
在 Chromium v76 之前， 上限是 10 分钟（即 600 秒)。
从 Chromium v76 开始，上限是 2 小时（即 7200 秒)。
Chromium 同时规定了一个默认值 5 秒。
如果值为 -1，表示禁用缓存，则每次请求前都需要使用 OPTIONS 预检请求。
将预检请求的结果缓存10分钟：

Access-Control-Max-Age: 600 
```
12. Access-Control-Request-Headers 出现于 preflight request （预检请求）中，用于通知服务器在真正的请求中会采用哪些请求头。
```
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
```
13. Access-Control-Request-Method 出现于 preflight request （预检请求）中，用于通知服务器在真正的请求中会采用哪种  HTTP 方法。因为预检请求所使用的方法总是 OPTIONS ，与实际请求所使用的方法不一样，所以这个请求头是必要的。