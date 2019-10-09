### OAuth 授权介绍

## 标准登录

可直接访问 OAuth 登录地址进行登录，`http://domain/oauth/login`，需配置默认的登录成功跳转地址，否则登录成功后会默认跳回到登录页面。

登录的字段默认支持 `用户名(username)、邮箱(email)、手机号(phone)`，可通过 `support-fields` 配置限制登录方式。

```
hzero:
  oauth:
    # 标题
    title: ${HZERO_OAUTH_TITLE:HZERO}
    login:
      # 允许使用的登录名，默认有 用户名、邮箱、手机号
      support-fields: ${HZERO_OAUTH_LOGIN_SUPPORT_FIELDS:username,email,phone}
      # 默认登录成功跳转地址
      success-url: ${HZERO_OAUTH_LOGIN_SUCCESS_URL:http://domain/index}
```

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1552480655.jpg)

## 用户锁定

登录流程中，用户校验主要会涉及 账号有效性、租户有效性、角色分配 等校验。

其中，如果用户输入密码错误，增加密码错误次数 (通过缓存记录)，查询用户所属租户的安全策略，接着检查是否达到了`最大错误次数`，如果`允许锁定用户`，则`将用户锁定`。

用户密码错误，如果`启用了验证码`，且密码错误次数超过`验证码检查阀值`则会校验验证码，进入登录页面时会判断是否显示验证码。

用户锁定之后可以通过`忘记密码`找回，或者在`账户管理`处解锁用户。

- 与用户密码、登录相关的请在 **安全策略** 下配置。 ![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568094984.png)

## Web 端授权

**OAuth 标准登录授权流程**

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1563627951.png)

前后端分离下，web 端登录采用 `简化模式` 授权，由前端页面跳转到 oauth 登录页面统一登录，前端检查到用户未登录时 (返回 `401` 状态码)，跳转到 oauth 进行用户认证，获取 `access_oken`。

- 1、访问接口返回 401

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1552994434.jpg)

- 2、访问 `/oauth/authorize` 授权端点

前端检测到 `401` 时，访问 `http://domain/oauth/oauth/authorize?response_type=token&client_id=client&redirect_uri=redirect_uri` 地址，将用户引导到 oauth 登录。

```
response_type: 授权类型，固定 token
client_id: 客户端名称，需在 oauth_client 表中配置客户端
redirect_uri: 登录成功后的重定向地址，需与 client 对应的重定向地址保持一致
```

其它可传参数

```
template: 使用的登录模板，默认有 main、portal，也可以自己按照标准目录开发模板，请求 /authorize 接口时通过该参数传入要使用的模板即可
logout_redirect_uri: 退出后重定向地址，可通过该参数指定退出成功后跳转的地址
user_type: 控制登录用户类型，中台用户-P/C端用户-C
```

实际上该地址是进入到 `spring security` 的过滤器链，spring security 检查到用户未登录，会重定向到登录页面

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1552994650.jpg)

- 3、用户登录

用户登录成功后，重定向到 `/oauth/authorize` 端点，获取 `access_token`。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1552995874.jpg)

- 4、登录成功，返回 access_token

`/oauth/authorize` 端点成功创建 `access_token` 后，根据 `redirect_uri` 并在 uri 后拼接 access_token 相关参数，重定向回去，至此授权完成。前端拿到 access_token 后，再访问后端服务。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1552996024.jpg)

```
access_token: 授权令牌
expires_in: 令牌过期时间
scope: 授权范围
```

- 5、access_token 唯一性

web 端通过加入 `sessionId` 来控制 access_token 的唯一性，这个 session 是来自 oauth 服务的 session。

- 6、图片验证码

如果想每次进入登录页面时，都启用验证码，需启用如下配置

```yaml
hzero:
  oauth:
    # 始终开启图形验证码校验，默认否
    enable-always-captcha: ${HZERO_OAUTH_ENABLE_ALWAYS_CAPTCHA:false}
```

## 移动端授权

移动端授权可以使用 `密码模式` 给用户认证授权，移动端有自己的 app 登录入口，不能跳转到 web 页面登录。APP 登录页面，用户输入用户名和密码即可完成登录。

**注意**：移动端登录时，密码需要用 RSA 加密后传输到后端。

- 1、访问 `/oauth/token` 授权端点

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1552998800.jpg)

```
grant_type: 授权类型，固定为 password
client_id: 客户端名称
client_secret: 客户端密钥
source_type: 登录来源，移动设备固定为 app
device_id: 登录设备ID，如果不传入设备ID，用户在多个设备登录返回的 access_token 是一样的
username: 登录用户名
password: 登录密码
user_type: 用户类型，中台用户-P/C端用户-C
```

**移动设备登录，需传入 source_type=app 标识**

- 2、配置项说明

```yaml
hzero:
  oauth:
    # 验证 client 时不检查 client 的一致性
    not-check-client-equals: ${HZERO_OAUTH_NOT_CHECK_CLIENT_EQUALS:false}
    # 移动设备ID参数
    device-id-parameter: ${HZERO_OAUTH_DEVICE_ID_PARAMETER:device_id}
    # 登录设备来源参数
    source-type-parameter: ${HZERO_OAUTH_SOURCE_TYPE_PARAMETER:source_type}
```

- 3、access_token 唯一性

移动设备登录，需传入 source_type=app 标识，通过 device_id 控制 access_token 的唯一性，可保证不同的设备获取到的 access_token 是唯一的。

## 并发登录控制

可在 **安全策略** 下配置 WEB 端或移动端 是否允许同一个用户在多处登录，移动端 和 WEB 端互不影响。当不允许用户多处登录时，用户在一处登录，自动失效其它 access_token。允许多处登录时，一个用户可以同时在多处登录，由于 access_token 不一样，也互不影响。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568094864.png)

## 其它配置

- 手机登录可配置发送验证码的模板，以及修改密码发送验证码的模板。
- 退出时若不清理 token ，那么下次登录时返回的 access_token 跟之前登录的一样。若清理 token，每次登录都会创建新的 access_token。

```yaml
hzero:
  send-message:
    # 手机登录发送验证码模板代码
    mobile-login: HOTH.MOBILE_LOGIN
    # 修改密码发送验证码模板代码
    modify-login-password: HOTH.MODIFY_PASSWORD
  oauth:
    logout:
      # 退出时是否清理token
      clear-token: ${HZERO_OAUTH_LOGOUT_CLEAR_TOKEN:true}
```

## 刷新 access_token

前端通过判断 access_token 过期时间，自动调用 `/oauth/token` 端点刷新 access_token

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1553006343.jpg)

```
grant_type: 授权类型，固定为 refresh_token
refresh_token: 获取 access_token 时返回的 refresh_token
client_id: 客户端名称
client_secret: 客户端密钥
```

## 冻结下线用户

子账户冻结用户后，会使用户下线。 oauth 服务提供两个接口，可删除 access_token，使用户下线

- 根据 clientId 和登录名下线
  - URI： [DELETE] /admin/token/{clientId}
  - 参数：clientId - 客户端 ID；loginNameList - 用户登录名集合
- 根据登录名下线
  - URI： [DELETE] /admin/token
  - 参数：loginNameList - 用户登录名集合

## 授权码获取用户信息

授权码标准模式是通过授权码换取 access_token，通过 **[GET] /oauth/user** 接口可通过授权码直接返回用户信息，请求参数跟授权码标准模式一致。

## OAuth 配置

```yaml
hzero:
  captcha:
    # 是否启用验证码
    enable: true
  send-message:
    # 手机登录发送验证码模板代码
    mobile-login: HOTH.MOBILE_LOGIN
    # 修改密码发送验证码模板代码
    modify-login-password: HOTH.MODIFY_PASSWORD
  oauth:
    # 认证服务器 自定义资源匹配器
    custom-resource-matcher: ${HZERO_OAUTH_CUSTOM_RESOURCE_MATCHER:false}
    # 验证 client 时不检查 client 的一致性
    not-check-client-equals: ${HZERO_OAUTH_NOT_CHECK_CLIENT_EQUALS:false}
    # 移动设备ID参数
    device-id-parameter: ${HZERO_OAUTH_DEVICE_ID_PARAMETER:device_id}
    # 登录设备来源参数
    source-type-parameter: ${HZERO_OAUTH_SOURCE_TYPE_PARAMETER:source_type}
    # 始终开启图形验证码校验，默认否
    enable-always-captcha: ${HZERO_OAUTH_ENABLE_ALWAYS_CAPTCHA:false}
    # 标题
    title: ${HZERO_OAUTH_TITLE:HZERO}
    login:
      # 允许使用的登录名，默认有 用户名、邮箱、手机号
      support-fields: ${HZERO_OAUTH_LOGIN_SUPPORT_FIELDS:username,email,phone}
      # 手机登录参数
      mobile-parameter: phone
      # 前端默认模板
      default-template: ${HZERO_OAUTH_LOGIN_DEFAULT_TEMPLATE:main}
      # 默认登录成功跳转地址
      success-url: ${HZERO_OAUTH_LOGIN_SUCCESS_URL:http://hzeronf.saas.hand-china.com/workplace}
    logout:
      # 退出时是否清理token
      clear-token: ${HZERO_OAUTH_LOGOUT_CLEAR_TOKEN:true}
```

