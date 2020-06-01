# OAuth2 权限认证
> OAuth是什么？<br/>
> &nbsp;&nbsp;&nbsp;&nbsp;
> OAuth 引入了一个授权层，用来分离两种不同的角色：客户端和资源所有者。得到资源所有者的同意后，
>资源服务器可以向客户端办法令牌。客户端通过令牌，去请求数据。**OAuth核心就是向第三方应用颁发令牌**，以下是四种授权方式
> - 授权码（authorization-code）
> - 隐藏式（implicit）
> - 密码式（password）
> - 客户端凭证（client credentials）
>

## 一、授权码
第一步，A 网站提供一个链接，用户点击跳转 B 网站，并授权用户数据给 A 网站使用。下面为 A 网站跳转 B 网站的一个示意链接。
```text
https://b.com/oauth/authorize?
  response_type=code&  （请求请求返回授权码）
  client_id=CLIENT_ID&   （让B知道是谁在请求）
  redirect_uri=CALLBACK_URL&  （B接收or拒绝请求后跳转的地址）
  scope=read    （授权范围）
```
第二步，用户跳转 B 后要求登录，并提示是否给予 A 授权，同意后，跳转redirect_uri网址，并包含授权码
```text
https://a.com/callback?code=AUTHORIZATION_CODE  （授权码）
```
第三步，A 拿到授权码后，可以通过后端向 B 网站请求令牌
```text
https:// b.com/oauth/token?
  client_id=CLIENT_ID&    （让B来确认A的身份）
  client_secret=CLIENT_SECRET&   
  grant_type=authorization_code&  （采用授权码的法师授权）
  code=AUTHORIZATION_CODE& （第2步骤返回的授权码）
  redirect_uri=CALLBACK_URL （令牌颁发后的回调地址）
```
第四步，B 网站接收请求，颁布令牌，向redirect_uri发送json参数
```json
{
  "//" : "ACCESS_TOKEN（token 令牌）",
  "access_token" : "ACCESS_TOKEN",
  "token_type" : "bearer",
  "expires_in" : 2592000,
  "refresh_token" : "REFRESH_TOKEN",
  "scope" : "read",
  "uid" : 100101,
  "info" : {}
}
```

## 二、密码式
第一步，A 网站要求 B 网站提供用户名密码
```text
https:// oauth.b.com/token?
  grant_type=password&   （认证方式为：密码）
  username=USERNAME&     （用户名）
  password=PASSWORD&     （密码）
  client_id=CLIENT_ID
```
第二步，B 网站验证身份后，直接给予令牌，令牌信息放在 JSON 中作为 HTTP 返回值。<br/>
注：此方式需要提供出自己的用户名/密码，显然风险会比较大。

## 三、隐藏式
第一步，A 网站提供链接，要求用户跳转 B 网站，授权用户数据给 A 网站使用
```text
https://b.com/oauth/authorize?
  response_type=token&   （表示直接返回token令牌）
  client_id=CLIENT_ID&
  redirect_uri=CALLBACK_URL&  （需要跳转的网址）
  scope=read
```
第二步，用户登录 B 网站，同意授权给 A 网站，B 跳转回 `redirect_uri`，并将令牌作为参数传给 A
```text
https://a.com/callback#token=ACCESS_TOKEN
```
令牌的位置是 URL 锚点（fragment），而不是查询字符串（querystring），这是因为 OAuth 2.0 允许跳转网址是 HTTP 协议，
因此存在"中间人攻击"的风险，而浏览器跳转时，锚点不会发到服务器，就减少了泄漏令牌的风险。<br/>
注：这种方式将令牌传给前端，很不安全。只能应用与一些安全性不高的场景。令牌有效期比较短，随着`session`有效，浏览器关掉，令牌即失效。

##  四、凭证式
第一步、直接根据 client 的 id 和密钥即可获取 token
```text
https:// oauth.b.com/token?
  grant_type=client_credentials&  （客户端凭证式）
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET   （客户端密钥）
```
第二步，B 网站验证通过后，直接返回令牌

## 令牌的使用
A 网站拿到令牌后，在令牌放在请求头上增加 `Authorization` 字段中，就可以向 B 网站进行请求了。
```shell script
curl -H "Authorization: Bearer ACCESS_TOKEN" \
"https://api.b.com"
```

## 令牌更新
令牌有效期到了以后，在重新走一遍流程来申请一个令牌，显然这样的方式比较复杂。<br/>
采用 B 网站办法令牌时，一次性发布两个令牌，一个令牌用于数据获取，另一个用于获取新的令牌。
```text
https://b.com/oauth/token?
  grant_type=refresh_token&   （类型更新令牌）
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET&
  refresh_token=REFRESH_TOKEN   （用于更新令牌的令牌）
```

参考：
- [OAuth 2.0 的四种方式](http://www.ruanyifeng.com/blog/2019/04/oauth-grant-types.html), by 阮一峰