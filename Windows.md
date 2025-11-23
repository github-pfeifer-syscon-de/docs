# Windows

## Clean install with upgrade media

```
HKEY_LOCAL_MACHINE/Software/Microsoft/Windows/CurrentVersion/Setup/OOBE/
```

```
MediaBootInstall from "1" to "0"
```

Command as admin:

```
slmgr /rearm
```

## Clone a Window partition

Boot into recovery (Shift Restart)
```
eg: dism /capture-image /imagefile:D:\backup.wim /capturedir:C:\\ /name:"WinBackup" /compress:max /checkintegrity
```

Create EFI Partition :
```
cre par efi size=512
format fs=fat32 quick label="System Boot"
assign letter S
```

Back to Gui, apply backup file :

```
dism /apply-image /imagefile:backup.wim /index:1 /applydir:W:\\
```

To populate the system partition
```
bcdboot w:\\windows /f uefi /s s:\\
```

## Migrate mbr to Gpt

At max 3 partitions, free space at least 300M

```
mbr2gpt
```