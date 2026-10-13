# Btrfs

Systemd and btrfs are far from being best friends as it seems...

The journald logs are written in small pieces and the copy on write of btrfs
will create many fragment.
Check use:

```
find /var/log/journald -xdev -type f| xargs filefrag 
```
The result is while booting flushing journals to persisten storage will exceed the time limit...
Reducing the size for the logs will not improve the situation...
```
SystemMaxUse=256M
RuntimeMaxUse=128M
MaxFileSec=1week
```

(But in /etc/system/journald.conf the journal is persistent anyway (which is the default), why this extra handling?
,is it like logrotate?, greate improvement Mr Poettering)

The suggestion of https://forum.manjaro.org/t/btrfs-flush-journal-to-persistent-storage-slow-boot-issue-solution/84958
```
btrfs file defrag -vrft 40M /var/log/journal
```
does not reduce the fragmentation.
Additional 
```
chattr -R +C /var/log/journal
```
was add in startup script, but the files will not catch the attribute while their size is > 0.
So maybe check after some time...
