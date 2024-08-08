
# Autotools

The Gnu way to build ...

[Autotools Pdf](https://www.lrde.epita.fr/~adl/dl/autotools.pdf)

## Error "libtool: Version mismatch error."

```
autoreconf -fi
```

## configure.ac

To add pgkconf-libs:

```
PKG_CHECK_MODULES... gthreads-2.0
```

Configure options e.g. ???:

```
ENABLE_NLS
```

## Makefile.am

```
SUBDIRS = src res po
```

Better do this in antuja with add as otherwise the depdenecies are missing...

## src/Makefile.am

```
logic_CXXFLAGS = -std=c++11 
```

## res/Makefile

Example resource build:

```
all: ../src/resources.c
clean:
   rm ../src/resources.c
PKGCONFIG = pkgconf
GLIB_COMPILE_RESOURCES = $(shell $(PKGCONFIG) --variable=glib_compile_resources gio-2.0)
#$(info $$GLIB_COMPILE_RESOURCES is [${GLIB_COMPILE_RESOURCES}])
resources = $(shell $(GLIB_COMPILE_RESOURCES) --sourcedir=. --generate-dependencies gresource.xml)
#$(info $$resources is [${resources}])
../src/resources.c: gresource.xml $(resources)
    $(GLIB_COMPILE_RESOURCES) gresource.xml --target=$@ --sourcedir=. --generate-source
```

## Initalisation compatible anjuta 4

```
setlocale(LC_ALL, "");
bindtextdomain(GETTEXT_PACKAGE, PACKAGE_LOCALE_DIR);
bind_textdomain_codeset(GETTEXT_PACKAGE, "UTF-8");
textdomain(GETTEXT_PACKAGE);
```

# gettext

The rather confusing way to internationalization:

[Gnu gettext](https://www.gnu.org/software/gettext/manual/gettext.html)

[the Gnome way](https://wiki.gnome.org/TranslationProject/LocalisationGuide)

To extract text:

```
xgettext -keyword=_ --output=- LogicDrawing.cpp
