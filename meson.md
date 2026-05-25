
# Meson

With more everyday options build in.

To setup the build directory:
<pre>
meson setup build -Dprefix=/usr
</pre>
The prefix is a suggestion, with msys2 use ${MINGW_PREFIX}.
Additional optiona may be possible use configure or read meson.options.

To compile use:
</pre>
cd build
meson compile
</pre>

To change the configuration use:
</pre>
meson setup --reconfigure --wipe
</pre>
Wipe clears out previous files

To change the build type (with my scripts this is the default):
<pre>
meson configure build --buildtype=debugoptimized
</pre>
Other values (see ): debug,debugoptimized,release,minsize,plain
See
[Tutorialpedia](https://www.tutorialpedia.org/blog/how-do-i-set-basic-options-with-meson/)


To check configuration:
<pre>
meson configure build
</pre>