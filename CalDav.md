# calDav

## Sardine

For accessing webDev resources:

https://github.com/lookfirst/sardine


For interpreting .ics files:

http://sourceforge.net/projects/ical4j/files/

## DaviCal

Alternative config with password changeable (after db setup):

```
$c->pg_connect[] = array( 'dsn' => 'pgsql:dbname=davical ','dbuser'=>'davical_app' ,'dbpass'=>'xxxx');
```

```
alter user davical_app password 'xxxx'
alter user davical_dba password 'xxxx'
```

Replace dedicated "trusted" with "password" authentication in "pg_hba.conf".

## sabreDav

Lightweight solution: http://sabre.io/dav

### Base setup
```
mkdir dav
cd dav
composer require sabre/dav 
mkdir data/
cat vendor/sabre/dav/examples/sql/sqlite.* | sqlite3 data/db.sqlite
chmod -Rv a+rw data/
cp vendor/sabre/dav/examples/groupwareserver.php .
vi: "$baseUri = '/~rpf/dav/groupwareserver.php';"
```

### Adding user
To get hash use:
```
php -r "echo md5('rpf:SabreDAV:rpf');"
```

Fill user info tables:
```
sqlite3 data/db.sqlite 
insert into principals (uri,email,displayname) values ('principals/rpf','rpf@pfeifer-syscon.de','Roland Pfeifer');
insert into users (username,digesta1) values ('rpf','7c1e2c86ebc04c5736f411a7a6dbbb90');
insert into principals (uri) values ('principals/rpf/calendar-proxy-read');
insert into principals (uri) values ('principals/rpf/calendar-proxy-write');
```

### Create calendar
```
* Browse admin/calendars
* Create calendar rpf
* Assign to user : update calendarinstances set principaluri = 'principals/rpf' where id = 1;
```

### Access Thunderbird/lightening
```
Add url (and dont reuse a previously existing calendar name ... tbis picky about that):
* http://localhost/~rpf/dav/groupwareserver.php/calendars/rpf/rpf/
