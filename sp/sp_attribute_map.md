# 属性过滤器

修改属性过滤器配置文件，```/etc/attribute-map.xml```

```
  <Attribute name="urn:mace:dir:attribute-def:uid" id="uid" />
  <Attribute name="urn:mace:dir:attribute-def:email" id="email" />
  <Attribute name="urn:mace:dir:attribute-def:cn" id="cn" />
  <Attribute name="urn:mace:dir:attribute-def:domainName" id="domainName" />
  <Attribute name="urn:mace:dir:attribute-def:inetUserStatus" id="inetUserStatus" />
  <Attribute name="urn:mace:dir:attribute-def:typeof" id="typeOf" />
  <Attribute name="urn:mace:dir:attribute-def:departname" id="departname" />
  <Attribute name="urn:mace:dir:attribute-def:eduPersonStudentID" id="eduPersonStudentID" />
  <Attribute name="urn:mace:dir:attribute-def:telephoneNumber" id="telephoneNumber" />
  <Attribute name="urn:mace:dir:attribute-def:eduPersonAffiliation" id="eduPersonAffiliation" />
  <Attribute name="urn:oid:1.3.6.1.4.1.5923.1.1.1.1" id="eduPersonAffiliation" />
  <Attribute name="urn:oid:2.5.4.20" id="telephoneNumber" />
  <Attribute name="urn:oid:2.16.756.1.2.5.1.1.11" id="eduPersonStudentID" />
  <Attribute name="urn:oid:2.5.4.2" id="uid" />
  <Attribute name="urn:oid:2.5.4.3" id="cn" />
  <Attribute name="urn:oid:2.5.4.5" id="domainName" />
  <Attribute name="urn:oid:0.9.2342.19200300.100.1.3" id="email" />
  <Attribute name="urn:oid:2.5.4.100.1" id="inetUserStatus" />
  <Attribute name="urn:oid:2.5.4.100.3" id="departname" />
  <Attribute name="urn:oid:2.5.4.100.2" id="typeOf" />
```
这里定义了 shibboleth 接受的属性，需要符合区域认证联盟内的属性标准规范。