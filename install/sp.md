# SP 安装

### 环境
| 版本 | 备注
|------|----
| Centos6.7 x64  | 因为官方只有 yum 源，因此尽量用 CentOS 或 OpenSUSE
| apache  | 需要 mod_ssl 或 mod_nss 以开启 https


### 安装
#### 安装 shibboleth
在 http://download.opensuse.org/repositories/security://shibboleth/ 下载添加 yum repo

然后 ```yum install shibboleth``` 即可

#### 安装 apache
```yum install httpd mod_ssl```

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

### 服务
启动服务
```service shibd start```
```service httpd start```

由于 Shibboleth 服务对时间敏感，因此需要配置 ntp，不再赘述
```service ntpd start```

