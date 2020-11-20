| ![Java](https://soucod.github.io/jdk5/javalogo52x88.gif) | Linux Build InstructionsJavaTM 2 Platform Standard Edition, v5.0 fcs | [Build Overview](https://soucod.github.io/jdk5/build.html) |
| -------------------------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
|                                                          |                                                              |                                                            |

------

## Contents

- [Introduction](https://soucod.github.io/jdk5/build-linux.html#intro)
- [Linux System Setup](https://soucod.github.io/jdk5/build-linux.html#setup)
- [Linux Build Tools and Libraries Setup](https://soucod.github.io/jdk5/build-linux.html#buildtools)
- [Linux Build Environment Variables](https://soucod.github.io/jdk5/build-linux.html#environmentvariables)
- [Linux Build](https://soucod.github.io/jdk5/build-linux.html#build)

Introduction

> This README file contains build instructions for the JavaTM 2 Platform Standard Edition, v5.0 (JDK 5.0) Community Source Release. Building the source code for the JDK requires a high level of technical expertise. Sun provides the source code primarily for technical experts who want to conduct research, port the platform to new operating systems and hardware, or add enhancements that require access to platform internals. If you are not a technical professional in one of these categories, this release is probably not for you.

Linux System Setup

> The official build platform for the JDK 5.0 is Redhat Enterprise Advanced Server 2.1. Formerly known as Redhat Advanced Server 2.1.
>
> The minimum recommended hardware for building the Linux version is a Pentium class processor or better, at least 256 MB of RAM, and approximately 1.5 GB of free disk space.
>
> Itanium 64-BIT-ONLY: The official build platform for the itanium 64-bit version of JDK 5.0 is Redhat Enterprise Advanced Server 2.1 - 64Bit Edition. The minimum recommended hardware for building the Linux-ia64 version is a Itanium 2 class processor, at least 1 GB of RAM, and approximately 4 GB of free disk space.
>
> AMD 64-BIT-ONLY: The official build platform for the amd 64-bit version of JDK 5.0 is Suse 8 Enterprise Server - AMD64Bit Edition. The minimum recommended hardware for building the Linux-amd64 version is a AMD opteron class processor, at least 512 MB of RAM, and approximately 4 GB of free disk space.
>
> The build uses the tools contained in `/bin` and `/usr/bin` of a standard installation of the Linux operating environment. You should ensure that these directories are on your `PATH`.

Linux Build Tools and Libraries Setup

> Once the Linux OS is set up, you will need to install additional tools and libraries for building the JDK.

Binutils (32-bit build only)

> The binutils package 2.11.93.0.2-11 or later is required. You can obtain binutils package 2.11.93.0.2-11 from
>
>  
>
> Redhat web site 
>
> . You need to remove the default binutils package shipped with Redhat Enterprise Advanced Server 2.1 and install the required binutils package 2.11.93.0.2-11 or later into the default location.

GCC Compiler

> A GNU gcc compiler version 3.2.2 is required or gcc 3.2.1-7 built with the latest binutils package, version 2.13. This requirement is needed due to the presence of gcc assembler bugs in early versions. You need to remove the default compiler shipped with Redhat Enterprise Advanced Server 2.1 which is 2.96. However if you have a new video card driver, like Geforce 4 it is best to use the same compiler as the kernel was used to build the new module, so either build the modules now or rebuild the kernel using gcc 3.x.
>
> Itanium 64-BIT-ONLY: The GNU gcc compiler version 2.96 is required. This compiler is shipped with Red Hat Enterprise Advanced Server 2.1.
>
> AMD 64-BIT-ONLY: The GNU gcc compiler version 3.2.2 is required. This compiler is shipped with SuSE 8 Enterprise Server (SLES).
>
> The compiler resides in the `/usr/bin` directory. If the compiler resides in a different directory on your machine, then set `ALT_COMPILER_PATH` to point to the location of the GCC compiler binaries. The GCC compiler binaries must also be in the `PATH`.



#### OJI Plug-in library Compiled with GCC 2.9 (32-bit build only)

> To support Mozilla compiled with GCC egcs 2.91.66 on Linux platform, you need to build OJI Plug-in library using GNU gcc/g++ compiler version egcs 2.91.66.
>
> Case i: Using a single build system
> In this case, both default GCC 3.2 and GCC egcs 2.91.66 are made to co-exist in the same build machine. Information on GCC egcs 2.91.66, including download sites, is available on the [GNU Gcc web site](http://gcc.gnu.org/) set `ALT_GCC29_COMPILER_PATH `to point to the location of GCC egcs 2.91.66 binary. If the GCC egcs 2.91.66 compiler resides in the `/localtools/usr/bin` directory then you must set `ALT_GCC29_COMPILER_PATH` to `/localtools/usr/`.
>
> Case ii: Using two build systems
> In this case, you need to build the OJI Plug-in library with GCC egcs 2.91.66 first before you start your primary linux-i586 build. You can build the OJI Plug-in library on another system where GCC egcs 2.91.66 comes as the default compiler such as RH 6.1. You can follow the guidelines below:
>
> - Set up [Mozilla headers](https://soucod.github.io/jdk5/build-linux.html#mozilla)
> - Set up J2SE and Deploy workspaces
> - cd into deploy/make/plugin/adapter directory.
> - Start the OJI Plug-in library compiled with GCC egcs 2.91.66 build with the command
>     `make all`
> - The libjavaplugin_oji.so library will be in the $(OUTPUTDIR)/plugin/i386/ns7-gcc29 directory.
>
> After successfully building the library, copy it to the primary build system and set `ALT_GCC29_PLUGIN_LIB_PATH `to point to the location of that library.

Bootstrap JDK

> You will need access to a JDK 1.4.2 for Linux installation. The 1.4.2 binaries can be downloaded from Sun's [JDK 1.4.2](http://java.sun.com/j2se/1.4.2/download.html) download site. Set `ALT_BOOTDIR` to point to the location of the bootstrap JDK installation.

GNU Make

> GNU make version 3.78.1 or later is required to build the JDK. Information on GNU make, including download sites, is available on the [GNU Make web site](http://www.gnu.org/software/make/make.html). For convenience, place the GNU make binary in the `PATH`.

zip

> The build requires `zip` version 2.2 (November 3rd 1997) or later. --> Set `ALT_DEVTOOLS_PATH` to point to the location of this binary. Information on zip, including download sites, is available on the [info-zip web site](http://www.info-zip.org/).

Motif 2.1

> Motif version 2.1 headers and libraries are required for building the Linux JDK. (Motif 2.2 headers will not work.)
>
> As a convenience, the source bundles include an archive of motif workspace that incorporate a number of JDK-related bug fixes. Target `gnumake scsl `would automatically take care about building our motif workspace and generate motif headers and libraries and use them when it required in the build.
>
> ```
>          +- $(OUTPUTDIR)/
>             +- motif/           (Automatically ALT_MOTIF_DIR set to this directory.)
>                +- include/
>                +- lib/
>                 
> ```



#### Mozilla Headers (32-bit build only)



> Mozilla headers are required for building Java Plug-in. 
>
> Download
>
>  
>
> and unpack the headers into a directory similar to the one shown below, and set the
>
>  
>
> ALT_MOZILLA_PATH
>
>  
>
> environment variable to the absolute path of the top-level directory.
>
> ```
>       +- devtools/          (set ALT_MOZILLA_PATH to this level)
>          +- share/
>             +- plugin/
>                +- mozilla_headers_ns7/
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



#### cacerts

A certificates file named "cacerts" represents a system-wide keystore with CA certificates. In JDK and JRE binary bundles, the "cacerts" file contains root CA certificates from several public CAs (e.g., VeriSign, Thawte, and Baltimore).The source bundles contain a cacerts file without CA root certificates. JDK builders will need to secure permission from each public CA and include the certificates into their own custom cacerts file. Failure to provide a populated cacerts file will result in verification of a certificate chain during runtime.The `ALT_CACERTS_FILE` should be set to point to the location of the populated cacerts file.



#### ALSA (i586, amd64)



> Advanced Linux Sound Architecture
>
>  
>
> version 0.9.1 or later is required for building the JDK.
>
> Most modern Linux distributions ship with ALSA. On rpm-based systems, you can see if ALSA is installed by running this command:
>
> ```
>          rpm -qa | grep alsa
> ```
>
> Both
>
>  
>
> ```
> alsa
> ```
>
>  
>
> and
>
>  
>
> ```
> alsa-devel
> ```
>
>  
>
> packages are needed.
>
> If your distribution does not come with ALSA, you can install pre-built ALSA rpm packages, for example from [Fresh RPMs](http://www.freshrpms.net/). Make sure that you do not link to the static library (`libasound.a`), by verifying that the dynamic library (`libasound.so`) is correctly installed in `/usr/lib`.
>
> If you are unable to find a pre-built ALSA package, you need to build it from the source distribution: Download driver and library source tarballs from [ALSA's homepage](http://www.alsa-project.org/). As root, execute the following commands (you may need to adapt the version number):
>
> ```
>       $ tar xjf alsa-driver-0.9.1.tar.bz2
>       $ cd alsa-driver-0.9.1
>       $ ./configure
>       $ make install
>       $ cd ..
>       $ tar xjf alsa-lib-0.9.1.tar.bz2
>       $ cd alsa-lib-0.9.1
>       $ ./configure
>       $ make install
> ```
>
> Should one of the above steps fail, refer to the documentation on ALSA's home page. Note that this is a minimum install that enables building the JDK platform. To actually use ALSA sound drivers, more steps are necessary as outlined in the documentation on ALSA's homepage.
>
> ALSA can be uninstalled by executing
>
>  
>
> make uninstall
>
>  
>
> first in the
>
>  
>
> ```
> alsa-lib-0.9.1
> ```
>
>  
>
> directory and then in
>
>  
>
> ```
> alsa-driver-0.9.1
> ```
>
> .

Linux Build Environment Variables

> This section describes environment variables that you can set to influence various aspects of the build. Some of these variables are mentioned specifically in the setup and build instructions above. Others you may set at your discretion.
>
> Environment variables may be set in the shell environment or on the GNU make command line.
>
> 
>
> - `PATH`
>
>   Set the `PATH` to contain:The location of the GNU make binaryThe location of the GCC compiler binaries (usually /usr/bin, but see `ALT_COMPILER_PATH`)`/usr/bin``/bin`
>
> - `ALT_BOOTDIR`
>
>   The location of the JDK 1.4.2 bootstrap installation.
>
> - `ALT_OUTPUTDIR`
>
>   An override for specifying the (absolute) path of where the build output is to go.
>
> - `ALT_UNIXCOMMAND_PATH`
>
>   An override for specifying where the Unix `/bin` commands are located. You usually do not need to set this variable: the default location is `/bin`.
>
> - `ALT_USRBIN_PATH`
>
>   An override for specifying where the Unix `/usr/bin` commands are located. You usually do not need to set this variable: the default location is `/usr/bin`)
>
> - `ALT_COMPILER_PATH`
>
>   An override for specifying the location of the GCC compiler. See [GCC Compiler](https://soucod.github.io/jdk5/build-linux.html#gcc) for details.
>
> - `ALT_GCC29_COMPILER_PATH`
>
>   An override for specifying the location of the GCC 2.9 compiler. See [OJI Plug-in library Compiled with GCC 2.9 ](https://soucod.github.io/jdk5/build-linux.html#gcc)for details.
>
> - `ALT_GCC29_PLUGIN_LIB_PATH`
>
>   An override for specifying the location of the OJI Plug-in library compiled with GCC 2.9. See [OJI Plug-in library Compiled with GCC 2.9 ](https://soucod.github.io/jdk5/build-linux.html#gcc)for details.
>
> - `ALT_DEVTOOLS_PATH`
>
>   The location of the gnumake binary. See [gnumake](https://soucod.github.io/jdk5/build-linux.html#gnumake) for details.
>
> - `ALT_CACERTS_FILE`
>
>   The location of the `cacerts` file. See [cacerts file](https://soucod.github.io/jdk5/build-linux.html#cacerts) for details.
>
> - `ALT_MOTIF_DIR`
>
>   The location of the Motif 2.1 headers and libraries. See [Motif 2.1](https://soucod.github.io/jdk5/build-linux.html#motif) for details.
>
> - `ALT_MOZILLA_PATH`
>
>   The location of the Mozilla headers. See [Mozilla Headers](https://soucod.github.io/jdk5/build-linux.html#mozilla) for details.
>
> - `MILESTONE`
>
>   The milestone name for the build (*e.g.* "beta").
>
> - `BUILD_NUMBER`
>
>   The build number for the build (*e.g.* "b27").

Linux Build

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