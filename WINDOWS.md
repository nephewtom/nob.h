# Notes for building on evil-Windows

## Small change to fit all MinGW-W64 compiler distributions
It seems some MinGW compiler distributions ship with a `cc.exe` executable file, but not all of them.
For example, in [WinLibs](https://winlibs.com/), aka: Brecht Sanders MinGW-W64 distribution, there is no `cc.exe`.
```
d:\
> where gcc.exe
C:\mingw64\bin\gcc.exe

d:\
> where cc.exe
INFO: Could not find files for the given pattern(s).

d:\
> gcc --version
gcc (MinGW-W64 x86_64-ucrt-posix-seh, built by Brecht Sanders, r3) 14.1.0
Copyright (C) 2024 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

Whereas in [skeeto w64devkit](https://github.com/skeeto/w64devkit) there is a `cc.exe` file.
```
d:/
$ where gcc
C:\w64devkit\bin\gcc.exe
C:\mingw64\bin\gcc.exe

d:/
$ where cc
C:\w64devkit\bin\cc.exe
```

So, just rename "cc" string to "gcc" in nob.h to fix it.

## nob.h files in how_to examples
The nob.h in this examples refer to the nob.h file in the root directory.
```
d:\playground\nob.h\how_to
> type 001_basic_usage\nob.h
../../nob.h
```

So, to avoid compilation errors, a symbolic link that points to the actual nob.h must be created.
And in evil-Windows, to avoid that the file becomes a copy, it needs to use `mklink` command.

But for some reason, in the Windows box tested requires admin permissions...
So, to create the symbolic link use the "Run as administrator" thing over Command Prompt (aka: CMD).

And then something like:

```
d:\playground\nob.h\how_to\001_basic_usage
> dir
 Volume in drive D is Disco
 Volume Serial Number is 34EF-48E9

 Directory of d:\playground\nob.h\how_to\001_basic_usage

30-Apr-25  12:57    <DIR>          .
30-Apr-25  12:57    <DIR>          ..
30-Apr-25  12:57                18 .gitignore
30-Apr-25  12:57             3,587 nob.c
30-Apr-25  12:57                11 nob.h
30-Apr-25  12:57    <DIR>          src
               3 File(s)          3,616 bytes
               3 Dir(s)  216,496,783,360 bytes free

d:\playground\nob.h\how_to\001_basic_usage
> del nob.h

d:\playground\nob.h\how_to\001_basic_usage
> mklink nob.h ..\..\nob.h
symbolic link created for nob.h <<===>> ..\..\nob.h

d:\playground\nob.h\how_to\001_basic_usage
>
```

Proceed in the same way with the other examples in how_to.

