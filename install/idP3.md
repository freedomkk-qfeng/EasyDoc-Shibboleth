# idP 3 安装

## 环境需求

- Oracle Jave或者OpenJDK 7或8，需下载对应的the Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 文件。虽然JRE可能可以使用，但是本版本只支持JDK。
- 需要支持Servlet API 3.0 的Servlet容器
	- Tomcat7+
	- Jetty8+
- 正式支持的版本 Tomcat 8.x和Jetty 9.2.x，尽管上文有提及Tomcat7等老版本，但并没有对以上版本进行测试，也未在案例中提及。
- 由于核心技术团队是以Jetty平台进行开发测试的，因此官方推荐使用Jetty 9 容器，笔者个人依然倾向于使用更为熟悉的 Tomcat8。
- 无操作系统限制，但推荐使用Linux, OS X 和 Windows。

#### 特别注意

- Ret hat/CentOS 一些老的版本里，默认使用的 GNU Java 编译的VM(gcj)。这是不支持的，请务必重新安装其他的JVM（比如 Oracle jdk）
- 关于 openjdk，我们还是建议使用 Oracle 官方的标准 jdk。openjdk 有时候会有些意料之外的问题，比如内存泄漏，比如上面 Debian 的问题。你也可以猜到到对任何莫名其妙问题的解释都将指向在在Oracle的JVM上重新部署。

#### 不支持的版本
idP V3.0不支持以下Idp的老版本配置：

- Java6或更早的版本.
- 不支持Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files.
- Tomcat6或更早版本。请注意如系统已安装了RHEL5或RHEL6（RHEL5预装的是Tomcat5，RHEL6预装的是Tomcat6），在安装Idp 3.0时，则需要先安装RHEL7的一个可选应用容器，这样才能可以使用Tomcat7（因此我们推荐直接使用Tomcat8）.
- Jetty7或更早的版本.

## 安装

#### 准备工作
- 一张 SSL 证书以开启 idP 的 https 访问
- 用于表示你 idP 的 entityID，安装程序默认会使用你的 hostname。
- 你的 DNS 子域，用于追加生成 "scope" 属性，比如根据你的用户名自动生成邮件地址（在用户名后追加@xxx.edu.cn，说老实话我觉得这没什么卵用）
- 一个 metadata 的获取源，用于和互信的 sp 进行通讯。通常你可以向联盟的管理方去咨询这个，或者你也可以手动创建自己的 metadata。

如果没有 metadata 的话，你可以在 [Testshib](http://www.testshib.org/) 上测试你的 idP。

安装程序会为你建议或生成一些信息：
- idP 的 entityID
- 一对自签发的密钥和证书，用于：
	- 消息传输的验证
	- 默认的 https 通讯加密，使用8443端口
	- 其他系统和 idP 的信息加密和解密
- idP 自己加密 cookies 和其他数据的 密钥和密钥版本文件。（这是 java keystore 的格式 "JCEKS")
- 基于这些信息产生的 idP 的默认配置

#### linux 下安装

Shibboleth idp 是一个标准的 Java web application，基于 Servlet 3.0 规范。你可以在所有兼容此规范的 Servlet 容器上运行 idP 。官方支持的版本是 Tomcat 和 Jetty，官方建议使用 Jetty，而我更喜欢 Tomcat。

1. 下载 [idP](http://shibboleth.net/downloads/identity-provider/latest) 最新的版本
2. 解压你下载的文件，例如 : 
	```
	unzip shibboleth-identityprovider-VERSION-bin.zip
	```
3. 进入解压的目录，例如：
	```
	cd shibboleth-identityprovider-VERSION
	```
4. 运行 ```./install.sh``` (linux) 或者 ```./install.bat``` (windows)
	- idP 的安装目录，本文中以 idp.home代替
5. 部署 idP 的 warfile。路径在  ```idp.home/war/idp.war```。后文会专门说明如何部署

##### Rebuild idP warfile
如果你要重新编译 idp warfie，运行 ```bin/build.sh``` 
```bash
opt/shibboleth-idp# bin/build.sh
```

#### Apache Tomcat 配置

