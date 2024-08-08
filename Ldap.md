# LDAP Setup

[Linix Ldap](http://www.yolinux.com/TUTORIALS/LinuxTutorialLDAP.html) which is not to much organized

[Ldap example](http://www.sudleyplace.com/LDAP/index.en.html) Some hieracycal features

```
apt-get install slapd ldap-utils jxplorer
```

```
dpkg-reconfigure slapd
```


/etc/ldap/ldap.conf

```
#
# LDAP Defaults
#
# See ldap.conf(5) for details
# This file should be world readable but not world writable.
BASE	dc=example,dc=com
URI	ldap://ldap.example.com
#SIZELIMIT	12
#TIMELIMIT	15
#DEREF		never
# TLS certificates (needed for GnuTLS)
TLS_CACERT	/etc/ssl/certs/ca-certificates.crt
```

Test conifg:

```
ldapsearch -x
```

May check content with adding adress entry to thunderbird ???

Define a country to collect entries:

```
dn: c=de,dc=example,dc=com
c: de
objectClass: top
objectClass: country
```

Add a country entry:

```
ldapadd -v -D "cn=admin,dc=example,dc=com" -Z -W -f de.ldif
```

Create a simple entry:

```
dn: cn=Roland Pfeifer,c=de,dc=example,dc=com
cn: Roland Pfeifer
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
mail: rpf@pfeifer-syscon.de
givenname: Roland
sn: Pfeifer          
street: Hungener Str. 57b
l: Lich
st: HE
postalCode: 35423
telephoneNumber: +49 6404 950200
homephone: +49 171 11 53 23 0
```

Add a simple entry:

```
ldapadd -v -D "cn=admin,dc=example,dc=com" -Z -W -f rpf.ldif
```

Create a modify file:

```
dn: cn=Roland Pfeifer,dc=example,dc=com
changetype: modify
replace: sn
sn: Pfeifer
```

Modify the simple entry:

```
ldapmodify -v -D "cn=admin,dc=example,dc=com"  -Z -W -f modify_rpf.ldif
```

# Apache Auth

```
apt-get install apache2 libapache2-mod-webauthldap
```

```
rc.update apache2 default
```

```
a2enmod webauthldap
a2enmod webauth
```


Add to /etc/apache2/mods-enabled/webauthldap.conf

```
WebAuthLdapHost localhost
WebAuthLdapBase c=de,dc=example,dc=com
WebAuthLdapAuthorizationAttribute objectClass
WebAuthLdapDebug on
```

Added to /etc/apache2/apache2.conf

```
<Directory /var/www/html/sub>
  AuthType WebAuthLdap
  AuthName "Example Web Site: Login with email address or display name"
  require valid-user
</Directory>
