# Bugzilla

(http://www.thegeekstuff.com/2010/05/install-bugzilla-on-linux/)

```
cd ~
wget http://ftp.mozilla.org/pub/mozilla.org/webtools/bugzilla-4.2.15.tar.gz
```

```
cd /var/www/html/
tar xzf ~/bugzilla-4.2.15.tar.gz
```

```
cd bugzilla-4.2.15/
./checksetup.pl --check-modules
```

```
Add required modules:
```

```
apt-get install libdatetime-perl
apt-get install libtemplate-perl
apt-get install libemail-mime-perl
apt-get install libmath-random-isaac-perl
```

```
apt-get install libapache2-mod-perl2
```

```
apt-get install gcc make
```

```
/usr/bin/perl install-module.pl Email::Send
```

```
./checksetup.pl
