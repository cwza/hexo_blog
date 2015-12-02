title: Windows環境下LDAP安裝筆記
categories: [computer science]
tags: [ldap]
date: 2015-12-02 19:41:44
---

## Something
[Reference](http://blog.xuite.net/tolarku/blog/151029105-LDAP+%E5%9F%BA%E7%A4%8E%E8%AA%AA%E6%98%8E)
DN(Distinguished Name)：識別名稱，絕對位置
RDN(Relative Distinguished Name)：相對識別名稱，相對位置
CN(Common Name) ：名稱 (SN:姓)
OU(Organizational Unit Name)：組織名稱
O (Organizational Unit )：組織
DC(Domain Componet)：網域元件
<!-- more -->

## Install openldap windows
* download http://sourceforge.net/projects/openldapwindows/files/
* install openldap-2.4.38-x86.exe
* run sbin\slappasswd.exe, replace rootpw in slapd.conf file
* run libexec\StartLDAP.cmd
* run bin\ldapadd.exe -v -x -D "cn=Manager,dc=my-domain,dc=com" -f ..\etc\ldif\base.ldif -W
* run bin\ldapadd.exe -v -x -D "cn=Manager,dc=my-domain,dc=com" -f ..\etc\ldif\users.ldif -W

## Ldap client 
 * [LDAP admin tool](http://www.ldapadmin.org/)

## Config
 * OpenLDAP\etc\openldap\slapd.conf(rootdn, password, include schema)
 * OpenLDAP\etc\openldap\schema(schema放在這)
 
## clear Ldap DB
 * just remove all files at /var/openldap-data
