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
