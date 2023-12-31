---
title: 搞定跨域资源共享CORS（笔记整理）
tags: HTTP
date: 2021-09-23 11:24:47
categories: [技术]
---

> 当从一个域向另一个不同的域或者端口请求资源的时候，会产生跨域请求。
![跨域.png](http://zwboy.oss-cn-beijing.aliyuncs.com/blog/kuayu.png)

### 一、什么是跨域

 1. 同源政策（same origin policy）：协议，主机名和端口号要相同，否则就会产生跨域请求。
 2. 通常的错误形式：Access to XMLHttpRequest at XXX has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
 3. 整个CORS通信过程，都是浏览器自动完成，不需要用户参与。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。
 4. 出于安全原因，浏览器限制从脚本内发起的跨源HTTP请求（或者跨域响应被拦截）。比如XMLHttpRequest和Fetch API都需要遵循同源策略。
 5. 实现CORS必须要在服务器端实现CORS接口，即响应报文包含了正确CORS响应头。

<!--more-->

### 二、为何要限制跨域
> 跨站请求攻击CSRF
![CSRF.png](http://zwboy.oss-cn-beijing.aliyuncs.com/blog/csrf.png)

1. 攻击者引导用户去登录访问正常的网站，比如银行，session_id等认证会留在浏览器中。
2. 同时用户还在访问攻击者提供的网站，但是该网站会自动或者通过点击触发向之前正常的网站发起请求，当然用了之前已经认证过的session_id。
3. 这样就在用户不知道的情况下进行了一些诸如发送邮件，甚至转账、购买商品等操作。
4. 其利用的是Web用户身份验证的漏洞，即，简单的身份验证智能保证请求是来自该用户的浏览器（不是其他人在另一个浏览器伪造），但是并不能保证请求是用户本身自愿发出的。
5. 同源政策将会确保网站a拒绝来自网站b的请求。

### 三、为何又需要跨域呢
> 前后端分离

当前端框架兴起之后，前后端彻底分离的开发方式渐渐流行。前端和后端往往部署在不同的域名之上。前端通过访问后端的API获取数据，渲染前端界面，甚至进行路由跳转。这通常意味着前后端会出现不同源的问题。因为即使部署在同一台主机上，二者也属于不同的端口。那么我们就需要某种策略使得跨域请求能够通过。

### 四、跨域请求过程
> 浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。
##### 1. 简单请求的跨域过程
- 1.1. 简单请求是什么

> 1. 请求方法是以下三种方法之一（简单方法）：HEAD，GET，POST
> 2. HTTP的头信息不超出以下几种字段（简单头）：Accept，Accept-Language，Content-Language，Last-Event-ID，Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain

- 1.2. 跨域过程

> 1. 浏览器检测到这是一个简单请求，会自动在Header中添加Origin字段，该字段描述了协议，域名以及端口的信息。
> 2. 服务器会检查Origin，如果不在许可范围内（allow-origin），就会返回一个正常的HTTP回应，浏览器会抛出异常。如果在，服务器会返回响应，响应Header中会怎加几个头信息字段。
> ```js
Access-Control-Allow-Origin:* //服务器接收的origin
Access-Control-Allow-Credentials: true // 服务器是否允许CORS请求中发送Cookie。
//如果想实现发送Cookie的话，还需要在浏览器端设置withCredentials = true;
Access-Control-Expose-Headers: // ResponsHeader中暴露的字段
```

##### 2. 非简单请求的跨域过程
> 比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json，含有自定义的请求头等。

- Step.1. 预检请求Preflight

> 旨在确保服务器对 CORS 标准知情，以保护不支持 CORS 的旧服务器
> 1. 浏览器发现是非简单请求，会在正式的通信之前，先向服务器询问当前网页所在的域名是否在服务器的许可名单中，以及可以使用哪些HTTP动词。
> 2. 预检请求是一个OPTION请求，会携带Access-Control-Request-Method（指明该跨域请求的Method），Access-Control-Request-Headers（该跨域请求额外会发送的头信息字段，比如认证字段）以及Origin信息。
> 3. 服务器检查以上三个字段，确认允许后就会做出回应。（服务器在设置跨域请求的时候不能忘记对预检请求的处理）。回应Header包括Access-Control-Allow-Methods，Access-Control-Allow-Headers，Access-Control-Allow-Credentials等。


- Step.2. 预检成功后再发起跨域请求

> 预检通过之后，浏览器会像简单请求一样，再发送正式的CORS请求。
![d](http://zwboy.oss-cn-beijing.aliyuncs.com/blog/cros-yujian.png)


### 五、服务器配置CORS
以nodejs koa为例，如下为koa corszho
```
const URL = require('url');
/**
 * 关键点：
 * 1、如果需要支持 cookies,
 *    Access-Control-Allow-Origin 不能设置为 *,
 *    并且 Access-Control-Allow-Credentials 需要设置为 true
 *    (注意前端请求需要设置 withCredentials = true)
 * 2、当 method = OPTIONS 时, 属于预检(复杂请求), 当为预检时, 可以直接返回空响应体, 对应的 http 状态码为 204
 * 3、通过 Access-Control-Max-Age 可以设置预检结果的缓存, 单位(秒)
 * 4、通过 Access-Control-Allow-Headers 设置需要支持的跨域请求头
 * 5、通过 Access-Control-Allow-Methods 设置需要支持的跨域请求方法
 */
module.exports = async function (ctx, next) {
    const origin = URL.parse(ctx.get('origin') || ctx.get('referer') || '');
    if (origin.protocol && origin.host) {
        ctx.set('Access-Control-Allow-Origin', `${origin.protocol}//${origin.host}`);
        ctx.set('Access-Control-Allow-Methods', 'POST, GET, OPTIONS, DELETE, PUT');
        ctx.set('Access-Control-Allow-Headers', 'X-Requested-With, User-Agent, Referer, Content-Type, Cache-Control,accesstoken');
        ctx.set('Access-Control-Max-Age', '86400');
        ctx.set('Access-Control-Allow-Credentials', 'true');
    }
    if (ctx.method !== 'OPTIONS') {
        // 如果请求类型为非预检请求，则进入下一个中间件（包括路由中间件等）
        await next();
    } else {
        // 当为预检时，直接返回204,代表空响应体
        ctx.body = '';
        ctx.status = 204;
    }
};

```

### 总结
在开发前端的过程中CROS问题总是让自己很头痛，前期花费了大量时间解决CROS问题。再了解了CROS的原理和过程后，对跨域问题不再畏惧。

### Reference
1. [https://segmentfault.com/a/1190000015017666](https://segmentfault.com/a/1190000015017666)
2. [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
3. [HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)


