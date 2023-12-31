---
title: 一次基于OAuth 2.0的第三方登录实践总结
categories: [技术]
tags:
  - OAuth
date: 2021-11-01 16:56:17
---

> OAuth 2.0 是目前最流行的授权机制，用来授权第三方应用，获取用户数据。


### 一、背景
- 最近在视频云项目的ToC观看端想引入用户登录功能。从用户需求的角度考虑，用户不喜欢仅为了发送一个弹幕或者评论而需要进行复杂的注册流程。目前很多网站都引入了第三方登录的方式，这样即解决了用户会忘记密码的问题，也简化了用户的登录流程，提高参与度。
- OAuth是目前主流的授权方式，值得系统学习下。

### 二、OAuth 2.0
##### 1. 令牌
- OAuth中授权使用的是token，而不是密码。
  - token具有时效性，可以随时被撤销，且权限范围受限
  - 相比于密码的方式，在实现授权给第三方的同时，具有较好的可控性。

##### 2. 角色
- 客户端：想要通过第三方登录的软件系统
- 资源所有者：授权方

##### 3. 授权层
- OAuth引入了授权层，来隔离这两个用户

<!--more-->

### 三、OAuth的四种授权方式
- 为了适应互联网各种不同的场景，OAuth提供了四种授权方式。
- 在授权之前，第三方系统需要到授权系统注册，说明自己的身份
  - 然后获得应用ID（app_id）以及应用密钥（app_secret)
- 下文中A系统为授权系统，比如微信、微博，B系统为第三方应用，比如我们的视频云系统。

##### 1. 授权码式（authorization-code）

- 最常用的方式，安全性也最高
- 需要应用后端支持，授权码由前端获取，令牌以及资源请求由后端进行，安全性高，能防止令牌泄露。
- 标志：response_type=code
- 主要流程：
  - 1、注册与登记
    - B系统到A系统的OAuth开放平台注册，获得app_id以及app_secret，A授权系统会提供一个用户授权的网页链接。
    - 注册时需要在开放平台配置redirect_uri等授权回调页以及取消授权回调页参数
  - 2、前端跳转到授权界面
    - B系统将A系统提供的授权链接放在前端登录位置，用户点击后，将带上app_id等参数跳转到A系统的授权界面。
    - state参数可以用来保持B系统当前的应用状态，在授权成功之后，会原封不动的返回。state需要存多个参数的时候，可以使用urlencode编码一下。
  - 3、用户登录A系统并授权
    - 用户通过密码登录A系统，如果A系统已经是登录状态，则直接进入授权界面。授权界面会展示B系统所请求的权限以及资源，用户检查后点击授权。
  - 4、B系统前端获取授权码
    - 授权成功，前端自动跳转到redirect_uri指定的网页，同时带上code以及state等query参数。
    - 其中code便是授权码，而state是之前传入的应用状态参数，用户恢复跳转前的应用状态。
  
    ```js
  // a系统提供的跳转授权链接例子
  https://a.com/oauth/authrize?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_RUL&scope=read
  // 参数
  response_type   // 对于授权码方式为code，指定为授权码方式
  redirect_uri    // 授权成功之后，将跳转到该页面
  scope           // 指定授权的方位，这里为只读权限
  state           // 可以传入随意的字符串参数，可用户保持B应用当前状态
  ```
  - 5、B后端向A系统请求令牌token
    - B前端从url中解析出code，将该code提交给B后端
    - B后端根据拿到的code、app_id以及app_secret向A系统提供的接口请求令牌token。

    ```js
  // B后端请求Token例子  
  https://b.com/oauth/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET
  &grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=CALLBACK_URL
    ```
  - 6、B后端向A系统请求资源
    - B后端获取到token后，使用该token获取用户在A系统中的资源，比如用户名，头像等。
  - 7、B后端将结果返回给B前端,前端恢复应用状态。

##### 2. 隐藏式（implicit）
- 不需要应用后端支持，前端直接请求令牌token，token存储在前端
- 适用于那些没有后端的纯前端应用，对安全性要求不高的场景，比如本博客。
- token的有效期不能太长。
- 标志：response_type=token
- 主要流程
  - 1、注册与登记，与授权码方式相同
  - 2、B前端跳转到A提供的授权界面
    - 与授权码方式不同的是，参数response_type为token，A系统将直接返回token令牌
  - 3、B前端获取令牌token
    - 授权成功之后，跳转到redirect_uri指定的网站，同时带上token
    - 与授权码式通过query参数返回不同，token以URL锚点（fragement）的方式返回给B前端
    - 以锚点方式返回能够避免中间人攻击问题，因为浏览器跳转时，锚点部分不会提交到服务器。
    - ```https://a.com/callback#token=ACCESS_TOKEN```
  - 4、B前端拿到token后，向A后端请求资源。

##### 3. 密码式（password）
- B网站直接在前端要求用户输入其在A系统的用户名和密码，然后直接通过用户名和密码获取A系统颁发的token
- 只适用于那些你高度信任的网站，或者其他方式没法使用的情况下，比如内部系统。
- 标志：grant_type=password
- 授权链接：`https://oauth.b.com/token?grant_type=password&username=USERNAME&password=PASSWORD&client_id=CLIENT_ID`

##### 4. 客户端凭证式（client credentials）
- 适用于没有前端的命令行应用，即在命令行下请求令牌
- 标志：grant_type=client_credentials

### 三、使用与更新
##### 1. 令牌的使用
- 每个发到 API 的请求，都必须带有令牌。具体做法是在请求的头信息，加上一个Authorization字段，令牌就放在这个字段里面

##### 2. 令牌的更新
- 令牌有效期到期后，再来一次完整流程比较麻烦，OAuth提供了令牌更新的机制。
- 标志：grant_type=refresh_token

### 四、在使用OAuth时候需要注意的安全问题
##### 1. CSRF劫持第三方账号
- 跨站请求伪造攻击
  - 攻击者利用自己获取到的授权码，在钓鱼网站上使用，该钓鱼网站会伪造用户请求，自动触发第三方授权过程。
  - 使用钓鱼网站的用户，自动触发第三方授权过程，若该用户当前登录了正常系统，且未与该第三方登录绑定。
  - 最终，正常网站将把该用户账号与攻击者第三方系统登录账号绑定，这样攻击者就可以通过第三方系统登录被攻击者的账号了。
- 根本原因：redirect_uri中的code参数没有和当前客户端的状态绑定，攻击者可以通过发送预先获取好的code参数到受害者电脑，导致导致受害者当前登录的应用方账号被绑定到攻击者指定的平台方（如微博）帐号上。

##### 2. 预防措施
- 引入第三方登陆的开发者，在OAuth认证过程中，加入state参数，验证它的参数便可以。
- 使用state参数的流程
  - 在将用户重定向到资源认证服务器授权界面的时候，为当前用户生成一个随机的字符串，并作为state参数加入到URL中，同时存储一份到 session 中。
  - 当第三方应用收到资源服务提供者返回的Authorization Code请求的时候，验证接收到的state参数值。

### 总结
- OAuth 在目前的互联网应用中使用的太多了，系统性了解它的原理很有用。有空可以给本博客集成下登录和评论系统了。
- 再一个体会，对于一些使用了OAuth第三方登陆的不是特别出名的小网站，真不敢再随便用自己的第三方账号随便登录了。

### 引用
- [[1]大神学长阮一峰的理解OAuth2.0](http://www.ruanyifeng.com/blog/2019/04/oauth_design.html)
- [[2]掘金上的一个博客](https://juejin.im/post/5cc81d5451882524f72cd32c)
- 微博、微信、QQ、github的授权认证接入文档