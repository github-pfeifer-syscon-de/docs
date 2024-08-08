# Java projects

Some project are build with java:

[Java](Java.md)

# Gtk3

Most projects listed here have been migrated to github  see [Github-Pfeifer-Syscon](https://github.com/github-pfeifer-syscon-de).

## fractale generator

![Julia fractale](/images/julia.png)

Example of a mandelbrot and julia generator:

[Media:Fract.zip](Media:Fract.zip.md) (C++ source, automake project, shoud work on any linux system dependencies: gtkmm3, glibmm2, gthreads )

#### build & run for Debian/Ubuntu

```
apt-get install build-essential libgtkmm-3.0-dev
cd fract-program-0.1
((do "autoreconf -i" if needed))
./configure
make
./fract
```

if you like to keep it 

```
# make install
```

#### build for windows

Install 
[msys2 for gtkmm,...](https://wiki.gnome.org/Projects/gtkmm/MSWindows)

Also useful infos:
[Msys2](https://github.com/msys2/msys2/wiki/MSYS2-installation).

```
 pacman -S mingw-w64-x86_64-toolchain base-devel unzip autotools mingw-w64-x86_64-gtkmm3 mingw-w64-x86_64-gsettings-desktop-schemas
```

In netbeans change build host to localhost (Win), Tools Mingw

## GenericGlm

Basic OpenGL function lib, using Gtk::GlArea including antialiased display, text display... 

[Media:Genericglm.zip](Media:Genericglm.zip.md) (C++ source, autotools, shoud work on any linux system dependencies: gtkmm3, glibmm2, fontconfig, freetype, epoxy, glu, glm )

Uptodate version at [Github](https://github.com/github-pfeifer-syscon-de).

#### build & run for Debian/Ubuntu
Additional to the above:

```
# apt-get install libglm-dev libglu1-mesa-dev
```

The glm includes may require tweaking depending on version:

```
 glm/ext/matrix_transform.hpp ->  glm/gtc/matrix_transform.hpp
```

#### build for Raspi's

```
./configure --prefix=/usr --with-gles  
```

The openGL implementation is switched to GL ES 3. 
Required to work with dtoverlay=vc4-kms-v3d in /boot/config.txt

Use --with-gles  also suggested for programs e.g. monglmm.

#### build for windows
Additional to the above:

----------------------
Temp collection try to fix, use glu in wind../sys32, link option:
```
pacman -S mingw-w64-x86_64-glm
in configure.ac comment "dnl PKG_CHECK_MODULES(GLU, [glu])
in src/Makefile.am change "_LDFLAGS = -lglu32 -no-undefined"
./configure --prefix=/mingw64
```


Somewhat aged but usefu link switch -mwindows:
http://users.wfu.edu/cottrell/cross-gtk/

## GLglobe

![Glglobe](/images/glglobe.png)

Example of a fancy desktop clock with world time, overlay satellite images, geo.json files.

[Media:Glglobe.zip](Media:Glglobe.zip.md) (C++ source, autotools project, shoud work on any linux system dependencies: gtkmm3, glibmm2, jsonglib1, libsoup3, GenericGlm)

Uptodate version at [Github](https://github.com/github-pfeifer-syscon-de).

&copy; Textures [Solarsystemscope.com](http://solarsystemscope.com)

&copy; Satellite image service [Realearth.ssec.wisc.edu](https://realearth.ssec.wisc.edu/)

#### build & run for Debian/Ubuntu
As above.

#### build for windows
As above

Add to glglobe_LDFLAGS -lglu32

Not working: correct timzone display

#### build for MacOS
Use Macports or alike...

Or this port with limited functions ![OSXglobe](/images/OSXglobe.zip) as XCode-project.

## Monglmm

![Monglmm](/images/monglmm.png)

Some kind of different system monitor (load, memory, network, disk, clock) for linux, delegates most display work to graphis-card.

[Media:Monglmm.zip](Media:Monglmm.zip.md) (C++ source, autotools project, shoud work on any linux system (with user accessible /proc) dependencies : gtkmm3, glibmm2, GenericGlm )(for g15 functions use ./configure --with-libg15 requires libusb)(for hardware sensors use --with-lmsensors requires lmsensors,  use  --with-gles to use glES e.g. nice for Raspi)

Uptodate version at [Github](https://github.com/github-pfeifer-syscon-de).

&copy; g15-function [Sourceforge g15tools](https://sourceforge.net/projects/g15tools/)

#### build & run for Debian/Ubuntu
Additional to the above (if the #include "filesystem" causes trouble remove it):

```
apt-get install libusb-dev
```


#### Granting g15 usb permission

Add rules to <tt>/etc/udev/rules.d</tt> (needs activating, by rule reload or restart):

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="046d", ATTRS{idProduct}=="c626",
    ACTION=="add", GROUP="usbdev", MODE="0664"
SUBSYSTEM=="usb", ATTRS{idVendor}=="046d", ATTRS{idProduct}=="c227",
    ACTION=="add", GROUP="usbdev", MODE="0664"
```

Add user to usbdev group:

```
usermod your_users -a -G usbdev
```

## Logic

If you own a Raspi4 and intrested in Hardware development you sometimes need some insight what is happening in electrical terms with some speed. So here comes a tool that helps you discover these mysteries:

![Logic](/images/logic2.png)

Example connect a switch to Gpio 4 (Raspi 40 pin header pin 7 [Gpio pinout](https://pinout.xyz/#)) and Ground (pin 9). With the program you now choose the Trigger 4 and "Single" (capture) and activate the switch. In a moment you see that a switch is not just on or off but usually bounces back and forth some times.

![Logic 2](/images/logic.png)

"Logic 2" is a example of a Spi communication.

[Media:LogicSrc.zip](Media:LogicSrc.zip.md) (C++ source, netbeans project, shoud work on any raspi dependencies: gtkmm3, glibmm2)

&copy; bcm2835-lib [mikem/bcm2835](http://www.airspayce.com/mikem/bcm2835/)

#### build & run for Raspian

```
sudo apt-get install libgtkmm-3.0
cd Logic
./configure --prefix=/usr
make
```

Preperation:

The preparation steps that are presented when you first start the program are required! You shoud add 

```
dtoverlay=gpio-no-irq
```

to /boot/config.txt and reboot (without this setup the program will not use triggers or without reboot your pi will hang!). The next udev setup is included in Raspian buster by default. If you run this program as root the setup-assistant might help.

To run use:

```
dist/Debug/GNU-Linux/logic
```

If you want to keep it:

```
sudo make install
```

## Calculator

![Calc](/images/calc.png)

A calculator that allows you to write your calculation 3 + 4 * 5 and press (Cntl) (Enter) to see the result. Also supports variables e.g. a = 3 + 4 * 5. 

[Media:CalcSrc.zip](Media:CalcSrc.zip.md) (unmaintained, vala source, automake project, shoud work with 
```
./configure --prefix=/usr 
make
```
on any *ix, dependencies: gtk+3, libgee)

[Media:Calcpp.zip](Media:Calcpp.zip.md) (c++ 11 source, automake project, dependency: Gtkmm, Glibmm, libunistring ) if you want to compare vala <-> c++, vala offers the best support for gtk and glib, where c++ allows easy integration for c/c++ libs. By the way this also gives a example of some gtkmm programming (but still a bit more advanced as the documentation examples) e.g. custom components, setting binding, signal lambda funtions... 
It cares about your locale settings, so with a e.g. german locale you write 3,14 * r ^ 2 (sorry separators are not supported). 

&copy; The parsing part was inspired by the shunting yard algorithm by E.W.Dijkstra

### Build/use for windows

For msys2 see above. Additional we use gsetting and its config is somehow incomplete, for shell use:

```
export GSETTINGS_SCHEMA_DIR=/usr/share/glib-2.0/schemas
```

For cmd.exe use:

```
set GSETTINGS_SCHEMA_DIR=C:\msys64\usr\share\glib-2.0\schemas\
```

or integrate this in your default environment.

## Local docs

(file:///usr/share/gtk-doc/html/gtk3/gtk.html)

## Gtk glibmm codegen

[glibmm codegen](https://github.com/Pelagicore/gdbus-codegen-glibmm)

```
pacman -S python-pip
pip install Jinja2
