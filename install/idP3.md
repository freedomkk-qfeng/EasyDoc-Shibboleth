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


