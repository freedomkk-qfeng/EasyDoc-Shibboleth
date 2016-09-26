# 使用 sp

### 使用 Shibboleth-sp
#### 认证

修改 ```/etc/httpd/conf.d/shib.conf``` 文件
这表示被 Shibboleth 保护的路径，访问此路径将会进行跨校认证。
```
<Location /secure>
  AuthType shibboleth
  ShibCompatWith24 On
  ShibRequestSetting requireSession 1
  require shib-session
</Location>
<Location /test>
  AuthType shibboleth
  ShibCompatWith24 On
  ShibRequestSetting requireSession 1
  require shib-session
</Location>
```

如上所示，我们只要配置了被 shibboleth 保护的路径，当访问这些路径时，就需要先通过跨校认证。

#### 属性
当跨校认证完成时，用户的相关信息会带在 http header 内。

以 php 为例
``` $uid=$_SERVER["uid"] ```

即可获取用户的用户名，即 uid

#### 登出
使用此链接登出跨校认证
https://sp.example.com/Shibboleth.sso/Logout
注意这并不会消除 idp 上 session。因此如果一个用户之前使用某 idp 认证之后，在 sp 上登出联邦认证后，再此从这个 sp 上发起联邦认证时，如果他还是选择同一个 idp，不需要重新输入用户名和密码，将直接跳回至 sp。
要支持 SLO 需要 idp3 以上的版本

#### Java Servlets
shibboleth 没法直接支持 Java Servlets，但是可以通过 AJP 来间接支持。
将 apache 上配置的 Shibboleth 受保护路径，设置 ajp 代理 java servlet 。
注意此时需要在 ``` shibboleth2.xml  ``` 里的 ```ApplicationDefaults``` 配置中增加 ``` attributePrefix="AJP_"```
```

<ApplicationDefaults id="default" policyId="default"
    entityID="https://sp.example.org/shibboleth"
    REMOTE_USER="eppn persistent-id targeted-id"
    signing="false" encryption="false"
    attributePrefix="AJP_">
```
详见
https://wiki.shibboleth.net/confluence/display/SHIB2/NativeSPJavaInstall

#### 接口模式
对于非 apache 的应用，更常见的做法是用户先跨校认证到 sp 的 apache 上。然后再将相应的用户信息转发给具体的应用，中间可以通过一些接口规定来保障安全。

一个参考的接口示例如下：

请求地址，作为应用发起跨校认证的入口

https://sp.example.cn/test/?redirect_uri=http://www.163.com

|参数|参数说明|
-----|----|
|redirect_uri|回调的地址|

经过跨校认证

返回到此接口后，重定向至 redirect_uri 并携带属性。
http://www.163.com/?uid=MjAxNTAwNzM=&cn=5Yav6aqQ&domainName=ZWNudS5lZHUuY24=&msg=296c6e5ed46b016881c192315ab12215&date=1473829135

|参数|参数说明|
-----|----|
|uid|用户名，base64封装|
|cn|姓名，base64封装|
|domainName|来源域，base64封装|
|msg|校验码，通过字符串连接时间戳计算校验码，判断来源请求是否伪造|
|date|时间戳，用于计算校验码，也可用于判断请求时间是否有效|