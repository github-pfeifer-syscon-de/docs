# Postgres

[Digital ocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-master-slave-replication-on-postgresql-on-an-ubuntu-12-04-vps)

[Postgres Wiki](https://wiki.postgresql.org/wiki/Streaming_Replication)

[Postgres Doc](http://www.postgresql.org/docs/current/static/warm-standby.html)

[Syntax 9.4](http://www.postgresql.org/docs/9.4/static/sql-commands.html)

## Basics

Startup by default 

```
update-rc.d postgres defaults
```

User

```
 create user rpf PASSWORD 'pass'
 CREATE SCHEMA rpf AUTHORIZATION rpf;
 ALTER USER rpf SET search_path=rpf;
```

Connection from remote, edit /etc/postgres/9.4/main/postgres.conf

```
 listen_addresses = '*'
```

And pg_hba.conf add line for network addresses like localhost.

To allow login with password add pg_hba.conf:

```
local   all             all                                     password
```

## Master-Slave replication

```
sudo apt-get install postgresql ssh-server rsync
```

copy VM master to slave

### Config for master

enable ssh access master to slave without password

```
su - root
su - postgres
psql 
CREATE USER rep REPLICATION LOGIN CONNECTION LIMIT 1 ENCRYPTED PASSWORD 'pass';
```

As root

```
cd /etc/postgresql/9.4/main
```

Edit pg_hba.conf

```
host    replication     rep     IP_address_of_slave/32   md5
```

Edit postgresql.conf

```
listen_addresses = 'localhost,IP_address_of_THIS_host'
wal_level = 'hot_standby'
wal_keep_segments = 32
archive_mode = on
archive_command = 'scp %p "IP_SLAVE:/var/lib/postgres/9.4/main/master_xlog/%f"'
max_wal_senders = 1
hot_standby = on
```

Restart the master server to implement your changes:

```
service postgresql restart
```

### Config for slave

```
service postgresql stop
```

Create dir:

```
/var/lib/postgres/9.4/main/master_xlog/
```

Edit pg_hba.conf

```
host    replication     rep     IP_address_of_master/32  md5
```

Edit postgresql.conf

```
listen_addresses = 'localhost,IP_address_of_THIS_host'
wal_level = 'hot_standby'
archive_mode = on
archive_command = 'cd .'
max_wal_senders = 1
hot_standby = on
```

### Inital database transfer

On master (not required to stop database !):

```
psql -c "select pg_start_backup('initial_backup');"
rsync -cva --inplace --exclude=*pg_xlog* /var/lib/postgresql/9.4/main/ slave_IP_address:/var/lib/postgresql/9.4/main/
psql -c "select pg_stop_backup();"
```

On slave:

```
cd /var/lib/postgresql/9.4/main
```

Edit recovery.conf:

```
standby_mode = 'on'
primary_conninfo = 'host=master_IP_address port=5432 user=rep password=yourpassword'
trigger_file = '/tmp/postgresql.trigger.5432'
restore_command = 'cp /var/lib/postgresql/9.4/main/master_xlog/%f "%p"'
```

Start service:

```
service postgresql start
```

Watch log.
