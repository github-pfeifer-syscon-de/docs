# Java3d

Some advice for using Java3d. Sounds to easy to describe? Right if you live on windows there is nothing more to describe than download and run ...

The problems arise if you try to get things running on a Mac (Linux might suffer from similar problems).

The reason for that lies simply in the history: 

Sun created the thing, Oracle took over and given things to the open source community (Without updating the readme's to state the actual situation).

## Leagacy

There might be a way to get things running when you use the Java3d 1.5.2 Version provided by Oracle.
The important point is to use the Jogl 1 implementation with that (this shoud at least work in theory).

## Present times

Important: remove all existing Java3d files from /Library/...java ! (As long as a previous version is present nothing will run).

Get Java3d files from [JoglJava3d](https://jogamp.org/deployment/java3d/) i used the latest 1.6 version.

Get a jogamp-all-platforms.7z from [Jogl current](http://jogamp.org/deployment/jogamp-current/archive/).

Include java3d*.jar's and gluegen-rt.jar and jogl-all.jar in classpath to get things running (you need most of the archive content as binary libraries are resolved at runtime, just to mention if you want to optimize this do it later).
