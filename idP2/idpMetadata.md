# idP 的注册

Shibboleth 框架内， idP 和 SP 需要通过 metadata 来互相取得信任关系。对于一个较大的跨域认证联盟而言，通常由统一的管理机构来统一管理 idP 和 SP 的 metadata。因此每一个 idP 或 SP，只需要将其 metadata 提交给管理机构注册后，即和其域内完成互信。

#### Metadata

在 idp 安装完以后，可以修改  ```IDP_HOME/metadata/idp-metadata.xml``` 文件

确保其各 location 的地址，和entityID 的名字与 relying_party.xml 的配置一致。



