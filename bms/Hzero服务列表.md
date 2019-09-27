# HZERO 服务列表

- [平台文档链接](http://hzerodoc.saas.hand-china.com/zh/docs/service/register/)

- [oauth 服链接](http://hzerodoc.saas.hand-china.com/zh/docs/service/oauth/oauth/)

- 注册中心：
  - 服务注册发现
  - 服务健康检查
  - 服务监控
  - 注册中心其他功能

- 配置中心：
  - 服务管理
  - 服务配置管理
  - 服务动态路由

- 网关服务：
  - 动态路由
  - API 访问限流
  - API 访问熔断
  - 用户鉴权
  - 整体运维

- 认证服务：见下

- 事务服务：
  - 事务定义
  - 事务实例

- IAM 服务
  - 角色管理
  - 菜单管理
  - 帐户管理
  - 用户组管理
  - 租户管理
  - 权限刷新
  - 单据权限管理

- Swagger 测试服务
  - API 文档查看
  - 服务 API 文档刷新
  - API 调试

- 平台基础服务

  - 系统配置
  - 值列表维护、值列表视图
  - 多语言描述维护
  - 编码规则管理
  - 配置管理
  - 规则引擎
  - 数据源、数据库管理

- 文件服务

  - 文件存储配置
  - 文件上传控制
  - 文件汇总查询
  - 文件在线编辑
  - 文件变更记录
  - 文件预览

- 消息服务

  - 模板管理
  - 短信账户配置
  - 邮箱账户配置
  - 消息发送配置

- 调度服务

  - 执行器管理
  - 调度任务管理
  - 并发可执行
  - 并发请求

- 调用导入服务

  - 模版管理
  - 模板下载
  - 通用上传界面
  - 通用上传封装接口拓展

- 接口服务

  - 接口配置

    - 服务注册：注册服务、接口，配置服务级别的认证信息；以及配置接口的运维信息、文档、测试用例等管理信息
    - 接口能力汇总：汇总展示平台通用接口能力以及本租户内的接口能力，并标识自己所拥有的接口能力；无权限的接口能力可通过接口文档查看接口信息；有权限的接口能力可配置不同维度的认证信息
  
- 接口授权
    - 角色授权：将接口能力授权给角色
  - 客户端授权：将角色授权给客户端
  - 接口运维
    - 接口监控：可监控接口调用详情信息
  - 健康状况监控：配置健康检查功能后，可通过此处查看近期健康检查的异常情况
  
- 数据分发服务

  - 初始化
    - 初始化时自动同步表结构
    - 仅初始化表结构
    - 单库历史数据初始化
  - 同步类型
    - 单表变更同步
    - 单表批量变更同步
    - 主表 + 多语言表变更同步
  - 同步维度
    - 整张表维度同步
    - 租户维度同步

- 新版工作流服务

  - 在原工作流的基础上整合配置，修复若干 Bug，将编辑器和服务整合在一起

- 监控审计服务

  - 操作审计

  ```yaml
  hzero:
    audit:
      operation:
        enable: false     # 全局开关，默认 false
        api-audit:
          enable: true    # API 审计开关，默认 true，如果全局开关关闭，此值无效
        annotation-audit:
          enable: true    # 注解审计（在Bean的方法上添加@OperationalAudit）开关，默认 true，如果全局开关关闭，此值无效
  ```

  - 数据审计

  ```yaml
  # application.yml
  hzero:
    transfer:
      monitor:
        enabled: true # 是否启用数据变更监控功能
      dataAudit:
        enabled: true # 是否启用数据变更审计功能
  ```

- 报表服务

  - 数据集管理
  - 报表定义
  - 报表查询
  - 报表模板管理
  - 其他报表使用的 API

- 内容提取

  - 基础数据管理
  - 模板管理
  - 词语映射
  - 内容提取测试

- 即时通讯

  - 群组管理
  - 好友管理
  - 消息单聊
  - 消息群聊

- 支付服务

  - 支付配置
  - 支付订单
  - 退款订单

- 发票服务

  - 根据发票六要素手工查验发票
  - 根据发票图片识别，并完成发票查验

- 前端服务：[平台文档对应链接](http://hzerodoc.saas.hand-china.com/zh/docs/service/front/)

---

- Web 端授权 OAuth 标准登录授权流程
<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/OAuth 标准登录授权流程.png"/></div>
---
<div align=center><img src="https://mortre-picgo.oss-cn-beijing.aliyuncs.com/Hzero服务调用链路.jpg"/></div>


---



# OAuth 认证服务

------

> 组件编码 `hzero-oauth`

## 简介

`hzero-oauth` 服务是基于 `Spring Security`、`Spring OAuth2`、`JWT` 实现的统一认证服务中心，登录基于 spring security 的标准登录流程，客户端授权支持 `oauth2.0` 的四种授权模式：`授权码模式`、`简化模式`、`密码模式`、`客户端模式`，授权流程跟标准的 oauth2 流程一致。web 端采用`简化模式(implicit)` 登录系统，移动端可使用`密码模式(password)` 登录系统 。并支持基于 `Spring Social` 的三方账号登录方式 (如微信)。

## 组件坐标

```xml
<dependency>
    <groupId>org.hzero</groupId>
    <artifactId>hzero-oauth</artifactId>
    <version>${hzero.service.version}</version>
</dependency>
```

## 主要功能

- 统一登录界面
- 账户、手机、邮箱登录
- 短信登录
- 第三方登录功能
- 可客制化登录模板

## 服务配置

OAuth 服务的参数配置使用场景可具体参考 OAuth 服务下的其它文档。

```yaml
hzero:
  send-message:
    # 手机登录发送验证码模板代码
    mobile-login: HOTH.MOBILE_LOGIN
    # 修改密码发送验证码模板代码
    modify-login-password: HOTH.MODIFY_PASSWORD
  oauth:
    # 认证服务器 自定义资源匹配器
    # 可实现 ResourceMatcher 接口，自定义 OAuth ResourceServer 对哪些API认证
    custom-resource-matcher: false
    # 授权码模式验证 client 时不检查 client 的一致性
    not-check-client-equals: false
    # 移动设备ID参数
    device-id-parameter: device_id
    # 登录设备来源参数
    source-type-parameter: source_type
    # 移动端开启图形验证码校验，默认不开启
    enable-app-captcha: false
    # 始终开启图形验证码校验，默认否
    # 设置为 true 时，在进入登录页面时就显示图形验证码
    enable-always-captcha: false
    # client_credentials 模式是否返回 refresh_token，标准模式不返回
    credentials-allow-refresh: false
    # 登录页面标题
    title: HZERO
    login:
      # 允许使用的登录名，默认有 用户名、邮箱、手机号
      support-fields: username,email,phone
      # 手机登录参数
      mobile-parameter: phone
      # 登录页面默认模板，oauth 提供了两套模板
      # 在请求 /oauth/authorize 接口时，可通过 template 参数指定使用的模板
      default-template: main
      # 默认登录成功跳转地址
      # 在直接访问 oauth 的登录地址时，登录成功后会跳转到这个默认地址
      success-url: http://dev.hzero.org/workplace
    logout:
      # 退出时是否清理token
      clear-token: true
    sso:
      # 是否启用二级域名单点登录
      enabled: false
      service: 
        # oauth 服务地址
        baseUrl: http://dev.hzero.org:8080/oauth
      # SAML 协议登录相关配置  
      saml:
        entity_id: hzero:org:sp
        passphrase: secret
        private_key: MIIEvQIBADANBgk......
        certificate: MIIDEzCCA.......
    password:
      # 是否启用 RSA 加密
      enable-encrypt: true
      # 密码传输加密：公钥
      public-key: MFwwDQYJKoZIhvcNAQEBB......
      # 密码传输加密：私钥
      private-key: MIIBVQIBADANBgkqhkiG......
```



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



### OAuth Token API

## 说明

本文档主要介绍与用户登录相关的客户端获取 Token 接口，及一些特殊说明，登录授权介绍请参考 [OAuth 授权介绍](http://hzerodoc.saas.hand-china.com/zh/docs/service/oauth/oauth)

## OAuth 密码模式

- API：POST /oauth/oauth/token
- 场景：需要用户登录，通过 `用户名+密码` 获取 Token

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559623043.png)

## OAuth 客户端模式

- API：POST /oauth/oauth/token
- 场景：无需用户登录，只获取 Token，一般用于外部系统接口调用

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559623482.png)

## 标准登录接口

> 登录接口

- API：POST /oauth/oauth/token
- 场景：仿 Web 端登录，需要用户登录，且需要用户输入验证码

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559624776.png)

- 请求参数

| 参数          | 说明                                                         | 必输 |
| :------------ | :----------------------------------------------------------- | :--- |
| grant_type    | 授权类型，固定 `password`                                    | Y    |
| client_id     | 客户端 ID                                                    | Y    |
| client_secret | 客户端密钥                                                   | Y    |
| username      | 登录账号：可以使用 `登录账号/手机号/邮箱` 进行认证           | Y    |
| password      | 登录密码：使用 RSA 加密后传输                                | Y    |
| source_type   | 设备来源，web-Web 端 /app - 移动设备端                       | Y    |
| device_id     | 设备 ID，可以使用 UUID，用于设置 Token 的唯一性              | Y    |
| user_type     | 用户类型：P - 平台用户 / C-C 端用户，默认 P                  | N    |
| login_field   | 登录字段，控制只能使用某个字段登录，username/phone/email，默认三个字段都支持 | N    |
| captcha       | 用户输入的验证码                                             | N    |
| captchaKey    | 生成验证码返回的 captchaKey                                  | N    |

> 获取初始化参数

- API：GET `/oauth/login/init-params?channel={channel}&client_id={clientId}`
- 场景：进入登录页面时，调用该接口获取一些初始化信息

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568085294.png)

- 请求参数

| 参数     | 说明                                               |
| :------- | :------------------------------------------------- |
| channel  | 登录渠道，查询对应渠道的三方登录方式               |
| clientId | 客户端 ID，用于查询客户端所属租户                  |
| username | 用户登录失败时可以传入用户名，用于查询用户所属租户 |

- 响应参数

| 参数          | 说明                   |
| :------------ | :--------------------- |
| openLoginWays | 三方登录方式           |
| isNeedCaptcha | 是否需要输入图形验证码 |

> 生成验证码

- API：GET /oauth/public/captcha-code
- 场景：如果 `isNeedCaptcha = true`，首先调用该接口获取验证码的 Key

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559626663.png)

> 获取图形验证码

- API：GET /oauth/public/captcha/{captchaKey}
- 场景：通过上一步得到 captchaKey 后，再调用该接口获取验证码图片

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559626806.png)

## 手机短信登录

> 登录接口

- API：POST /oauth/token/mobile
- 场景：通过 `手机+短信验证码` 登录，需要用户输入手机验证码

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559628288.png)

- 请求参数

| 参数          | 说明                                            | 必输 |
| :------------ | :---------------------------------------------- | :--- |
| grant_type    | 授权类型，固定 `implicit`                       | Y    |
| client_id     | 客户端 ID                                       | Y    |
| client_secret | 客户端密钥                                      | Y    |
| phone         | 手机号                                          | Y    |
| source_type   | 设备来源，web-Web 端 /app - 移动设备端          | Y    |
| device_id     | 设备 ID，可以使用 UUID，用于设置 Token 的唯一性 | Y    |
| user_type     | 用户类型：P - 平台用户 / C-C 端用户，默认 P     | N    |
| captcha       | 用户输入的手机验证码                            | Y    |
| captchaKey    | 发送手机验证码返回的 captchaKey                 | Y    |

> 获取手机验证码

- API：/oauth/public/send-phone-captcha

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559628605.png)

## 接口返回说明

**请求失败**

可通过 `success=false` 判断接口是否调用失败，如果调用失败，返回失败编码 `code` 及消息 `message`。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559610841.png)

**请求成功**

请求成功则返回 access_token

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1559613450.png)

**接口返回码**

| 返回编码                                    | 说明                                                         |
| :------------------------------------------ | :----------------------------------------------------------- |
| hoth.warn.usernameNotFound                  | 您输入的登录账户不存在                                       |
| hoth.warn.phoneNotFound                     | 您输入的手机号未注册                                         |
| hoth.warn.userNotActive                     | 您的账号未激活，无法登录                                     |
| hoth.warn.accountLocked                     | 您的账户已锁定，请通过找回密码解锁，或联系平台运维中心       |
| hoth.warn.accountExpire                     | 您的账户已过期，无法登录                                     |
| hoth.warn.tenantInvalid                     | 您的所属组织无效，请联系管理员                               |
| hoth.warn.tenantDisabled                    | 您的所属组织已被禁用，请联系管理员                           |
| hoth.warn.passwordError                     | 您的密码错误，还可以尝试 {0} 次                              |
| hoth.warn.usernameNotFoundOrPasswordIsWrong | 您输入的账号或密码错误                                       |
| hoth.warn.loginErrorMaxTimes                | 密码错误次数超过限制，您的账户已锁定！请通过找回密码解锁，或联系平台运维中心！ |
| hoth.warn.ldapIsDisable                     | LDAP 已被禁用                                                |
| hoth.warn.captchaNull                       | 请输入验证码                                                 |
| hoth.warn.captchaWrong                      | 您输入的验证码有误                                           |
| hoth.warn.phoneNotCheck                     | 您的手机未验证，可使用邮箱或账号登录                         |
| hoth.warn.emailNotCheck                     | 您的邮箱未验证，可使用手机或账号登录                         |
| hoth.warn.phoneAndEmailNotCheck             | 您的手机和邮箱均未验证，可使用账号登录                       |
| hoth.warn.defaultTenantRoleNull             | 您的默认租户下没有分配角色，请联系管理员分配角色             |
| hoth.warn.loginMobileCaptchaNull            | 请输入手机验证码                                             |
| hoth.warn.validCaptchaFirst                 | 请先验证手机 / 邮箱                                          |
| hoth.warn.accountNotEquals                  | 您的账号与验证的账号不一致                                   |
| hoth.warn.captcha.sendPhoneCaptchaError     | 发送手机验证码失败，请稍后重试                               |
| hoth.warn.phoneOrEmailNotnull               | 请输入手机 / 邮箱                                            |
| hoth.warn.phoneOrEmailNotFound              | 您输入的手机 / 邮箱不存在                                    |
| hoth.warn.phoneOrEmailNotInvalid            | 您输入的手机 / 邮箱格式不正确                                |
| hoth.user.phone.not-register                | 手机未注册，请先注册后再登录                                 |
| hoth.warn.captchaNotnull                    | 请输入验证码                                                 |
| hoth.warn.ldapCannotChangePassword          | LDAP 用户不能进行修改密码操作                                |
| hoth.warn.roleNone                          | 您没有任何角色，请联系管理员分配角色                         |
| hoth.warn.decodePasswordError               | 解码密码错误，请用 Base64 加密后传输                         |
| hoth.warn.createClientError                 | 创建 Token 错误                                              |



### 多域名单点登录

## 多域名单点登录

- 多域名单点登录概述：

用于集成外部 CAS，OAUTH2，SAML 等协议的单点登录，访问配置好的二级域名，自动跳转到对应单点登录页进行认证。

- 单点登录启用配置：

```yml
hzero:
  ### SSO配置 ###
  oauth:
    sso:
      # 是否启用二级域名单点登录
      enabled: true
      # oauth服务基础地址，用于单点登录回调
      service: 
        baseUrl: http://dev.hzero.org:8080/oauth
```

- 多域名单点登录效果：

\1. 在系统管理 -》域名配置中，创建单点登录配置

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/CAS%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E9%85%8D%E7%BD%AE.png)

\2. 浏览器打开配置的域名 `http://cas.hzero.org` , 自动跳转到单点登录页面，并附带回调地址

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/CAS%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E9%A1%B5.png)

\3. 单点登录页登录成功后，跳转回 `http://cas.hzero.org`

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/CAS%E7%99%BB%E5%BD%95%E6%88%90%E5%8A%9F%E9%A1%B5.png)

## Cas 单点登录集成

- Cas 单点登录流程

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/1563628114.png)

- Cas 单点登录配置如下：

\1. 单点域名：平台域名的代理地址；

\2. 单点登录类型选择：CAS 协议；

\3. 单点登录服务器地址：CAS 服务器地址；

\4. 单点登录地址：CAS 认证地址；

\5. 客户端地址：登录成功后的回调地址；

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/CAS%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E9%85%8D%E7%BD%AE.png)

- Cas 单点登录效果：

参见上一小节 多域名单点登录效果，此处不再赘述。

## Oauth2 单点登录集成

- Oauth2 单点登录流程：

登录时，判断二级域名对应的单点登录类型为 Oauth2 单点登录，跳转到 OAUTH2 单点登录系统，登录成功后调用回调地址并附带授权码参数，回调 api 处理逻辑中，会根据授权码获取对应 token，再根据 token 获取用户名。原理与 CAS 单点登录类似，不再赘述。

- Oauth2 单点登录配置如下：

\1. 单点域名：平台域名的代理地址；

\2. 单点登录类型选择：OAUTH2 协议；

\3. 单点登录服务器地址：OAUTH2 服务器地址；

\4. 单点登录地址：OAUTH2 认证地址；

\5. 客户端地址：登录成功后的回调地址；

6.clientId：OAUTH2 认证客户端 ID；

7.client 密码：OAUTH2 认证客户端密码；

8.Oauth2 认证用户信息：OAUTH2 根据 token 获取用户信息地址；

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/OAUTH2%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E9%85%8D%E7%BD%AE.png)

- Oauth2 单点登录效果：

\1. 在系统管理 -》域名配置中，创建单点登录配置

\2. 浏览器打开配置的域名 `http://auth.hzero.org` , 自动跳转到单点登录页面，并附带回调地址

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/OAUTH2%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E9%A1%B5.png)

\3. 单点登录页登录成功后，跳转回 `http://auth.hzero.org`

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/OAUTH2%E7%99%BB%E5%BD%95%E6%88%90%E5%8A%9F%E9%A1%B5.png)

## Azure-AD 单点登录集成

- Azure-AD 本质是 Oauth2 协议，具体配置可参照上一小节：Oauth2 单点登录集成
- 区别之处在于 Azure-AD 已提供 SDK 根据 token 查询用户信息，无需配置 Oauth2 获取用户信息地址。

## saml 单点登录集成

- saml 单点登录流程

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/saml-flow.png)

- saml 单点登录配置：

\1. 下载 hzero saml 元数据，下载地址为 oauth 服务 +/saml/metadata，例如：http://hzeronb.saas.hand-china.com/oauth/saml/metadata

\2. 将 hzero 元数据提供给 saml 身份提供方

\3. 获取 saml 身份提供方的元数据地址，配置在域名配置 ->saml 元数据地址字段中。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/saml%E9%85%8D%E7%BD%AE%E9%A1%B5.png)





### 三方登录简介

## 三方登录简介

三方登录目前 HZERO 支持 微信、QQ 三方登录，同时支持项目上开发特定的三方登录，只需按规范开发相应的实现，然后在 oauth 服务中引入依赖即可。

### 1. 组件依赖

如果想使用某个组件，需自行在 oauth 服务中引入相关依赖：

- QQ

  ```xml
  <dependency>
      <groupId>org.hzero.starter</groupId>
      <artifactId>hzero-starter-social-qq</artifactId>
      <version>${hzero.starter.version}</version>
  </dependency>
  ```

- 微信

  ```xml
  <dependency>
      <groupId>org.hzero.starter</groupId>
      <artifactId>hzero-starter-social-wechat</artifactId>
      <version>${hzero.starter.version}</version>
  </dependency>
  ```

### 2. 三方登录组件

hzero-starter-social 三方登录组件基于 spring-social、spring-security、oauth2.0 扩展开发，hzero 三方组件如下：

- hzero-starter-social-core : 三方登录核心组件，抽象了三方认证流程，及相关 API 封装
- hzero-starter-social-qq : 三方 QQ 登录
- hzero-starter-social-wechat : 三方微信登录

## 三方登录流程

Spring Social 三方登录流程是基于 oauth2.0 标准的授权码模式来完成的，所以 hzero-starter-social 组件只能在三方应用平台的授权方式是授权码模式才可以使用。具体的流程可以参考如下流程图。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/social_login.jpg)

## 三方应用管理

### 1. 申请授权信息

在使用某种三方登录方式时，首先需要到对应三方开放平台上申请三方应用的授权信息。

- QQ 开放平台 ：https://connect.qq.com/index.html
- 微信 开放平台：https://open.weixin.qq.com/cgi-bin/index
- 微博 开放平台：https://open.weibo.com/

在申请三方应用授权信息时，需要填入网站回调地址，回调地址在 oauth 服务中，且回调地址必须能让外网访问，否则三方平台无法回调。
回调地址格式为：`http://{domain}/oauth/open/{appCode}/callback`
其中 **domain** 为网站网关域名，**appCode** 为三方应用编码。

- QQ 回调地址 : `http://domain/oauth/open/qq/callback`
- 微信 回调地址 : `http://domain/oauth/open/wechat/callback`
- 微博 回调地址 : `http://domain/oauth/open/sina/callback`

申请成功后，将得到三方应用平台的 `APP ID` 以及 `APP Key`，例如 QQ 开放平台申请的应用：

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568027343.png)

### 2. 配置三方应用

首先需要在 `三方应用管理` 功能下配置系统的三方应用信息，维护好之后，才可以在个人中心三方账号及 oauth 登录页面看到三方应用的图标。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568027904.png)

- 应用编码：取自值集：HIAM.OPEN_APP_CODE，应用编码的值就是回调地址中的 appCode
- 登录渠道：取自值集：HIAM.CHANNEL，前端根据渠道查询对应渠道的三方应用，且在调用 /open/** 接口时传入渠道参数 (channel=xx)
- APP ID：申请的三方应用的授权 APP ID
- APP Key：申请的三方应用的授权 APP Key
- 应用图片：三方应用的图标

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568031410.png)

## 三方登录接口

下面以 QQ 三方登录为例介绍三方授权相关的一些接口。

### 1. 获取三方登录方式

调用 `/oauth/login/init-params?channel={channel}&client_id={clientId}` 获取三方登录方式

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568085294.png)

```
openLoginWays: 三方登录方式
isNeedCaptcha: 是否需要输入图形验证码
```

### 2. PC 端跳转三方授权平台

PC 端需跳转到三方平台让三方用户授权登录，APP 端则直接使用 SDK 拉起本地应用授权。

访问 `http://domain/oauth/open/qq?channel=pc`，后台自动跳转到 QQ 授权页面

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568087393.png)

### 3. 用户授权回调

用户授权后，三方平台将回调 `http://domain/oauth/open/qq/callback?code=XXXXX&state=xxx`，并带上授权码返回。移动端则会将授权码返回本地应用。

之后的获取 access_token、认证用户是否已绑定，都是在后端自动进行，无需特别处理。（用户如果未绑定，默认返回错误信息到登录页面，将在下个迭代中支持跳转到绑定账号页面）

### 4. 移动端三方认证接口

移动端在本地获取到三方平台的 access_token 和 open_id 之后，调用后端接口 `/oauth/token/open` 认证用户及获取 oauth access_token。

![img](http://hzerodoc.saas.hand-china.com/img/docs/service/oauth/oauth_1568090584.png)

认证成功将返回 access_token，认证失败将返回对应的失败信息。

**接口返回码**

| 返回编码                               | 说明                                                   |
| :------------------------------------- | :----------------------------------------------------- |
| hoth.social.providerUserNotFound       | 未查询到您的三方用户信息                               |
| hoth.social.openIdNotFound             | 无法获取到您的三方账号                                 |
| hoth.social.userAlreadyBind            | 您已绑定三方账户                                       |
| hoth.social.openIdAlreadyBindOtherUser | 您的三方账户已绑定其他用户，您可以先解绑再绑定当前用户 |
| hoth.social.providerNotBindUser        | 三方账号未绑定系统用户                                 |
| hoth.social.userNotFound               | 系统用户不存在                                         |

### 5. PC 端用户绑定三方账号

用户登录后，可在个人中心绑定三方账号。绑定账号访问 `http://domain/oauth/open/qq?channel=pc&access_token=xxxxx&bind_redirect_uri=redirectUrl`

```
channel: 登录渠道
access_token: 用户登录后的 access_token
bind_redirect_uri: 绑定成功或失败的重定向地址，绑定失败将在重定向地址后通过 `social_error_message` 参数返回。
```

## 开发三方登录

HZERO 目前已支持 微信、QQ 三方登录方式，如果项目上需要开发其它的三方登录，可按照如下步骤开发。三方应用平台相关的接口、参数、返回内容等请到对应三方开放平台查找。

### 1. 创建三方组件

开发三方登录时，建议新建一个项目或模块，开发完成后在 oauth 服务中依赖该组件即可。parent 依赖 `hzero-starter-social-parent`，引入 `hzero-starter-social-core` 组件。下面以 QQ 开发的流程为例讲解如何基于 hzero-starter-social-core 开发三方登录。

- QQ pom：

  ```xml
  <parent>
      <groupId>org.hzero.starter</groupId>
      <artifactId>hzero-starter-social-parent</artifactId>
      <version>1.0.0.RELEASE</version>
  </parent>
  <artifactId>hzero-starter-social-qq</artifactId>
  
  <dependencies>
      <dependency>
          <groupId>org.hzero.starter</groupId>
          <artifactId>hzero-starter-social-core</artifactId>
          <version>${hzero.starter.version}</version>
      </dependency>
  </dependencies>
  ```

### 2. 三方 API 封装

① 三方用户信息类：实现 `org.hzero.starter.social.core.common.api.SocialUser` 接口，根据三方开放平台文档，封装三方用户信息

```java
public class QQUser implements SocialUser {

    private String ret;

    private String msg;

    private String openId;

    private String nickname;

    private String figureurl;

    private String gender;

    // getter/setter...
}
```

② 三方接口类：继承 `org.hzero.starter.social.core.common.api.SocialApi` 接口，该接口类有一个默认的 getUser 接口方法，用于向三方平台获取用户信息。

```java
public interface QQApi extends SocialApi {

}
```

③ 三方接口默认实现：继承 `org.hzero.starter.social.core.common.api.AbstractSocialApi` 抽象类，并实现 `QQApi` 三方接口。

在构造方法中，必须包含 `access_token` 参数，Provider 则封装了三方平台的信息，包括 APP ID、APP Key、Token 地址、用户地址等等。
在构造方法中调用三方平台获取 open_id 的接口，根据 access_token 查询 open_id。有些三方登录在返回 access_token 时会将 open_id 直接返回，这时可以不用查询 open_id；有些则需要一次接口调用。

实现 getUser 方法，调用三方平台用户信息查询接口，根据 APP ID 及 openId 查询用户信息

```java
public class DefaultQQApi extends AbstractSocialApi implements QQApi {

    private static final Logger LOGGER = LoggerFactory.getLogger(DefaultQQApi.class);

    private String userInfoUrl;
    private String openIdUrl;

    /**
     * 客户端 appId
     */
    private String appId;
    /**
     * openId
     */
    private String openId;

    private static final ObjectMapper mapper = new ObjectMapper();

    public DefaultQQApi(String accessToken, Provider provider) {
        super(accessToken);
        // APP ID
        this.appId = provider.getAppId();
        // 获取用户信息的地址
        this.userInfoUrl = provider.getUserInfoUrl() + "?oauth_consumer_key={appId}&openid={openId}";
        // 获取 open_id 的地址
        this.openIdUrl = provider.getOpenIdUrl() + "?access_token={accessToken}";
        // 根据 access_token 获取 open_id
        this.openId = getOpenId(accessToken);
    }

    @Override
    public QQUser getUser() {
        // 获取用户信息
        String result = getRestTemplate().getForObject(userInfoUrl, String.class, appId, openId);

        QQUser user = null;
        try {
            user = mapper.readValue(result, QQUser.class);
        } catch (Exception e) {
            LOGGER.error("parse qq UserInfo error. result : {}", result);
        }
        if (user == null) {
            throw new ProviderUserNotFoundException(SocialErrorCode.PROVIDER_USER_NOT_FOUND);
        }
        user.setOpenId(openId);
        return user;
    }

    /**
     * 获取用户 OpenId
     */
    private String getOpenId(String accessToken) {
        // 返回结构：callback( {"client_id":"YOUR_APPID","openid":"YOUR_OPENID"} );
        String openIdResult = getRestTemplate().getForObject(openIdUrl, String.class, accessToken);
        if (StringUtils.isBlank(openIdResult) || openIdResult.contains("code")) {
            throw new CommonSocialException(SocialErrorCode.OPEN_ID_NOT_FOUND);
        }
        // 解析 openId
        String[] arr = StringUtils.substringBetween(openIdResult, "{", "}").replace("\"", "").split(",");
        String openid = null;
        for (String s : arr) {
            if (s.contains("openid")) {
                openid = s.split(":")[1];
            }
        }
        return openid;
    }
}
```

### 3. API 适配器

开发三方应用与本地应用用户之间的适配器，继承 `org.hzero.starter.social.core.common.connect.SocialApiAdapter` 抽象类，覆盖 `setConnectionValues` ，在方法中，首先调用 api 获取用户信息，然后向 ConnectionValues 中设置用户昵称、open_id 等。

```java
public class QQApiAdapter extends SocialApiAdapter {
    /**
     * QQApi 与 Connection 做适配
     * @param api QQApi
     * @param values Connection
     */
    @Override
    public void setConnectionValues(SocialApi api, ConnectionValues values) {
        // 调用三方接口获取用户信息
        QQUser user = (QQUser) api.getUser();
        // 设置昵称
        values.setDisplayName(user.getNickname());
        values.setImageUrl(user.getFigureurl());
        // 设置 open_id
        values.setProviderUserId(user.getOpenId());
    }
}
```

### 4. 三方服务提供商

服务提供商用于提供具体的 API，需继承 `org.hzero.starter.social.core.common.connect.SocialServiceProvider` 抽象类，在 `getSocialApi` 方法中，返回三方 API 的具体实现类。

```java
public class QQServiceProvider extends SocialServiceProvider {

    private Provider provider;

    public QQServiceProvider(Provider provider, SocialTemplate template) {
        super(provider, template);
        this.provider = provider;
    }

    @Override
    public QQApi getSocialApi(String accessToken) {
        // 构造服务提供商API
        return new DefaultQQApi(accessToken, provider);
    }
}
```

### 5. OAuth token 模板类

OAuth token 模板类用于获取三方应用 access_token，刷新 token 等等，需继承 `org.hzero.starter.social.core.common.connect.SocialTemplate` 抽象类，根据实际的 API 情况获取授权信息。

```java
public class QQTemplate extends SocialTemplate {

    private static final Logger LOGGER = LoggerFactory.getLogger(QQTemplate.class);

    public QQTemplate(Provider provider) {
        super(provider);
        // 设置带上 client_id、client_secret
        setUseParametersForClientAuthentication(true);
    }

    /**
     * 解析 QQ 返回的令牌
     */
    @Override
    protected AccessGrant postForAccessGrant(String accessTokenUrl, MultiValueMap<String, String> parameters) {
        // 返回格式：access_token=FE04********CCE2&expires_in=7776000&refresh_token=88E4***********BE14
        String result = getRestTemplate().postForObject(accessTokenUrl, parameters, String.class);
        if (StringUtils.isBlank(result)) {
            throw new RestClientException("access token endpoint returned empty result");
        }
        LOGGER.debug("==> get qq access_token: " + result);
        String[] arr = StringUtils.split(result, "&");
        String accessToken = "", expireIn = "", refreshToken = "";
        for (String s : arr) {
            if (s.contains("access_token")) {
                accessToken = s.split("=")[1];
            } else if (s.contains("expires_in")) {
                expireIn = s.split("=")[1];
            } else if (s.contains("refresh_token")) {
                refreshToken = s.split("=")[1];
            }
        }
        return createAccessGrant(accessToken, null, refreshToken, Long.valueOf(expireIn), null);
    }

    /**
     * QQ 响应 ContentType=text/html;因此需要加入 text/html; 的处理器
     */
    @Override
    protected RestTemplate createRestTemplate() {
        RestTemplate restTemplate = super.createRestTemplate();
        restTemplate.getMessageConverters().add(new StringHttpMessageConverter(Charsets.UTF_8));
        return restTemplate;
    }
}
```

### 6. 连接工厂

连接工厂用于创建 Connection 连接信息，需继承 `org.hzero.starter.social.core.common.connect.SocialConnectionFactory` 类。

```
public class QQConnectionFactory extends SocialConnectionFactory {

    public QQConnectionFactory(Provider provider, SocialServiceProvider serviceProvider, SocialApiAdapter apiAdapter) {
        super(provider, serviceProvider, apiAdapter);
    }
}
```

### 7. 连接工厂构造器

连接工厂构造器用于创建连接工厂，需实现 `org.hzero.starter.social.core.common.configurer.SocialConnectionFactoryBuilder` 接口
并实现三个方法，`getChannel` 返回登录渠道，`getProviderId` 返回应用编码，在 buildConnectionFactory 中构造连接工厂。
参数 provider 会自动根据 channel 和 providerId 查询并传入，相关授权地址需自行到三方开放平台获取。

```java
@Configuration
public class QQSocialBuilder implements SocialConnectionFactoryBuilder {

    @Override
    public String getChannel() {
        return ChannelEnum.pc.name();
    }

    @Override
    public String getProviderId() {
        return ProviderEnum.qq.name();
    }

    @Override
    public SocialConnectionFactory buildConnectionFactory(Provider provider) {
        // 获取授权码地址
        final String URL_AUTHORIZE = "https://graph.qq.com/oauth2.0/authorize";
        // 获取令牌地址
        final String URL_GET_ACCESS_TOKEN = "https://graph.qq.com/oauth2.0/token";
        // 获取 openId 的地址
        final String URL_GET_OPEN_ID = "https://graph.qq.com/oauth2.0/me";
        // 获取用户信息的地址
        final String URL_GET_USER_INFO = "https://graph.qq.com/user/get_user_info";

        provider.setAuthorizeUrl(URL_AUTHORIZE);
        provider.setAccessTokenUrl(URL_GET_ACCESS_TOKEN);
        provider.setOpenIdUrl(URL_GET_OPEN_ID);
        provider.setUserInfoUrl(URL_GET_USER_INFO);
        // 创建适配器
        QQApiAdapter apiAdapter = new QQApiAdapter();
        // 创建三方模板
        QQTemplate template = new QQTemplate(provider);
        // 创建服务提供商
        QQServiceProvider serviceProvider = new QQServiceProvider(provider, template);
        // 创建连接工厂
        return new QQConnectionFactory(provider, serviceProvider, apiAdapter);
    }
}
```

### 8. 添加配置

在 resources 资源目录下，新建 `META-INF` 目录，添加 `spring.factories` 文件，并将 QQSocialBuilder 添加到自动配置。内容如下：

```yml
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.hzero.starter.social.qq.config.QQSocialBuilder
```

### 9. API 测试

开发完成后，就可以打包发布，然后在 oauth 服务中引入依赖即可使用。

正常情况下，个人中心或登录页面，我们可以看到在三方应用管理配置的三方登录方式。

点击 QQ 图标会访问 `http://domain/oauth/open/qq?channel=pc`，接着会跳转到三方应用平台，后续的流程可参考三方登录流程图。



### 客制化开发

## 个性化开发

oauth 服务提供了很多可定制化的端点，使用者可根据实际需求个性化 oauth 认证中的一些业务逻辑，然后在客制化类上添加 `@Component` 生效

## 登录用户信息校验

`UserAccountService`： 用户账户相关，提供了用户查询、检查用户账户、检查用户租户、检查手机 / 邮箱是否已验证、检查用户角色、锁定用户、解锁用户 等接口。定制化时可自定义个性化 Service 并继承 `DefaultUserAccountService`，Override 个性化的方法即可。

## 登录用户记录

`LoginRecordService`： 登录记录相关，提供了 登录失败处理、登录成功处理、获取登录跳转地址 等接口。定制化时可自定义个性化 Service 并继承 `DefaultLoginRecordService`，Override 个性化的方法即可。

## 登录用户信息扩展

`UserDetailsWrapper`： CustomUserDetails 处理，只有一个 `wrap` 接口，用于对 CustomUserDetails 做一些包装处理，例如加入用户的当前租户，角色列表等。

`ClientDetailsWrapper`： CustomClientDetails 处理，只有一个 `wrap` 接口，用于对 CustomClientDetails 做一些包装处理，例如加入用户的当前租户，角色列表等。

## 资源匹配器

`ResourceMatcher`： 资源匹配器，通过令牌访问 oauth 服务资源时，默认只开放了 `/api/**` 接口，若要匹配其它资源，需自定义资源匹配器，并设置如下参数。

```yaml
hzero:
  oauth:
    # 认证服务器 自定义资源匹配器
    custom-resource-matcher: ${HZERO_OAUTH_CUSTOM_RESOURCE_MATCHER:true}
```

## OAuth 获取 Token 接口

`LoginTokenService`： 抽象增强了 OAuth2.0 标准的获取 access_token 的接口，开发者可根据自己的需求自定义获取 token 的接口，通过继承 `LoginTokenService` 并扩展相关认证方法即可。

`MobileLoginTokenService`： 为默认的 手机号 + 短信验证码 获取 access_token 的接口实现

`OpenLoginTokenService`： 为默认的三方登录 openId 获取 access_token 的接口实现

## 登录查询用户

`CustomUserDetailsService`：默认从数据库加载用户信息

## 登录认证器

`CustomAuthenticationProvider`：用户登录认证处理器，包含用户的获取、账号有效性校验、校验验证码、校验密码、Ldap 用户校验等，可以通过继承该类客制化相关校验。

## 登录成功处理器

`CustomAuthenticationSuccessHandler`：用户登录成功处理器，用户登录成功将记录登录日志，默认跳转到前端地址。

## 登录失败处理器

`CustomAuthenticationFailureHandler`：用户登录失败处理器，用户登录失败信息处理，返回异常信息，再次返回到登录页面。

## 退出处理器

`CustomLogoutSuccessHandler`：用户退出处理器，记录用户退出日志，重定向到退出页面。