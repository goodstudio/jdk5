| ![Java](https://soucod.github.io/jdk5/javalogo52x88.gif) | Windows Build InstructionsJavaTM 2 Platform Standard Edition, v5.0 fcs | [Build Overview](https://soucod.github.io/jdk5/build.html) |
| -------------------------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
|                                                          |                                                              |                                                            |

------

## Contents

- [Introduction](https://soucod.github.io/jdk5/build-windows-i586.html#intro)
- [Windows System Setup](https://soucod.github.io/jdk5/build-windows-i586.html#setup)
- [Windows Build Tools and Libraries Setup](https://soucod.github.io/jdk5/build-windows-i586.html#buildtools)
- [Windows Build Environment Variables](https://soucod.github.io/jdk5/build-windows-i586.html#environmentvariables)
- [Windows Build](https://soucod.github.io/jdk5/build-windows-i586.html#build)

Introduction

> This README file contains build instructions for the JavaTM 2 Platform Standard Edition, v5.0 (JDK 5.0) Community Source Release. Building the source code for the JDK requires a high level of technical expertise. Sun provides the source code primarily for technical experts who want to conduct research, port the platform to new operating systems and hardware, or add enhancements that require access to platform internals. If you are not a technical professional in one of these categories, this release is probably not for you.

Windows System Setup

> The official build platform for the JDK 5.0 is Windows Professional 2000 with Service Pack 2 or greater installed and Microsoft Security Patch MS03-026 installed.
>
> The minimum recommended hardware for building the Windows version is a Pentium class processor or better, at least 256 MB of RAM, and approximately 600 MB of free disk space.

Windows Build Tools and Libraries Setup

> Once the Windows OS is set up, you will need to install additional tools and libraries for building the JDK.

Microsoft Visual C++ Compiler

> The Microsoft Visual C++ 6.0 Professional Edition compiler, with Visual C++ 6.0 Service Pack 3 installed (
>
> not
>
>  
>
> Service Pack 4), is required for building the JDK. The compiler and other tools are expected to reside in the locations defined by the variables
>
>  
>
> MSVCDir
>
>  
>
> and
>
>  
>
> MSDevDir
>
> , which are set by the setup utility
>
>  
>
> VCVARS32.BAT
>
>  
>
> (usually found in
>
>  
>
> C:\Program Files\Microsoft Visual Studio\VC98\Bin\
>
> ). It is highly recommended that you run
>
>  
>
> VCVARS32.BAT
>
>  
>
> to set
>
>  
>
> MSVCDir
>
> ,
>
>  
>
> MSDevDir
>
> ,
>
>  
>
> INCLUDE
>
> ,
>
>  
>
> LIB
>
> , and the
>
>  
>
> PATH
>
>  
>
> prior to building the JDK. If your compiler resides in a place other than the default, you can set
>
>  
>
> ALT_COMPILER_PATH
>
>  
>
> to point to the location of the
>
>  
>
> cl
>
>  
>
> compiler binary, and
>
>  
>
> ALT_MSDEVTOOLS_PATH
>
>  
>
> to point to the location of the development tools. The compiler and tools binaries must be in the
>
>  
>
> PATH
>
> .

MKS Unix Command Binaries

> The JDK build requires access to Unix command binaries supplied by MKS Toolkit version 6.1a or later. Information about the MKS Toolkit can be obtained from the MKS website at
>
>  
>
> http://www.mks.com
>
> . If the binaries are not installed in
>
>  
>
> C:\mksnt\
>
> , set the
>
>  
>
> ALT_UNIXCOMMAND_PATH
>
>  
>
> environment variable to their location.

msvcrt.dll

> The JDK build requires access to msvcrt.dll version 6.00.8337.0 (version that we use ) supplied by Microsoft Visual C++ 6.0 Service Pack 2 (usually found in
>
>  
>
> C:\WINNT\System32\
>
> ). If the msvcrt.dll is not installed in
>
>  
>
> $(J2SE_TOPDIR)\make\redist\i586
>
> , set the
>
>  
>
> ALT_MSVCRT_DLL_PATH
>
>  
>
> environment variable to their location.

Bootstrap JDK

> You will need access to a JDK 1.4.2 for Windows installation. The 1.4.2 binaries can be downloaded from Sun's [JDK 1.4.2](http://java.sun.com/j2se/1.4.2/download.html) download site. Set `ALT_BOOTDIR` to point to the location of the bootstrap JDK installation, and place its `bin` directory in the `PATH`.



#### GNU Make

> GNU make version 3.78.1 or later, built for the MKS shell per the instructions in its documentation, is required to build the JDK. Information on GNU make, and access to ftp download sites, are available on the [GNU make web site](http://www.gnu.org/software/make/make.html). Place the location of the GNU make binary in the `PATH`.



#### `Zip and Unzip`

> `ZIP.EXE` and `UNZIP.EXE` should be installed in `C:\UTILS`. If you have them installed elsewhere set the environment variable `ALT_DEVTOOLS_PATH` to their location. ZIP.EXE version should be 2.2 or 2.[2-9]. UNZIP.EXE version should be 5.12 or 5.1[2-9]. Information and the source code for ZIP.EXE and UNZIP.EXE is available on the [info-zip web site](http://www.info-zip.org/).



#### cacerts

A certificates file named "cacerts" represents a system-wide keystore with CA certificates. In JDK and JRE binary bundles, the "cacerts" file contains root CA certificates from several public CAs (e.g., VeriSign, Thawte, and Baltimore).The source bundles contain a cacerts file without CA root certificates. JDK builders will need to secure permission from each public CA and include the certificates into their own custom cacerts file. Failure to provide a populated cacerts file will result in verification of a certificate chain during runtime.The `ALT_CACERTS_FILE` should be set to point to the location of the populated cacerts file.



#### Mozilla Headers and Libraries



> Note: The Java Plug-in product for Windows cannot be built from the Community Source Release. This section applies only to those with a separate source license for that product. The Mozilla Headers and Libraries are only required for building the Java Plug-in. If you do not build Java Plug-in, the headers and libraries are not required.
>
> Mozilla headers and libraries are required for building Java Plug-in. Since Netscape 7 has been widely adopted, we decided to stop building OJI plugin for Netscape 6.x in JDK release. As a result, the JDK build requires set of header files as shown below. [Download](http://wwws.sun.com/software/java2/download.html) and unpack the headers and libraries into a directory similar to the one shown below, and set the `ALT_MOZILLA_PATH` environment variable to the absolute path of the top-level directory.
>
> ```
>             +- devtools\                (set ALT_MOZILLA_PATH to this level)
>                +- share\
>                     +- plugin\
>                          +- mozilla_headers_ns7.win32\
>         
> ```
>
> The name of the top-level directory is not significant; it is not required to be named
>
>  
>
> devtools
>
> .



#### Microsoft DirectX 7 or DirectX 9 SDK header files and libraries



> Microsoft DirectX 7 or DirectX 9 SDK (Summer 2004 Update or newer) headers are required for building JDK. The DirectX 7 is no longer available, DirectX 9 SDK (Summer 2004 Update or newer) can be downloaded from 
>
> http://msdn.microsoft.com/library/default.asp?url=/downloads/list/directx.asp
>
> . If the link above becomes obsolete, the SDK can be obtained from
>
>  
>
> http://download.microsoft.com
>
>  
>
> (search with the keywords "directx 9 sdk"). If the SDK is not installed to
>
>  
>
> ```
> c:\dxsdk
> ```
>
> , set the
>
>  
>
> ```
> ALT_DXSDK_PATH
> ```
>
>  
>
> environment variable to its location.
>
> ```
> 	  c:\ 			(set ALT_DXSDK_DRIVE to this level)
>             +- DXSDK\		(set ALT_DXSDK_PATH to this level)
>                  +- Include\    (set ALT_DXSDK_INCLUDE_PATH to this level)
>                  +- Libs\       (set ALT_DXSDK_LIBS_PATH to this level)
>         
> ```
>
> Note: the ALT_DXSDK_* variables have the following order of precedence (from most to least):
>
> - ALT_DXSDK_INCLUDE_PATH, ALT_DXSDK_LIBS_PATH
> - ALT_DXSDK_PATH
> - ALT_DXSDK_DRIVE
>
> For example, ALT_DXSDK_DRIVE will be ignored if ALT_DXSDK_PATH is set.



#### Microsoft Platform Software Development Kit (SDK), November 2001 Edition

> The Microsoft Platform Software Development Kit (SDK), November 2001 Edition compiler, is required for building the Java Plug-in and/or Java Web Start. You will need to minimally install the
>
>  
>
> Core SDK
>
>  
>
> and the
>
>  
>
> Windows Installer SDK
>
>  
>
> features of this compiler.
>
> If your compiler resides in a place other than the default (usually found in `C:\Program Files\Microsoft SDK\`), you can set `ALT_PLUGIN_MSSDK` to point to the location of the `MSSDK`
>
> Because GNU Make has problems with spaces in command PATHS, it may be required that you set `ALT_PLUGIN_MSSDK` to point to the locations of the SDK using the Microsoft Mangled Path name convention. For example, if the Microsoft SDK is installed in `C:\Progam Files\Microsoft SDK` then the settings using mangled path names on the system may be: `SET ALT_PLUGIN_MSSDK=C:\Progra~1\Micros~3`, depending on the unique situation of your build machine.



#### Microsoft Layer for Unicode on Windows 95, 98 and Me Systems libraries



> Microsoft Layer for Unicode on Windows 95, 98 and Me Systems (MSLU) libraries are required for building JDK. The MSLU libraries consist of two files. One is the runtime binary (UnicoWS.dll) and the other is the static library (UnicoWS.lib). The runtime binary can be downloaded from here ( http://go.microsoft.com/fwlink/?LinkId=14851 ), and the static library is included in the Microsoft Platform SDK. The version of the runtime binary DLL is: 1.0.4018.0, and the static library should be the one in November 2002 edition (October 2002 update) of the Microsoft Platform SDK (Build 5.2.3718.1).
>
> If the UnicoWS.dll and the UnicoWS.lib are not installed in `%SystemDrive%\MSLU`, you can specify those locations by setting the `ALT_UNICOWS_DLL_PATH` and the `ALT_UNICOWS_LIB_PATH` for each file.



## Windows Build Environment Variables

> This section describes environment variables that you can set to influence various aspects of the build. Some of these variables are mentioned specifically in the setup and build instructions above. Others you may set at your discretion.
>
> Environment variables may be set in the shell environment or on the GNU make command line.
>
> 
>
> - `PATH`
>
>   Set the `PATH` to contain:The location of the GNU make binaryThe location of the MKS Unix command binaries (see `ALT_UNIXCOMMAND_PATH` below)The location of the JDK bootstrap installation (see `ALT_BOOTDIR` below)The locations of the Windows NT system commands (usually `C:\WINNT` and `C:\WINNT\system32`)In addition, execute the setup utility [`VCVARS32.BAT`](https://soucod.github.io/jdk5/build-windows-i586.html#msvc).
>
> - `ALT_BOOTDIR`
>
>   The location of the JDK 1.4.2 [bootstrap installation](https://soucod.github.io/jdk5/build-windows-i586.html#bootdir).
>
> - `ALT_OUTPUTDIR`
>
>   An override for specifying the (absolute) path of where the build output is to go.
>
> - `ALT_UNIXCOMMAND_PATH`
>
>   An override for specifying the location of the MKS Unix command binaries, needed only if they are not installed in `C:\mksnt\`
>
> - `ALT_COMPILER_PATH`
>
>   An override for specifying the location of the [Microsoft Visual C++](https://soucod.github.io/jdk5/build-windows-i586.html#msvc) compiler. By default the build uses a path based on `MSVCDir`, which is set by the `VCVARS32.BAT` setup utility.
>
> - `ALT_MSDEVTOOLS_PATH`
>
>   An override for specifying the location of the [Microsoft Visual C++](https://soucod.github.io/jdk5/build-windows-i586.html#msvc) development tools. By default the build uses a path based on `MSDevDir`, which is set by the `VCVARS32.BAT` setup utility.
>
> - `ALT_DEVTOOLS_PATH`
>
>   The location of the `zip` and `unzip` binaries. See [Zip and Unzip](https://soucod.github.io/jdk5/build-windows-i586.html#zip) for details.
>
> - `ALT_CACERTS_FILE`
>
>   The location of the `cacerts` file. See [cacerts file](https://soucod.github.io/jdk5/build-windows-i586.html#cacerts) for details.
>
> - `ALT_MOZILLA_PATH`
>
>   The location of the [Mozilla headers and libraries](https://soucod.github.io/jdk5/build-windows-i586.html#mozilla), needed only if building Java Plug-in.
>
> - `ALT_PLUGIN_MSSDK`
>
>   The location of the [Microsoft SDK](https://soucod.github.io/jdk5/build-windows-i586.html#mssdk) development tools, needed only if building Java Plug-in and/or Java Web Start.
>
> - `ALT_DXSDK_PATH`
>
>   The location of the [Microsoft DirectX SDK headers and libraries](https://soucod.github.io/jdk5/build-windows-i586.html#dxsdk).
>
> - `ALT_MSVCRT_DLL_PATH`
>
>   The location of the [msvcrt.dll](https://soucod.github.io/jdk5/build-windows-i586.html#msvcrt).
>
> - `ALT_UNICOWS_DLL_PATH`
>
>   The location of the [Microsoft Layer for Unicode runtime binary path](https://soucod.github.io/jdk5/build-windows-i586.html#mslu).
>
> - `ALT_UNICOWS_LIB_PATH`
>
>   The location of the [Microsoft Layer for Unicode static library path](https://soucod.github.io/jdk5/build-windows-i586.html#mslu).
>
> - `MILESTONE`
>
>   The milestone name for the build (*e.g.* "beta").
>
> - `BUILD_NUMBER`
>
>   The build number for the build (*e.g.* "b27").

Windows Build

1. cd into the `control/make` directory.
2. Start the build with the command:
     `make scsl [ALT_OUTPUTDIR=directory-name-for-output] [MILESTONE=*milestone_name*] [BUILD_NUMBER=*build_number*] [other "ALT_" overrides]`
3. 

> Please be sure to use the GNU version of `make`.

------

| Copyright 2004 Sun Microsystems, Inc., 4150 Network Circle, Santa Clara, California 95054, U.S.A. All rights reserved. | ![Sun](https://soucod.github.io/jdk5/sunlogo64x30.gif) |
| ------------------------------------------------------------ | ------------------------------------------------------ |
|                                                              |                                                        |

```
    
```