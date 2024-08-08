# Postfixadmin

```
apt-get install postfixadmin php5-pgsql postfix postfix-pgsql 
```

Create postfixadmin user with password by setup (Debian does the most work).

For a compatible layout change in /etc/postfixadmin/config.inc.php:

```
$CONF['domain_path'] = 'YES';
$CONF['domain_in_mailbox'] = 'NO';
```

Working auth:

```
$CONF['encrypt'] = 'md5crypt';
$CONF['authlib_default_flavor'] = 'md5raw';
```

Adapt /etc/mod-availiable/postfixamdin to apache virtual host remove it from mods-available:

Invoke:

```
localhost/postfixadmin/setup.php
```


## Postfix setup

[postfixadmin guide](http://codepoets.co.uk/2009/postfixadmin-setupinstall-guide-for-virtual-mail-users-on-postfix/)

Create /etc/postfix/pgsql/relay_domain.cf

```
user = postfixadmin
password = postfixadmin
hosts = localhost
dbname = postfixadmin
query = SELECT domain FROM domain WHERE domain='%s' and backupmx = true
```

Create virtual_domains_maps.cf 

```
user = postfixadmin
password = postfixadmin
hosts = localhost
dbname = postfixadmin
query = SELECT domain FROM domain WHERE domain='%s' and backupmx = false and active = true
```

Create virtual_mailbox_limits.cf

```
user = postfixadmin
password = postfixadmin
hosts = localhost
dbname = postfixadmin
query = SELECT quota FROM mailbox WHERE username='%s'
```

Create virtual_mailbox_maps.cf 

```
user = postfixadmin
password = postfixadmin
hosts = localhost
dbname = postfixadmin
query = SELECT maildir FROM mailbox WHERE username='%s' AND active = true
```

Create virtual_alias_maps.cf 

```
user = postfixadmin     
password = postfixadmin 
hosts = localhost
dbname = postfixadmin
query = SELECT goto FROM alias WHERE address='%s' AND active = true
        UNION SELECT goto FROM alias a 
         JOIN alias_domain ad 
           ON target_domain = domain 
             AND trim(trailing domain from address)||alias_domain = '%s' 
             AND ad.active = true 
         WHERE a.active = true
```


Simple query seems not complete:

```
SELECT goto FROM alias WHERE address='%s' AND active = true
```

Modifiy /etc/postfix/main.cf

```
relay_domains = proxy:pgsql:/etc/postfix/pgsql/relay_domains.cf
virtual_alias_maps = proxy:pgsql:/etc/postfix/pgsql/virtual_alias_maps.cf
#virtual_alias_domains = proxy:pgsql:/etc/postfix/pgsql/virtual_domains_alias.cf
virtual_mailbox_domains = proxy:pgsql:/etc/postfix/pgsql/virtual_domains_maps.cf
virtual_mailbox_maps = proxy:pgsql:/etc/postfix/pgsql/virtual_mailbox_maps.cf
virtual_mailbox_base = /var/mail/vmail
virtual_mailbox_limit = 512000000
virtual_minimum_uid = 8
virtual_transport = virtual
virtual_uid_maps = static:8
virtual_gid_maps = static:8
local_transport = virtual
local_recipient_maps = $virtual_mailbox_maps
# this is only needed if you want vacation support -
transport_maps = hash:/etc/postfix/transport
```


### Spam protection

smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination

### Mailbox format

Postfix changes it format by altering the final slash in directory 

Dovecot changes the format by prefixing the directory by maildir: / mbox:

But still there seem to more missunderstandings.

# Vacation support

[Blog](http://craigballinger.com/blog/2009/08/postfix-vacation-autoresponder/)

In {{{/etc/apt/source.list}}} add non-free as source

```
apt-get update
```

```
apt-get install libmail-sender-perl libemail-valid-perl libmime-perl  liblog-log4perl-perl liblog-dispatch-perl libgetopt-argvfile-perl libmime-charset-perl libmime-encwords-perl libdbd-pg-perl
```

Then the above config works.




# Imap/Pop Access

## Dovecot

```
apt-get install dovecot-common dovecot-pop3d dovecot-imapd dovecot-pgsql
```

[Dovecot](http://wiki.nefarius.at/linux/der_perfekte_mail-server)

Edit these lines in /etc/dovecot/conf.d/10.auth.conf

```
!include auth-system.conf.ext
!include auth-sql.conf.ext
```

To allow virtual mail user change /etc/dovecot/10.mail.conf : 

```
first_valid_uid = 8
first_valid_gid = 8
```


Edit /etc/dovecot/dovecot-sql.conf.ext

```
driver=pgsql
connect = host=localhost dbname=postfixadmin user=postfixadmin password=postfixadmin
default_pass_scheme = MD5
password_query = SELECT password FROM mailbox WHERE username = '%u' AND active = true
user_query = SELECT CONCAT('maildir:/var/mail/vmail/',maildir) AS mail, '8' AS uid, '8' AS gid FROM mailbox WHERE username = '%u' AND active = true
```

## Courier setup

```
apt-get install courier-imap courier-imap-ssl courier-pop courier-pop-ssl courier-authlib-postgresql 
```

Modify /etc/courier/authdaemonrc

```
authmodulelist="authpgsql"
```

```
PGSQL_HOST        localhost
PGSQL_PORT        5432
PGSQL_USERNAME        postfixadmin
PGSQL_PASSWORD        postfixadmin
PGSQL_DATABASE         postfixadmin
PGSQL_USER_TABLE    mailbox
PGSQL_CRYPT_PWFIELD    password
PGSQL_UID_FIELD        ‘8’
PGSQL_GID_FIELD        ‘8’
PGSQL_LOGIN_FIELD    username
PGSQL_HOME_FIELD    ‘/var/mail/vmail’
PGSQL_NAME_FIELD    name
PGSQL_MAILDIR_FIELD    maildir
PGSQL_QUOTA_FIELD    quota
```

[Squirel mail](https://www.linode.com/docs/email/clients/installing-squirrelmail-on-debian-7)

# Webmail

## Squirelmail

Simple but effective 

## Roundcube

### The simple (Debian 7) way

```
apt-get install roundcube php-mdb2-driver-pgsql php5-mcrypt php5-intl
```

```
cp /etc/roundcube/apache.conf /etc/apache2/sites-available/010-roundcube.conf
```

### The hard (Debian 8) way

Download the newest roundcube release and unapck to /var/lib/roundcube

Stolen dependcies for legacy roundcube package:

```
apt-get install  libjs-jquery  libjs-jquery-ui  libmagic1  php-auth  php-mail-mime  php-net-smtp   php-net-socket php5-intl  php5-json  php5-mcrypt  tinymce  php5-gd  php5-pspell  php-mail-mimedecode
```

Pull package from channel:

```
pear -c pear.php.net install Net_IDNA2-0.1.1 
```

Edit /etc/apache2/sites-available/010-roundcube.conf and link to sites-enabled.


```
Alias /roundcube /var/lib/roundcube
<Directory "/usr/share/tinymce/www/">
     Options Indexes MultiViews FollowSymLinks
     AllowOverride None
     <IfVersion >= 2.3>
       Require all granted
     </IfVersion>
     <IfVersion < 2.3>
       Order allow,deny
       Allow from all
     </IfVersion>
</Directory>
<Directory /var/lib/roundcube/>
 Options +FollowSymLinks
 # This is needed to parse /var/lib/roundcube/.htaccess. See its
 # content before setting AllowOverride to None.
 AllowOverride All
 <IfVersion >= 2.3>
   Require all granted
 </IfVersion>
 <IfVersion < 2.3>
   Order allow,deny
   Allow from all
 </IfVersion>
</Directory>
<Directory /var/lib/roundcube/config>
       Options -FollowSymLinks
       AllowOverride None
</Directory>
<Directory /var/lib/roundcube/temp>
       Options -FollowSymLinks
       AllowOverride None
       <IfVersion >= 2.3>
         Require all denied
       </IfVersion>
       <IfVersion < 2.3>
         Order allow,deny
         Deny from all
       </IfVersion>
</Directory>
<Directory /var/lib/roundcube/logs>
       Options -FollowSymLinks
       AllowOverride None
       <IfVersion >= 2.3>
         Require all denied
       </IfVersion>
       <IfVersion < 2.3>
         Order allow,deny
         Deny from all
       </IfVersion>
</Directory>
```

Setup postgresql Database :

```
createuser -P roundcube
createdb -O roundcube -E UNICODE roundcube
psql -U roundcube -f SQL/postgres.initial.sql roundcube
```


Enable Apache modules:

```
ln -s ../mods-available/expires.load
ln -s ../mods-available/rewrite.load 
ln -s ../mods-available/deflate.load
ln -s ../mods-available/headers.load
```

Invoke installer:

```
 localhost/roundcoube/installer
```

To make send work leave the smtp_server field blank.

Remove the recommended directories.

### Use composer ???

!!!! do not use in prod !!!!

Composer install conflicts with installed php packages 

```
curl -sS https://getcomposer.org/installer | php
```

Rename composer.json-dist to composer.json

```
php /root/composer/composer.phar install --no-dev
