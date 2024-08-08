# Arch-Linux

Some bits and pieces to get Arch-Linux up and running.

Dont get me wrong the documention is excelent, but also quite verbose, so i write down these as a helper for my personal tweaks.

And here you see how serious they support you in case of .... :
[Arch support form](http://ubuntuweblogs.org/1893/high-quality-linux-wallpapers/)

## /etc/vconsole.conf

```
KEYMAP=de-latin1
#FONT=lat9u-16
FONT=Lat2-Termins16
FONT_MAP=8859-1
```

## Network

```
ip link                    ; List interfaces in my case enp4s0
cp /etc/netctl/examples/ethernet-dhcp /etc/netctl enp4s0
netctl start enp4s0
ip link show dev enp4s0    ; Check interface is up and working also ping ...
netctl enable enp4s0       ; make changes permanent
```

## User

Groups : log,audio,video,wheel


## X-Org

```
pacman -S xf86-video-vesa xf86-video-nouveau xorg-server xorg-xinit lxde ttf-dejavu xorg-fonts-100dpi
```



```
Section "InputClass"
	Identifier	"system-keyboard"
	MatchIsKeyboard	"on"
	Option	"XkbLayout"	"de"
	Option	"XkbModel"	"pc104"
EndSection
```

Use .xinitrc:

```
#!/bin/sh
userresources=$HOME/.Xresources
usermodmap=$HOME/.Xmodmap
sysresources=/etc/X11/xinit/.Xresources
sysmodmap=/etc/X11/xinit/.Xmodmap
# merge in defaults and keymaps
if [ -f $sysresources ]; then
   xrdb -merge $sysresources
fi
if [ -f $sysmodmap ]; then
   xmodmap $sysmodmap
fi
if [ -f "$userresources" ]; then
   xrdb -merge "$userresources"
fi
if [ -f "$usermodmap" ]; then
   xmodmap "$usermodmap"
fi
# start some nice programs
if [ -d /etc/X11/xinit/xinitrc.d ] ; then
f or f in /etc/X11/xinit/xinitrc.d/?*.sh ; do
  [ -x "$f" ] && . "$f"
 done
 unset f
fi
#exec lxterminal
#exec /usr/bin/startlxde
exec startxfce4
#export XDG_CURRENT_DESKTOP=GNOME-Classic:GNOME
#export GNOME_SHELL_SESSION_MODE=classic
#exec gnome-session 
#exec gnome-session --session=gnome-classic
#exec startkde
#exec openbox-kde-session
```


```
pacman -S xfce4-taskmanager    
```


Some nice look&feels

```
pacman -S adapta-gtk-theme
pacman -S breeze-gtk
```

### User-name changed

If you change the name for a user, be careful with the saved xfce session (this incorporates the absolute path). Even removing {{{~/.config/xfce4}}} and {{{~/.cache/xfce4}}} wont fix the blank screen on starting. As a workaround create a symlink from to old to the new username and remove the session with xfce-setting startup.

## Useful tools

```
pacman -S jedit firefox thunderbird
```



### Bash

For some shell color modify /etc/bash.bashrc:

```

#
# /etc/bash.bashrc
#

# If not running interactively, don't do anything
[[ $- != *i* ]] && return

[[ $DISPLAY ]] && shopt -s checkwinsize

#PS1='[\u@\h \W]\$ '
PS1=''

case ${TERM} in
  xterm*|rxvt*|Eterm|aterm|kterm|gnome*)
    PROMPT_COMMAND=${PROMPT_COMMAND:+$PROMPT_COMMAND; }'printf "\033]0;%s@%s:%s\007" "${USER}" "${HOSTNAME%%.*}" "${PWD/#$HOME/\~}"'

    ;;
  screen*)
    PROMPT_COMMAND=${PROMPT_COMMAND:+$PROMPT_COMMAND; }'printf "\033_%s@%s:%s\033\\" "${USER}" "${HOSTNAME%%.*}" "${PWD/#$HOME/\~}"'
    ;;
esac

[ -r /usr/share/bash-completion/bash_completion   ] && . /usr/share/bash-completion/bash_completion

# Set colorful PS1 only on colorful terminals.
# dircolors --print-database uses its own built-in database
# instead of using /etc/DIR_COLORS.  Try to use the external file
# first to take advantage of user additions.
# We run dircolors directly due to its changes in file syntax and
# terminal name patching.
use_color=false
if type -P dircolors >/dev/null ; then
	# Enable colors for ls, etc.  Prefer ~/.dir_colors #64489
	LS_COLORS=
	if [[ -f ~/.dir_colors ]] ; then
		eval "$(dircolors -b ~/.dir_colors)"
	elif [[ -f /etc/DIR_COLORS ]] ; then
		eval "$(dircolors -b /etc/DIR_COLORS)"
	else
		eval "$(dircolors -b)"
	fi
	# Note: We always evaluate the LS_COLORS setting even when it's the
	# default.  If it isn't set, then `ls` will only colorize by default
	# based on file attributes and ignore extensions (even the compiled
	# in defaults of dircolors). #583814
	if [[ -n ${LS_COLORS:+set} ]] ; then
		use_color=true
	else
		# Delete it if it's empty as it's useless in that case.
		unset LS_COLORS
	fi
else
	# Some systems (e.g. BSD & embedded) don't typically come with
	# dircolors so we need to hardcode some terminals in here.
	case ${TERM} in
	[aEkx]term*|rxvt*|gnome*|konsole*|screen|cons25|*color) use_color=true;;
	esac
fi
#echo "use_color "$use_color
#echo "EUDI "$EUID
#echo "TERM "$TERM

if ${use_color} ; then
	if [[ ${EUID} == 0 ]] ; then
		PS1+='\[\033[01;31m\]\h\[\033[01;34m\] \w \$\[\033[00m\] '
	else
		PS1+='\[\033[01;32m\]\u@\h\[\033[01;34m\] \w \$\[\033[00m\] '
	fi

	#BSD#@export CLICOLOR=1
	#GNU#@alias ls='ls --color=auto'
	alias grep='grep --colour=auto'
	alias egrep='egrep --colour=auto'
	alias fgrep='fgrep --colour=auto'
else
	if [[ ${EUID} == 0 ]] ; then
		# show root@ when we don't have colors
		PS1+='\u@\h \w \$ '
	else
		PS1+='\u@\h \w \$ '
	fi
fi

for sh in /etc/bash/bashrc.d/* ; do
	[[ -r ${sh} ]] && source "${sh}"
done

#echo "PS1 "$PS1
# Try to keep environment pollution down, EPA loves us.
unset use_color sh
```


### Conky

Some desktop background infos (replaced by gtk3/monglmm):
After trying to nudge this in the right direction (as this isn't really lean for a config by hand thing) i decided to make my own solution see [Gtk3](Gtk3.md) Monglmm.

```
pacman -S conky 
```

```

-- vim: ts=4 sw=4 noet ai cindent syntax=lua
--[[
Conky, a system monitor, based on torsmo

Any original torsmo code is licensed under the BSD license

All code written since the fork of torsmo is licensed under the GPL

Please see COPYING for details

Copyright (c) 2004, Hannu Saransaari and Lauri Hakkarainen
Copyright (c) 2005-2012 Brenden Matthews, Philip Kovacs, et. al. (see AUTHORS)
All rights reserved.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
]]

conky.config = {
    alignment = 'top_right',
    background = false,
    border_width = 1,
    cpu_avg_samples = 2,
	default_color = 'white',
    default_outline_color = 'white',
    default_shade_color = 'white',
    draw_borders = false,
    draw_graph_borders = true,
    draw_outline = false,
    draw_shades = false,
    use_xft = true,
    font = 'DejaVu Sans Mono:size=8',
    gap_x = 5,
    gap_y = 60,
    minimum_height = 5,
    minimum_width = 280,
    maximum_width = 280,
    net_avg_samples = 2,
    no_buffers = true,
    out_to_console = false,
    out_to_stderr = false,
    extra_newline = false,
    own_window = true,
    own_window_class = 'Conky',
    own_window_type = 'normal',
    own_window_hints = 'undecorated,below,sticky,skip_taskbar,skip_pager',
    own_window_transparent = true,
    stippled_borders = 0,
    update_interval = 5.0,
    uppercase = false,
    use_spacer = 'none',
    show_graph_scale = false,
    show_graph_range = false,
    double_buffer = true
}

conky.text = [[
$nodename - $sysname $kernel on $machine 
$hr
${color grey}Uptime:$color $uptime 
${color grey}Now:$color ${time %d.%m.%y %T Dy:%j Wy:%V Dw:%u}
${color grey}Freq $color ${freq_g 1} ${freq_g 2} ${freq_g 3} ${freq_g 4}GHz
${color grey}Temp acpi $color ${acpitemp}°C
${color grey}Temp hwmo $color ${hwmon temp 1} ${hwmon temp 2}°C
${color grey}RAM Usage:$color $mem/$memmax - $memperc% 
  ${memgraph 32,256 00ff00 ff0000 -t}
${color grey}Swap Usage:$color $swap/$swapmax - $swapperc% 
  ${swapbar 4,256}
${color grey}CPU Usage:$color $cpu% 
  ${cpugraph 32,256 00ff00 ff0000 -t}
${color grey}Processes:$color $processes  ${color grey}Running:$color $running_processes
$hr
${color grey}Networking:
 ${color grey}Down: $color ${downspeed enp4s0} k/s ${color #606060}Up: $color ${upspeed enp4s0} k/s
 ${color grey}${downspeedgraph enp4s0 32,150 0000ec ec0000 -t} ${upspeedgraph enp4s0    32,150 0000ec ec0000 -t}
$hr
${color grey} Inbound Connection ${alignr} Local Service/Port
${color}${tcp_portmon 1 32767 rhost 0} ${alignr} ${tcp_portmon 1 32767    lservice 0}
 ${tcp_portmon 1 32767 rhost 1} ${alignr} ${tcp_portmon 1 32767 lservice 1}
 ${tcp_portmon 1 32767 rhost 2} ${alignr} ${tcp_portmon 1 32767 lservice 2}
 ${tcp_portmon 1 32767 rhost 3} ${alignr} ${tcp_portmon 1 32767 lservice 3}
$hr
${color grey} Outbound Connection ${alignr} Remote Service/Port$color
${color} ${tcp_portmon 32768 61000 rhost 0} ${alignr} ${tcp_portmon    32768 61000 rservice 0}
 ${tcp_portmon 32768 61000 rhost 1} ${alignr} ${tcp_portmon 32768 61000 rservice    1}
 ${tcp_portmon 32768 61000 rhost 2} ${alignr} ${tcp_portmon 32768 61000 rservice    2}
 ${tcp_portmon 32768 61000 rhost 3} ${alignr} ${tcp_portmon 32768 61000 rservice    3}
 ${tcp_portmon 32768 61000 rhost 4} ${alignr} ${tcp_portmon 32768 61000 rservice    4}
$hr
${color grey}File systems (Free):
 $color /     ${fs_free /} ${fs_bar 6 /}  
 $color /home ${fs_free /home}  ${fs_bar 6 /home}
$hr
${color grey}Name              PID   CPU%   MEM%   Write Read
${color} ${top name 1} ${top pid 1} ${top cpu 1} ${top mem 1} ${top io_write 1} ${top io_read 1}
 ${top name 2} ${top pid 2} ${top cpu 2} ${top mem 2} ${top io_write 2} ${top io_read 2}
 ${top name 3} ${top pid 3} ${top cpu 3} ${top mem 3} ${top io_write 3} ${top io_read 3}
 ${top name 4} ${top pid 4} ${top cpu 4} ${top mem 4} ${top io_write 4} ${top io_read 4}
]]
```

[Reference](http://conky.sourceforge.net/variables.html)

## Grub

```
pacman -S grub efibootmgr
mkfs.fat -F32 /dev/sda1             ; Format partion with EFI System type
mount /dev/sda1 /boot/efi
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=grub
```

Tweak default /etc/default/grub
```

GRUB_TIMEOUT=10 
#GRUB_CMDLINE_LINUX_DEFAULT="quiet"
GRUB_DISABLE_SUBMENU=y
GRUB_COLOR_NORMAL="light-blue/black"
GRUB_COLOR_HIGHLIGHT="light-cyan/blue"
```


```
grub-mkconfig -o /boot/grub/grub.cfg
```

## User

```
 useradd -m -G wheel,log,video,log,audio,vboxusers -s /bin/bash rpf
```

Edit sudoers file (visudo) to allow sudo (required for Aur package handling):

```
%wheel ALL=(ALL) ALL
```

## Udev

```
/etc/udev/rules.d/30-usbdev.rules 
SUBSYSTEM=="usb", ATTRS{idVendor}=="046d", ATTRS{idProduct}=="c626", ACTION=="add", GROUP="usbdev", MODE="0664"
SUBSYSTEM=="usb", ATTRS{idVendor}=="046d", ATTRS{idProduct}=="c227", ACTION=="add", GROUP="usbdev",  MODE="0664"
SUBSYSTEM=="usb", ATTRS{idVendor}=="04a9", ATTRS{idProduct}=="1906", ACTION=="add", GROUP="usbdev",  MODE="0664"

/etc/udev/rules.d/40-usbmon.rules 
SUBSYSTEM=="usbmon",  ACTION=="add", GROUP="usbmon", MODE="0664"
```


## ClamAV

```
pacman clamav
freshclam           ; Run signature update manually
```

Modfied structure edit /etc/systemd/system/clamav-daemon.socket (Overrides /lib/usr) :

```
[Unit]
Description=Socket for Clam AntiVirus userspace daemon
Documentation=man:clamd(8) man:clamd.conf(5) http://www.clamav.net/lang/en/doc/
# Check for database existence
ConditionPathExistsGlob=/var/lib/clamav/main.{c[vl]d,inc}
ConditionPathExistsGlob=/var/lib/clamav/daily.{c[vl]d,inc}
[Socket]
ListenStream=/var/lib/clamav/clamd.sock
ListenStream=127.0.0.1:3310
SocketUser=clamav
SocketGroup=clamav
RemoveOnStop=True
[Install]
WantedBy=sockets.target
```

Enable service:

```
systemctl enable --now clamav-daemon 
systemctl enable --now clamav-freshclam
```

And remember to use (On access may also be an alternative...) for downloaded packages:

```
clamscan -ri x
```

Add to .bashrc:

```
alias clamscan="clamscan -r --infected"
```

For system that may not always be online add to update:
```
systemctl kill --signal=SIGUSR1 clamav-freshclam
```


On access scan modify /etc/clamd/clamd.conf:

```
##
## Example config file for the Clam AV daemon
## Please read the clamd.conf(5) manual before editing this file.
##
# Comment or remove the line below.
#Example
# Uncomment this option to enable logging.
# LogFile must be writable for the user running daemon.
# A full path is required.
# Default: disabled
LogFile /var/log/clamav/clamd.log
# By default the log file is locked for writing - the lock protects against
# running clamd multiple times (if want to run another clamd, please
# copy the configuration file, change the LogFile variable, and run
# the daemon with --config-file option).
# This option disables log file locking.
# Default: no
#LogFileUnlock yes
# Maximum size of the log file.
# Value of 0 disables the limit.
# You may use 'M' or 'm' for megabytes (1M = 1m = 1048576 bytes)
# and 'K' or 'k' for kilobytes (1K = 1k = 1024 bytes). To specify the size
# in bytes just don't use modifiers. If LogFileMaxSize is enabled, log
# rotation (the LogRotate option) will always be enabled.
# Default: 1M
#LogFileMaxSize 2M
# Log time with each message.
# Default: no
LogTime yes
# Also log clean files. Useful in debugging but drastically increases the
# log size.
# Default: no
#LogClean yes
# Use system logger (can work together with LogFile).
# Default: no
#LogSyslog yes
# Specify the type of syslog messages - please refer to 'man syslog'
# for facility names.
# Default: LOG_LOCAL6
#LogFacility LOG_MAIL
# Enable verbose logging.
# Default: no
#LogVerbose yes
# Enable log rotation. Always enabled when LogFileMaxSize is enabled.
# Default: no
#LogRotate yes
# Log additional information about the infected file, such as its
# size and hash, together with the virus name.
#ExtendedDetectionInfo yes
# This option allows you to save a process identifier of the listening
# daemon (main thread).
# Default: disabled
PidFile /run/clamav/clamd.pid
# Optional path to the global temporary directory.
# Default: system specific (usually /tmp or /var/tmp).
TemporaryDirectory /tmp
# Path to the database directory.
# Default: hardcoded (depends on installation options)
#DatabaseDirectory /var/lib/clamav
# Only load the official signatures published by the ClamAV project.
# Default: no
#OfficialDatabaseOnly no
# The daemon can work in local mode, network mode or both. 
# Due to security reasons we recommend the local mode.
# Path to a local socket file the daemon will listen on.
# Default: disabled (must be specified by a user)
#LocalSocket /run/clamav/clamd.ctl
# for this see  /usr/lib/systemd/system/clamav-daemon.socket !
LocalSocket /var/lib/clamav/clamd.sock
# Sets the group ownership on the unix socket.
# Default: disabled (the primary group of the user running clamd)
#LocalSocketGroup virusgroup
LocalSocketGroup clamd
# Sets the permissions on the unix socket to the specified mode.
# Default: disabled (socket is world accessible)
#LocalSocketMode 660
# Remove stale socket after unclean shutdown.
# Default: yes
FixStaleSocket yes
# TCP port address.
# Default: no
# for this see  /usr/lib/systemd/system/clamav-daemon.socket !
TCPSocket 3310
# TCP address.
# By default we bind to INADDR_ANY, probably not wise.
# Enable the following to provide some degree of protection
# from the outside world. This option can be specified multiple
# times if you want to listen on multiple IPs. IPv6 is now supported.
# Default: no
# for this see  /usr/lib/systemd/system/clamav-daemon.socket !
TCPAddr 127.0.0.1
# Maximum length the queue of pending connections may grow to.
# Default: 200
#MaxConnectionQueueLength 30
# Clamd uses FTP-like protocol to receive data from remote clients.
# If you are using clamav-milter to balance load between remote clamd daemons
# on firewall servers you may need to tune the options below.
# Close the connection when the data size limit is exceeded.
# The value should match your MTA's limit for a maximum attachment size.
# Default: 25M
#StreamMaxLength 10M
# Limit port range.
# Default: 1024
#StreamMinPort 30000
# Default: 2048
#StreamMaxPort 32000
# Maximum number of threads running at the same time.
# Default: 10
#MaxThreads 20
# Waiting for data from a client socket will timeout after this time (seconds).
# Default: 120
#ReadTimeout 300
# This option specifies the time (in seconds) after which clamd should
# timeout if a client doesn't provide any initial command after connecting.
# Default: 5
#CommandReadTimeout 5
# This option specifies how long to wait (in miliseconds) if the send buffer is full.
# Keep this value low to prevent clamd hanging
#
# Default: 500
#SendBufTimeout 200
# Maximum number of queued items (including those being processed by MaxThreads threads)
# It is recommended to have this value at least twice MaxThreads if possible.
# WARNING: you shouldn't increase this too much to avoid running out  of file descriptors,
# the following condition should hold:
# MaxThreads*MaxRecursion + (MaxQueue - MaxThreads) + 6< RLIMIT_NOFILE (usual max is 1024)
#
# Default: 100
#MaxQueue 200
# Waiting for a new job will timeout after this time (seconds).
# Default: 30
#IdleTimeout 60
# Don't scan files and directories matching regex
# This directive can be used multiple times
# Default: scan all
#ExcludePath ^/proc/
#ExcludePath ^/sys/
# Maximum depth directories are scanned at.
# Default: 15
#MaxDirectoryRecursion 20
# Follow directory symlinks.
# Default: no
FollowDirectorySymlinks yes
# Follow regular file symlinks.
# Default: no
#FollowFileSymlinks yes
# Scan files and directories on other filesystems.
# Default: yes
#CrossFilesystems yes
# Perform a database check.
# Default: 600 (10 min)
#SelfCheck 600
# Execute a command when virus is found. In the command string %v will
# be replaced with the virus name.
# Default: no
#VirusEvent /usr/local/bin/send_sms 123456789 "VIRUS ALERT: %v"
# Run as another user (clamd must be started by root for this option to work)
# Default: don't drop privileges
#User clamav
User root
# Initialize supplementary group access (clamd must be started by root).
# Default: no
#AllowSupplementaryGroups no
# Stop daemon when libclamav reports out of memory condition.
#ExitOnOOM yes
# Don't fork into background.
# Default: no
#Foreground yes
# Enable debug messages in libclamav.
# Default: no
#Debug yes
# Do not remove temporary files (for debug purposes).
# Default: no
#LeaveTemporaryFiles yes
# Permit use of the ALLMATCHSCAN command. If set to no, clamd will reject
# any ALLMATCHSCAN command as invalid.
# Default: yes
#AllowAllMatchScan no
# Detect Possibly Unwanted Applications.
# Default: no
#DetectPUA yes
# Exclude a specific PUA category. This directive can be used multiple times.
# See https://github.com/vrtadmin/clamav-faq/blob/master/faq/faq-pua.md for 
# the complete list of PUA categories.
# Default: Load all categories (if DetectPUA is activated)
#ExcludePUA NetTool
#ExcludePUA PWTool
# Only include a specific PUA category. This directive can be used multiple
# times.
# Default: Load all categories (if DetectPUA is activated)
#IncludePUA Spy
#IncludePUA Scanner
#IncludePUA RAT
# In some cases (eg. complex malware, exploits in graphic files, and others),
# ClamAV uses special algorithms to provide accurate detection. This option
# controls the algorithmic detection.
# Default: yes
#AlgorithmicDetection yes
# This option causes memory or nested map scans to dump the content to disk.
# If you turn on this option, more data is written to disk and is available
# when the LeaveTemporaryFiles option is enabled.
#ForceToDisk yes
# This option allows you to disable the caching feature of the engine. By
# default, the engine will store an MD5 in a cache of any files that are
# not flagged as virus or that hit limits checks. Disabling the cache will
# have a negative performance impact on large scans.
# Default: no
#DisableCache yes
##
## Executable files
##
# PE stands for Portable Executable - it's an executable file format used
# in all 32 and 64-bit versions of Windows operating systems. This option allows
# ClamAV to perform a deeper analysis of executable files and it's also
#  required for decompression of popular executable packers such as UPX, FSG,
# and Petite. If you turn off this option, the original files will still be
# scanned, but without additional processing.
# Default: yes
#ScanPE yes
# Certain PE files contain an authenticode signature. By default, we check
# the signature chain in the PE file against a database of trusted and
# revoked certificates if the file being scanned is marked as a virus.
# If any certificate in the chain validates against any trusted root, but
# does not match any revoked certificate, the file is marked as whitelisted.
# If the file does match a revoked certificate, the file is marked as virus.
# The following setting completely turns off authenticode verification.
# Default: no
#DisableCertCheck yes
# Executable and Linking Format is a standard format for UN*X executables.
# This option allows you to control the scanning of ELF files.
# If you turn off this option, the original files will still be scanned, but
# without additional processing.
# Default: yes
#ScanELF yes
# With this option clamav will try to detect broken executables (both PE and
# ELF) and mark them as Broken.Executable.
# Default: no
#DetectBrokenExecutables yes
##
## Documents
##
# This option enables scanning of OLE2 files, such as Microsoft Office
# documents and .msi files.
# If you turn off this option, the original files will still be scanned, but
# without additional processing.
# Default: yes
#ScanOLE2 yes
# With this option enabled OLE2 files with VBA macros, which were not
# detected by signatures will be marked as "Heuristics.OLE2.ContainsMacros".
# Default: no
#OLE2BlockMacros no
# This option enables scanning within PDF files.
# If you turn off this option, the original files will still be scanned, but
# without decoding and additional processing.
# Default: yes
#ScanPDF yes
# This option enables scanning within SWF files.
# If you turn off this option, the original files will still be scanned, but
# without decoding and additional processing.
# Default: yes
#ScanSWF yes
# This option enables scanning xml-based document files supported by libclamav.
# If you turn off this option, the original files will still be scanned, but
# without additional processing.
# Default: yes
#ScanXMLDOCS yes
# This option enables scanning of HWP3 files.
# If you turn off this option, the original files will still be scanned, but
# without additional processing.
# Default: yes
#ScanHWP3 yes
##
## Mail files
##
# Enable internal e-mail scanner.
# If you turn off this option, the original files will still be scanned, but
# without parsing individual messages/attachments.
# Default: yes
#ScanMail yes
# Scan RFC1341 messages split over many emails.
# You will need to periodically clean up $TemporaryDirectory/clamav-partial directory.
# WARNING: This option may open your system to a DoS attack.
#	   Never use it on loaded servers.
# Default: no
#ScanPartialMessages yes
# With this option enabled ClamAV will try to detect phishing attempts by using
# signatures.
# Default: yes
#PhishingSignatures yes
# Scan URLs found in mails for phishing attempts using heuristics.
# Default: yes
#PhishingScanURLs yes
# Always block SSL mismatches in URLs, even if the URL isn't in the database.
# This can lead to false positives.
#
# Default: no
#PhishingAlwaysBlockSSLMismatch no
# Always block cloaked URLs, even if URL isn't in database.
# This can lead to false positives.
#
# Default: no
#PhishingAlwaysBlockCloak no
# Detect partition intersections in raw disk images using heuristics.
# Default: no
#PartitionIntersection no
# Allow heuristic match to take precedence.
# When enabled, if a heuristic scan (such as phishingScan) detects
# a possible virus/phish it will stop scan immediately. Recommended, saves CPU
# scan-time.
# When disabled, virus/phish detected by heuristic scans will be reported only at
# the end of a scan. If an archive contains both a heuristically detected
# virus/phish, and a real malware, the real malware will be reported
#
# Keep this disabled if you intend to handle "*.Heuristics.*" viruses 
# differently from "real" malware.
# If a non-heuristically-detected virus (signature-based) is found first, 
# the scan is interrupted immediately, regardless of this config option.
#
# Default: no
#HeuristicScanPrecedence yes
##
## Data Loss Prevention (DLP)
##
# Enable the DLP module
# Default: No
#StructuredDataDetection yes
# This option sets the lowest number of Credit Card numbers found in a file
# to generate a detect.
# Default: 3
#StructuredMinCreditCardCount 5
# This option sets the lowest number of Social Security Numbers found
# in a file to generate a detect.
# Default: 3
#StructuredMinSSNCount 5
# With this option enabled the DLP module will search for valid
# SSNs formatted as xxx-yy-zzzz
# Default: yes
#StructuredSSNFormatNormal yes
# With this option enabled the DLP module will search for valid
# SSNs formatted as xxxyyzzzz
# Default: no
#StructuredSSNFormatStripped yes
##
## HTML
##
# Perform HTML normalisation and decryption of MS Script Encoder code.
# Default: yes
# If you turn off this option, the original files will still be scanned, but
# without additional processing.
#ScanHTML yes
##
## Archives
##
# ClamAV can scan within archives and compressed files.
# If you turn off this option, the original files will still be scanned, but
# without unpacking and additional processing.
# Default: yes
#ScanArchive yes
# Mark encrypted archives as viruses (Encrypted.Zip, Encrypted.RAR).
# Default: no
#ArchiveBlockEncrypted no
##
## Limits
##
# The options below protect your system against Denial of Service attacks
# using archive bombs.
# This option sets the maximum amount of data to be scanned for each input file.
# Archives and other containers are recursively extracted and scanned up to this
# value.
# Value of 0 disables the limit
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 100M
#MaxScanSize 150M
MaxScanSize 250M
# Files larger than this limit won't be scanned. Affects the input file itself
# as well as files contained inside it (when the input file is an archive, a
# document or some other kind of container).
# Value of 0 disables the limit.
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 25M
#MaxFileSize 30M
MaxFileSize 500M
# Nested archives are scanned recursively, e.g. if a Zip archive contains a RAR
# file, all files within it will also be scanned. This options specifies how
# deeply the process should be continued.
# Note: setting this limit too high may result in severe damage to the system.
# Default: 16
#MaxRecursion 10
# Number of files to be scanned within an archive, a document, or any other
# container file.
# Value of 0 disables the limit.
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 10000
#MaxFiles 15000
# Maximum size of a file to check for embedded PE. Files larger than this value
# will skip the additional analysis step.
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 10M
#MaxEmbeddedPE 10M
# Maximum size of a HTML file to normalize. HTML files larger than this value
# will not be normalized or scanned.
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 10M
#MaxHTMLNormalize 10M
# Maximum size of a normalized HTML file to scan. HTML files larger than this
#  value after normalization will not be scanned.
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 2M
#MaxHTMLNoTags 2M 
# Maximum size of a script file to normalize. Script content larger than this
# value will not be normalized or scanned.
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 5M
#MaxScriptNormalize 5M
# Maximum size of a ZIP file to reanalyze type recognition. ZIP files larger
# than this value will skip the step to potentially reanalyze as PE.
# Note: disabling this limit or setting it too high may result in severe damage
# to the system.
# Default: 1M
#MaxZipTypeRcg 1M
# This option sets the maximum number of partitions of a raw disk image to be scanned.
# Raw disk images with more partitions than this value will have up to the value number
# partitions scanned. Negative values are not allowed.
# Note: setting this limit too high may result in severe damage or impact performance.
# Default: 50
#MaxPartitions 128
# This option sets the maximum number of icons within a PE to be scanned.
# PE files with more icons than this value will have up to the value number icons scanned.
# Negative values are not allowed.
# WARNING: setting this limit too high may result in severe damage or impact performance.
# Default: 100
#MaxIconsPE 200
# This option sets the maximum recursive calls for HWP3 parsing during scanning.
# HWP3 files using more than this limit will be terminated and alert the user.
# Scans will be unable to scan any HWP3 attachments if the recursive limit is reached.
# Negative values are not allowed.
# WARNING: setting this limit too high may result in severe damage or impact performance.
# Default: 16
#MaxRecHWP3 16
# This option sets the maximum calls to the PCRE match function during an instance of regex matching.
# Instances using more than this limit will be terminated and alert the user but the scan will continue.
# For more information on match_limit, see the PCRE documentation.
# Negative values are not allowed.
# WARNING: setting this limit too high may severely impact performance.
# Default: 10000
#PCREMatchLimit 20000
# This option sets the maximum recursive calls to the PCRE match function during an instance of regex matching.
# Instances using more than this limit will be terminated and alert the user but the scan will continue.
# For more information on match_limit_recursion, see the PCRE documentation.
# Negative values are not allowed and values > PCREMatchLimit are superfluous.
# WARNING: setting this limit too high may severely impact performance.
# Default: 5000
#PCRERecMatchLimit 10000
# This option sets the maximum filesize for which PCRE subsigs will be executed.
# Files exceeding this limit will not have PCRE subsigs executed unless a subsig is encompassed to a smaller buffer.
# Negative values are not allowed.
# Setting this value to zero disables the limit.
# WARNING: setting this limit too high or disabling it may severely impact performance.
# Default: 25M
#PCREMaxFileSize 100M
##
## On-access Scan Settings
##
# Enable on-access scanning. Currently, this is supported via fanotify.
# Clamuko/Dazuko support has been deprecated.
# Default: no
ScanOnAccess yes
# Set the  mount point to be scanned. The mount point specified, or the mount point 
# containing the specified directory will be watched. If any directories are specified, 
# this option will preempt the DDD system. This will notify only. It can be used multiple times.
# (On-access scan only)
# Default: disabled
#OnAccessMountPath /
#OnAccessMountPath /home/user
# Don't scan files larger than OnAccessMaxFileSize
# Value of 0 disables the limit.
# Default: 5M
#OnAccessMaxFileSize 10M
# Set the include paths (all files inside them will be scanned). You can have
# multiple OnAccessIncludePath directives but each directory must be added
# in a separate line. (On-access scan only)
# Default: disabled
#OnAccessIncludePath /home
#OnAccessIncludePath /students
OnAccessIncludePath /home/rpf/Downloads
# Set the exclude paths. All subdirectories are also excluded.
# (On-access scan only)
# Default: disabled
#OnAccessExcludePath /home/bofh
# With this option you can whitelist specific UIDs. Processes with these UIDs
# will be able to access all files.
# This option can be used multiple times (one per line).
# Default: disabled
OnAccessExcludeUID 0
# Toggles dynamic directory determination. Allows for recursively watching include paths.
# (On-access scan only)
# Default: no
OnAccessDisableDDD yes
# Modifies fanotify blocking behaviour when handling permission events.
# If off, fanotify will only notify if the file scanned is a virus,
# and not perform any blocking.
# (On-access scan only)
# Default: no
OnAccessPrevention yes
# Toggles extra scanning and notifications when a file or directory is created or moved.
# Requires the  DDD system to kick-off extra scans.
# (On-access scan only)
# Default: no
#OnAccessExtraScanning yes
##
## Bytecode
##
# With this option enabled ClamAV will load bytecode from the database. 
# It is highly recommended you keep this option on, otherwise you'll miss detections for many new viruses.
# Default: yes
Bytecode yes
# Set bytecode security level.
# Possible values:
#       None - no security at all, meant for debugging. DO NOT USE THIS ON PRODUCTION SYSTEMS
#         This value is only available if clamav was built with --enable-debug!
#       TrustSigned - trust bytecode loaded from signed .c[lv]d files,
#                insert runtime safety checks for bytecode loaded from other sources
#       Paranoid - don't trust any bytecode, insert runtime checks for all
# Recommended: TrustSigned, because bytecode in .cvd files already has these checks
# Note that by default only signed bytecode is loaded, currently you can only
# load unsigned bytecode in --enable-debug mode.
#
# Default: TrustSigned
BytecodeSecurity TrustSigned
# Set bytecode timeout in miliseconds.
# 
# Default: 5000
# BytecodeTimeout 1000
##
## Statistics gathering and submitting
##
# Enable statistical reporting.
# Default: no
#StatsEnabled yes
# Disable submission of individual PE sections for files flagged as malware.
# Default: no
#StatsPEDisabled yes
# HostID in the form of an UUID to use when submitting statistical information.
# Default: auto
#StatsHostID auto
# Time in seconds to wait for the stats server to come back with a response
# Default: 10
#StatsTimeout 10
```

Thunderbird ClamdRib lin:

```
TCPSocket 3310
TCPAddr 127.0.0.1
```

## Avahi

```
[system] Activation via systemd failed for unit 'dbus-org.freedesktop.Avahi.service': Unit dbus-org.freedesktop.Avahi.service not found.
```

[Archlinux forum](https://archlinuxarm.org/forum/viewtopic.php?f=53&t=8004)

```
cd /usr/lib/systemd/system/
ln -s avahi-daemon.service /usr/lib/systemd/system/dbus-org.freedesktop.Avahi.service
```

## Image copy

[Tinyapps](https://tinyapps.org/docs/mount_partitions_from_disk_images.html)


```
losetup --partscan --find --show disk.img
lsblk --fs
```

Recreate part table 

```
dd if=/dev/loop0p1 of=/dev/sda1 status=progress
```

## X-Access

```
cp /from/.Xauthority /to/.Xauthority
export DISPLAY=:0.0
```

## VirtualBox

Use:
```
virtualbox 
virtualbox-host-modules-arch
```

## Custom kernel


```
pacman -S base-devel
pacman -S cvsup wget abs
```

# Arch way (But cleans up everything from scratch, bad to use own config or patched srcs)

```
ASPROOT=. asp checkout linux
```

```
makepkg -s
```

```
pacman -U linux-custom-4.12.8-2-x86_64.pkg.tar.xz 
pacman -U linux-custom-4.12.8-2-x86_64.pkg.tar.xz 
```


# Hybrid approach (best with the above run first)

```
cd build/linux/repos/core-x86_64/src/linux-4.12
cp arch/x86_64/boot/bzImage /boot/vmlinuz-linux-custom
make modules_install
mkinitcpio -g /boot/initramfs-linux-custom.img
```



And regenerate Grub menu.

```
grub-mkconfig -o /boot/grub/grub.cfg
```

[En Wiki](https://wiki.archlinux.org/index.php/Kernels/Arch_Build_System)

[De Wiki more verbose, more option but also outdated for some parts](https://wiki.archlinux.de/title/Eigenen_Kernel_erstellen)

## Firewall

[Simple stateful FW](https://wiki.archlinux.org/index.php/Simple_stateful_firewall)

IpTables rules allow ping & ntp

```

# Generated by iptables-save v1.6.1 on Tue Aug 29 17:13:25 2017
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]
:TCP - [0:0]
:UDP - [0:0]
-A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m conntrack --ctstate INVALID -j DROP
-A INPUT -p icmp -m icmp --icmp-type 8 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p udp -m conntrack --ctstate NEW -j UDP
-A INPUT -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j TCP
-A INPUT -p udp -m udp --dport 123 -j ACCEPT
-A INPUT -p udp -j REJECT --reject-with icmp-port-unreachable
-A INPUT -p tcp -j REJECT --reject-with tcp-reset
-A INPUT -j REJECT --reject-with icmp-proto-unreachable
COMMIT
# Completed on Tue Aug 29 17:13:25 2017
```

Ip6Tables 
```

# Generated by ip6tables-save v1.6.1 on Tue Aug 29 17:14:22 2017
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]
:TCP - [0:0]
:UDP - [0:0]
-A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m conntrack --ctstate INVALID -j DROP
-A INPUT -p ipv6-icmp -m icmp6 --icmpv6-type 128 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p udp -m conntrack --ctstate NEW -j UDP
-A INPUT -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j TCP
-A INPUT -p udp -m udp --dport 123 -j ACCEPT
-A INPUT -p udp -j REJECT --reject-with icmp6-adm-prohibited
-A INPUT -p tcp -j REJECT --reject-with tcp-reset
-A INPUT -j REJECT --reject-with icmp6-adm-prohibited
COMMIT
# Completed on Tue Aug 29 17:14:22 2017
```

Saving it, and enable by boot : 

```
systemctl enable --now iptables
systemctl enable --now ip6tables
```

Show rules:

```
iptables -nvL
```

## share X access

```
cat ~/.Xauthority | sudo -u user2 -i tee .Xauthority > /dev/null
```

## Second X screen

```
xinit -- :1
```

## Ntp

[Win use UTC](https://www.georglutz.de/blog/2011/06/13/echtzeit-uhr-unter-windows-auf-utc-stellen/)

## gtkmm-3

Dependencies:

```
pacman -S glibmm
pacman -S pagomm
pacman -S pangomm
pacman -S cairomm
pacman -S atkmm
```

Config & make:

```
./configure --prefix=/usr
make prefix=/usr
```

Install

```
make prefix=/usr install
```

## Netstat replacement

[BinaryTides info for netstat replacement](http://www.binarytides.com/linux-ss-command/)

## Java

Add to .bashrc

```
_SILENT_JAVA_OPTIONS='-Dawt.useSystemAAFontSettings=gasp -Dswing.aatext=true'
```

as the _JAVA_OPTIONS confuses the jdk build

Warning: using ttf-fira-code will crash XOrg!!!

### Netbeans

to add Javadocs in Netbeans install "java-8-openjdk" or alike

Add in Netbeans in Java-platform


```
/usr/share/doc/java8-openjdk/jdk/api/javadoc
```


### Java11

Requires to link the config dir:

```
# cd /etc
# ln -s java11-jre java11-jre11
```

Errors in netbeans log: unlimited cryto policy not found
displayed error: Class not found... Could not initialize class sun.security.ssl.SSLContextImpl$DefaultSSLContext

## Sane

Debug

```
export SANE_DEBUG_DLL=255
```

Usb 3 workaround

```
export SANE_USB_WORKAROUND=1
```

[Genesys man](https://www.systutorials.com/docs/linux/man/5-sane-genesys/)

## Pacman

Run automated database clean & update:
```
alias update="paccache -rk2 && pacman -Syu"
```

## Packages


-Fresh/clean install:----------------------------------------------
```

alsa-utils 1.1.5-2
autoconf 2.69-4
automake 1.15.1-1
bash 4.4.012-2
binutils 2.29.1-1
bison 3.0.4-3
breeze-gtk 5.11.4-1
bzip2 1.0.6-6
clamav 0.99.2-7
coreutils 8.28-1
cryptsetup 2.0.0-1
device-mapper 2.02.177-1
dhcpcd 6.11.5-1
diffutils 3.6-1
dosfstools 4.1-1
e2fsprogs 1.43.7-1
efibootmgr 15-1
exo 0.11.5-1
fakeroot 1.22-1
file 5.32-1
filesystem 2017.10-2
findutils 4.6.0-2
firefox 57.0.2-1
flex 2.6.4-1
garcon 0.6.1-1
gawk 4.2.0-2
gcc 7.2.1-2
gcc-libs 7.2.1-2
gdb 8.0.1-1
gettext 0.19.8.1-2
gimp 2.8.22-1
git 2.15.1-2
glade 3.20.2-1
glibc 2.26-8
grep 3.1-1
grub 2:2.02-4
gtk-xfce-engine 2.10.1-1
gtkmm 2.24.5-2
gzip 1.8-2
inetutils 1.9.4-5
intel-ucode 20171117-1
iproute2 4.14.1-2
iputils 20161105.1f2bb12-2
jdk8-openjdk 8.u144-1
jedit 5.4.0-5
jfsutils 1.1.15-4
less 487-1
libfm-gtk2 1.2.5-1
licenses 20171006-1
linux 4.14.8-1
logrotate 3.13.0-1
lvm2 2.02.177-1
m4 1.4.18-1
make 4.2.1-2
man-db 2.7.6.1-2
man-pages 4.14-1
mdadm 4.0-1
nano 2.9.1-1
netctl 1.14-1
ntp 4.2.8.p10-2
pacman 5.0.2-2
parted 3.2-6
patch 2.7.5-1
pciutils 3.5.6-1
pcmciautils 018-7
perl 5.26.1-1
pkg-config 0.29.2-1
procps-ng 3.3.12-2
psmisc 23.1-1
reiserfsprogs 3.6.27-1
s-nail 14.9.6-1
sed 4.4-1
shadow 4.5-4
subversion 1.9.7-3
sudo 1.8.21.p2-1
sysfsutils 2.1.0-9
systemd-sysvcompat 236.0-2
tar 1.30-1
texinfo 6.5-1
thunar 1.6.13-1
thunar-volman 0.8.1-2
thunderbird 52.5.0-1
ttf-dejavu 2.37-1
tumbler 0.2.0-1
usbutils 009-1
util-linux 2.31-2
vi 1:070224-2
virtualbox 5.2.4-1
virtualbox-host-modules-arch 5.2.4-3
which 2.21-2
xf86-video-intel 1:2.99.917+802+gaf6d8e9e-1
xf86-video-vesa 2.3.4-4
xfce4-appfinder 4.12.0-4
xfce4-panel 4.12.2-1
xfce4-power-manager 1.6.1-1
xfce4-session 4.12.1-7
xfce4-settings 4.12.1-1
xfce4-taskmanager 1.2.0-1
xfce4-terminal 0.8.6-1
xfconf 4.12.1-4
xfdesktop 4.12.4-1
xfsprogs 4.13.1-1
xfwm4 4.12.4-1
xfwm4-themes 4.10.0-2
xorg-server 1.19.5-1
xorg-xinit 1.3.4-4
```

## Logo

fancy login / logo script

```
#!/bin/bash 
#
# based on python archey3  
# Copyright 2010 Melik Manukyan <melik@archlinux.us>
# Copyright 2010-2012 Laurie Clark-Michalek <bluepeppers@archlinux.us>
# Distributed under the terms of the GNU General Public License v3.
# See http://www.gnu.org/licenses/gpl.txt for the full license text.
#
# Simple bash script to display an Archlinux logo in ASCII art
# Along with basic system information.
# just wanted a fancy login used bash for a better response on older system
#
function memory() 
{
 #mem=`free -m |grep "Mem:" | sed -e 's/ \+/~/g' `
 #total=`echo ${mem} | cut -d "~" -f2`
 #free=`echo ${mem} | cut -d "~" -f3`
 local -a mem
 mem=(`free -m |grep "Mem:"`)
 local total=${mem[1]}
 local free=${mem[2]}
 echo "${free} MB used of ${total} MB"
}
function fs() 
{
 local "$@"
 #local fst=`df -TPh $1 | tail -n1 | sed -e 's/ \+/_/g' `
 #local fsType=`echo $fst | cut -d "_" -f2`
 #local fsTotal=`echo $fst | cut -d "_" -f3`
 #local fsFree=`echo $fst | cut -d "_" -f5`
 #local fsPerc=`echo $fst | cut -d "_" -f6`
 local -a fst
 fst=(`df -TPh $root | tail -n1`)
 local fsType=${fst[1]}
 local fsTotal=${fst[2]}
 local fsFree=${fst[4]}
 local fsPerc=${fst[5]}
 echo "$root ${fsFree} free of ${fsTotal} used ${fsPerc} (${fsType})"
}
function sys() 
{
 local sys=`systemctl list-units |wc -l`
 local sys=$(expr $sys - 1)
 local sysFail=`systemctl  --failed |wc -l`
 local sysFail=$(expr $sysFail - 1)
 echo "${sys} running ${sysFail} failed"
}
function disp()
{
 local vga=`lspci | grep "VGA" | sed -e 's/.*: //'`
 echo "${vga}"
}
function proc() 
{
 local all=`ps -e |wc -l`
 local all=$(expr $all - 1)
 local usr=`ps |wc -l`
 local usr=$(expr $usr - 1)
 local sys=$(expr $all - $usr)
 echo "${sys} sys ${usr} user"
}
function pack() 
{
 local total=`pacman -Q |wc -l`
# nice but takes too long
#  local exp=`pacman -Qe |wc -l`
 echo "$total total"
}
function cpu() 
{
 cat /proc/cpuinfo | grep "model name" |cut -d ":" -f2
}
function net()
{
	for intf in $( ls -d /sys/class/net/en* ) ; do
		local speed=`cat $intf/speed`
		local duplex=`cat $intf/duplex`
		local updown=`cat $intf/operstate`
	 	if [[ ${speed} -ge 1000 ]] ; then
			local speed="$(expr $speed / 1000)G"
		else 
			local speed="${speed}M"
		fi
		local inf=`echo ${intf} | cut -d"/" -f5`
		local ip=`ip addr show ${inf} | grep "inet " |cut -d" " -f6 `		
		echo "${inf} ${speed} ${duplex} ${updown} ${ip}"
#		inf=`ip -j -o -4 addr show dev $intf`
#		for i in {1..10} ; do
#			infn=`cut -d"," -f$i`
#			if [[ $infn == "local"* ]] ; then
#				echo "$intf $infn" 
#			fi
#		done
	done
}
function ansi_color()
{
	local "$@"
	case $color in 
		'black')	echo '0';; 
		'red')		echo '1';;
		'green')	echo '2';;
		'yellow')	echo '3';;
		'blue')		echo '4';;
		'magenta')	echo '5';;
		'cyan')		echo '6';;
		'white')	echo '7';;
	esac
}
function info()
{
local "$@"
local c=$( ansi_color color=$color)
local c1="\033[1;3${c}m"
local c2="\033[0;3${c}m"
norm="\033[0m"
echo -e "${c1}               +                "
echo -e "${c1}               #                ${c1}OS  :${norm} `uname -s -m`"
echo -e "${c1}              ###               ${c1}Vers:${norm} `uname -r`"
echo -e "${c1}             #####              ${c1}Host:${norm} `uname -n`"
echo -e "${c1}             ######             ${c1}Mem :${norm} $( memory )"
echo -e "${c1}            ; #####;            ${c1}Cpu :${norm}$( cpu )"
echo -e "${c1}           +##.#####            ${c1}Disp:${norm} $( disp )"
echo -e "${c1}          +##########           ${c1}Pack:${norm} $( pack )"
echo -e "${c1}         ######${c2}#####${c1}##;         ${c1}Proc:${norm} $( proc )"       
echo -e "${c1}        ###${c2}############${c1}+        ${c1}Net :${norm} $( net )"
echo -e "${c1}       #${c2}######   #######        ${c1}Sys :${norm} $( sys )"
echo -e "${c2}     .######;     ;###;\`\".      ${c1}Fs  :${norm} $( fs root='/' )"
echo -e "${c2}    .#######;     ;#####.       "
echo -e "${c2}    #########.   .########\`    "
echo -e "${c2}   ######'           '######    "
echo -e "${c2}  ;####                 ####;   "
echo -e "${c2}  ##'                     '##   "
echo -e "${c2} #'                         \`#  "
echo -e ${norm}
}
info color="yellow"
```

## Hbci



Download jameica unzip, run jameica.sh choose plugin hibiscus

### Reinert cyberjack

```
git clone https://aur.archlinux.org/pcsc-cyberjack.git
cd pcsc-cyberjack 
makepkg -s
# pacman -U pcsc-cyberjack-3.99.5_SP13-1-x86_64.pkg.tar.xz
#  systemctl  enable --now pcscd.service
```

Download cyberjack linux driver

```
aclocal
autoheader
automake
autoconf
autoreconf --install
./configure
make
```

Result poba works, comba uses a RSA encoded card not supported by hibiscus (only DSA(DES)...)

to:
```
~/.jameica/cfg 
```
add 
```
sun.security.smartcardio.library=/usr/lib/libpcsclite.so.1
```

## Qt-theme

as audacious is switched to qt5 it does no longer share the gtk-theme.

So install

```
qt5ct
```

For some themes might look nice (but not the dark one) ...

```
qtcurve-qt5
```

run 

```
 qt5ct 
```

set env:

```
export QT_QPA_PLATFORMTHEME="qt5ct"
```

## Journal

In /etc/systemd/journald.conf enable the following to limit the journal size:

```
SystemMaxUse=100M
```

## Yay

includes aur maintenance :

https://linuxhint.com/aur_arch_linux/

## Dbus

List services 

```
dbus-send --print-reply --dest=org.freedesktop.DBus  /org/freedesktop/DBus org.freedesktop.DBus.ListNames
```

Use d-feet itrospection

```
gdbus-codegen --generate-c-code thumbnail-generated --output-directory ../src  thumbnail1.xml
```

## Thunderbird

Upgrade to 102 will frezze on startup.

According to [Mozilla](https://bugzilla.mozilla.org/show_bug.cgi?id=1776218) this is related to calendar.

Change/add within profile user.js: 

```
user_pref("calendar.icaljs", false);
