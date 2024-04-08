---
title: Oauth2.0
top: false
cover: false
toc: true
mathjax: true
date: 2024-04-07 21:14:41
password:
summary:
tags: [java面试,Oauth2.0]
categories: java面经
---

# Oauth2.0

## 认证流程

(1)传统授权流程

![客 户  令 牌 台 法 ， 返 回 资 源  申 清 令 牌  权 务  生 成 令 牌  返 回 令 牌  校 令 牌 的 台 法 性  返 回 校 结 果 及 用 户 信  令 牌 访 问 资 源  资 源 务 器 ](Oauth2-0/clip_image002.gif)

资源服务器授权流程如上图，客户端先去授权服务器申请令牌，申请令牌后，携带令牌访问资源服务器，资源服务器访问授权服务校验令牌的合法性，授权服务会返回校验结果，如果校验成功会返回用户信息给资源服务器，资源服务器如果接收到的校验结果通过了，则返回资源给客户端。

传统授权方法的问题是用户每次请求资源服务，资源服务都需要携带令牌访问认证服务去

校验令牌的合法性，并根 据令牌获取用户的相关信息，性能低下。 

(2)公钥私钥授权流程

![申 情 令 牌  客 户  私 钥  生 成 令 牌  返 回 令 牌  携 諾 令 牌 访 向 源  资 源 服 务 器  公 钥  校 令 牌 的 台 法 性  令 台 进 ， 逛 回 资 源 ](Oauth2-0/clip_image004.gif)

传统的授权模式性能低下，每次都需要请求授权服务校验令牌合法性，我们可以利用公钥私钥完成对令牌的加密，如果加密解密成功，则表示令牌合法，如果加密解密失败，则表示令牌无效不合法，合法则允许访问资源服务器的资源，解密失败，则不允许访问资源服务器资源。

## 项目开发的认证流程：

![O  登 录 页 面  0  资 源 页 面  O  退 出 页 面  令 牌 仕 鼓 巴 n 〕 添 加 到 c 囗 鼓 中  C 囗 囗 巴 ： t 囗 en  校 令 牌 ， 不 台 法 则 拒 绝 访 问  de 1 巴 ： C 囗 囗 巴 ： t 囗 en  认 务  务 网 关  认 务 ](Oauth2-0/clip_image006.gif)

1、用户登录，请求认证服务 

2、认证服务认证通过，生成jwt令牌，将jwt令牌及相关信息写入cookie 

3、用户访问资源页面，带着cookie到网关 

4、网关从cookie获取token，如果存在token，则校验token合法性，如果不合法则拒绝访问，否则放行 

5、用户退出，请求认证服务，删除cookie中的token

认证服务需要实现的功能如下： 

1、登录接口 

前端post提交账号、密码等，用户身份校验通过，生成令牌，并将令牌写入cookie。 

2、退出接口 校验当前用户的身份为合法并且为已登录状态。 将令牌从cookie中删除。

![O  /user/login  changgou-user-oauth  Spr ingSecuri ty  /user/login->hft'fi  @NLILL %LIserDetai Is  LIserDetai 1sServiceImp1  NULL  LIserDetai Is ](Oauth2-0/clip_image008.gif)

业务层

![0  登 录 页 面  0 & 此 h2  Controller  将 令 牌 。 0 ） 添 加 到 0 “ k “ 中  令 牌 息  Service ](Oauth2-0/clip_image010.gif)

如上图，我们现在实现一个认证流程，用户从页面输入账号密码，到认证服务的Controller层，Controller层调用Service层，Service层调用OAuth2.0的认证地址，进行密码授权认证操作，如果账号密码正确了，就返回令牌信息给Service层，Service将令牌信息给Controller层，Controller层将数据存入到Cookie中，再响应用户。

## 单点登录与OAuth的关系

用户访问的项目中，至少有3个微服务需要识别用户身份，如果用户访问每个微服务都登录一次就太麻烦了，需要实现让用户在一个系统中登录，其他任意受信任的系统都可以访问，这个功能就叫单点登录。

单点登录（Single Sign On），简称为 SSO。

![ffZNSSO? ,  SSO Single Sign On —nn, "BEN.  Spring Security ](Oauth2-0/clip_image012.jpg)

## 什么是认证和授权

![． 什 么 是 认 证 和 授 权 ？ 如 何 设 计 一 个 权 限 认 证 框 架 ？  认 证 ； 就 是 对 系 骁 访 回 占 的 身 佾 讲 巧 确 认 ． 用 户 名 醪 0 录 ． 二 鲢 醪 丰 机 矧 信 0 ． 坟 ． 刷 ． ·  授 权 ， 就 是 对 系 晚 访 问 占 的 行 为 行 制 · 授 板 0 斓 是 在 认 过 之 后 ， 系 内 的 用 户 河 私 数 的 行 护 · 后 台 接 冂 访 问 仅 、  前 台 怿 的 砺 。  RBACE' 主 体 一 } 角 色 一 》 源 ． 》 访 回 系 统 蝓 巧 为 ·  认 证 和 覆 也 是 对 一 个 艱 认 梅 架 行 扩 層 的 两 主 要 的 方 面 到 ](Oauth2-0/clip_image014.jpg)

## 定义和几种模式

授权码模式，简化模式，密码模式，客户端认证模式

![img](Oauth2-0/clip_image016.jpg)

![img](Oauth2-0/clip_image017.jpg)

认证服务需要实现的功能如下：

1、登录接口 

前端post提交账号、密码等，用户身份校验通过，生成令牌，并将令牌写入cookie。 

2、退出接口 校验当前用户的身份为合法并且为已登录状态。 将令牌从cookie中删除。 

 

![img](Oauth2-0/clip_image018.jpg)

三方都不互信的情况：百度是看不到用户授权的，用户是看不到令牌的

![img](Oauth2-0/clip_image019.jpg)

剩余两方对用户的信任增加了，让用户知道令牌了

![img](Oauth2-0/clip_image020.jpg)

用户和百度完全互信，直接告诉百度用户名密码去申请令牌

![img](Oauth2-0/clip_image021.jpg)

三方完全互信，直接用客户端id和秘钥去申请令牌

https://www.bilibili.com/video/BV1Kt4y1i7nk?from=search&seid=10964394913623719590

## 设计一个Oauth2.0协议

使用授权码模式完成OAuth2.0授权的过程需要以下三个步骤：

client请求授权服务端，获取Authorization Code；

client通过Authorization Code再次请求授权服务端，获取Access Token；

client通过服务端返回的Access Token获取用户的基本信息

因此，OAuth2.0授权服务端的设计也就主要围绕这几个接口展开，其主要流程是这样的：

![img](Oauth2-0/clip_image022.jpg)

https://www.zifangsky.cn/1313.html

## 如何设计开放授权平台

![img](Oauth2-0/clip_image024.jpg)

## JWT令牌，和普通令牌有什么区别

![lb9f2eaa1-8715.gp.B6c7-06bf757a5f7c  ImFk W. liwi N  TizzmEtN 2 IhZGJlL NTIm C161mMKlno.tJ5d7R8KP8d6w78260 2 Dt ZXV ](Oauth2-0/clip_image026.jpg)

### JWT的构成

一个JWT实际上就是一个字符串，它由三部分组成，头部、载荷与签名。

### 头部（Header）

头部用于描述关于该JWT的最基本的信息，例如其类型以及签名所用的算法

等。这也可以被表示成一个JSON对象。

{"typ":"JWT","alg":"HS256"}

在头部指明了签名算法是HS256算法。 

### 载荷（playload）

载荷就是存放有效信息的地方。这个名字像是特指飞机上承载的货品，这些有效信息包含三个部分

（1）标准中注册的声明（建议但不强制使用）

iss: jwt签发者

sub: jwt所面向的用户

aud: 接收jwt的一方

exp: jwt的过期时间，这个过期时间必须要大于签发时间

nbf: 定义在什么时间之前，该jwt都是不可用的.

iat: jwt的签发时间

jti: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。

（2）公共的声明

公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息.但不建议添加敏感信息，因为该部分在客户端可解密.  

（3）私有的声明

私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为base64是对称解密的，意味着该部分信息可以归类为明文信息。

这个指的就是自定义的claim。比如下面面结构举例中的admin和name都属于自定的claim。这些claim跟JWT标准规定的claim区别在于：JWT规定的claim，JWT的接收方在拿到JWT之后，都知道怎么对这些标准的claim进行验证(还不知道是否能够验证)；而private claims不会验证，除非明确告诉接收方要对这些claim进行验证以及规则才行。

定义一个payload:

{"sub":"1234567890","name":"John Doe","admin":true}

### 签证（signature）

jwt的第三部分是一个签证信息，这个签证信息由三部分组成：

 header (base64后的)

 payload (base64后的)

secret

这个部分需要base64加密后的header和base64加密后的payload使用.连接组成的字符串，然后通过header中声明的加密方式进行加盐secret组合加密（第一部分+第二部分，再用secret+加盐加密），然后就构成了jwt的第三部分。加盐加密：就是人为的通过一组随机字符与用户原密码的组合形成一个新的字符。

TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ

将这三部分用.连接成一个完整的字符串,构成了最终的jwt:

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ

**注意**：secret是保存在服务器端的，jwt的签发生成也是在服务器端的，secret就是用来进行jwt的签发和jwt的验证，所以，它就是你服务端的私钥，在任何场景都不应该流露出去。一旦客户端得知这个secret, 那就意味着客户端是可以自我签发jwt了。根据以下公式生

成签名：HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload),secret)

## 什么是CSRF攻击？如何防止

![img](Oauth2-0/clip_image028.jpg)

如果某一个Cookie 选项被设置成 HttpOnly = true 的话，那此Cookie 只能通过服务器端修改，Js 是操作不了的，对于 document.cookie 来说是透明的。

![img](Oauth2-0/clip_image030.jpg)

### sso和OAuth2.0的关系

## Cookie和Session区别

![img](Oauth2-0/clip_image032.jpg)

 