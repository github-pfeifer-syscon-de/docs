# Netbeans

## Nb8 repos for 9+

https://sunilkanzar.wordpress.com/2018/08/02/plugin-set-up-in-netbeans-9-0/

## Darcula

Theme (thats all in one)

clone https://github.com/Revivius/nb-darcula

Install the darcula.jar localy:

[media:darcula.zip](media:darcula.zip.md)

mvn  install:install-file -Dfile=darcula.jar -DgroupId=com.bulenkov -DartifactId=darcula  -Dversion=1.0.0  -Dpackaging=jar

modify pom.xml:

```

        <!-- repository>
            <id>netbeans</id>
            <name>Repository hosting NetBeans modules</name>
            <url>http://bits.netbeans.org/nexus/content/groups/netbeans</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository -->
        <repository>
            <id>netbeans</id>
            <name>NetBeans</name>
            <url>https://netbeans.apidesign.org/maven2/</url>
        </repository>
```

Or the resulting .nbm

[Darcula AddOn](images/Darcula.zip)

## Toolbar reset

```
..\AppData\Roaming\NetBeans\8.2\config
delete the Windows2Local

## Windows Msys2 integration

with newer version of netbeans e.g. 25 the integration of msys2 became a bit fiddely. 
- install Netbeans 
- install Msys2 
- there seems to be preference to use Mingw64, so add to path: C:\msys64\usr\bin C:\msys64\mingw64\bin
- install the build-tools e.g. pacman -S base-devel  mingw-w64-x86_64-gcc mingw-w64-x86_64-autotools ...
- in Netbeans open Options C/C++ Add enviroment base dir c:\msys64\mingw64 (and if needed some of the tools)
- now the building C/C++ projects should work
