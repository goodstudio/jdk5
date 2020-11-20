| ![Java](https://soucod.github.io/jdk5/javalogo52x88.gif) | Build OverviewJavaTM 2 Platform Standard Edition, v5.0 fcs |      |
| -------------------------------------------------------- | ---------------------------------------------------------- | ---- |
|                                                          |                                                            |      |

------

## Contents

- [The Directory Structure](https://soucod.github.io/jdk5/build.html#directories)
- Building Java 2 Platform Standard Edition, v5.0 fcs
  - [Solaris](https://soucod.github.io/jdk5/build-solaris.html)
  - [Linux](https://soucod.github.io/jdk5/build-linux.html)
  - [Windows-i586](https://soucod.github.io/jdk5/build-windows-i586.html)
- [Testing the Build](https://soucod.github.io/jdk5/build.html#testing)
- [Troubleshooting Build Problems](https://soucod.github.io/jdk5/build.html#troubleshooting)

The Directory Structure

> The source code for the Java 2 Platform Standard Edition, v5.0 fcs is delivered in three sibling directories named `hotspot`, `j2se` and `control`. The `hotspot` directory contains the source code and make files for building the Java(tm) Hotspot Virutal Machine. The `j2se` directory contains the source code and make files for building the JDK runtime libraries, tools and demos. The `control` directory contains make files to build the entire JDK release including building the hotspot VM, staging the VM binaries, and building the JDK runtime libraries, tools and demos. Building using the `control` workspace is the recommended manner of building the JDK.

Building Java 2 Platform Standard Edition, v5.0 fcs

> For step-by-step instructions on building the JDK 5.0, choose a platform:
>
> - [Solaris](https://soucod.github.io/jdk5/build-solaris.html)
> - [Linux](https://soucod.github.io/jdk5/build-linux.html)
> - [Windows-i586](https://soucod.github.io/jdk5/build-windows-i586.html)

Testing the Build

> When the build is completed, you should see the generated binaries and associated files in the
>
>  
>
> control/build/<platform>/j2sdk-image
>
>  
>
> directory of your source installation. In particular, the
>
>  
>
> control/build/<platform>/j2sdk-image/bin
>
>  
>
> directory should contain executables for the JDK tools and utilities. If you set ALT_OUTPUTDIR variable during your build then you should cd to $(ALT_OUTPUTDIR) to find the JDK tools and utilities.
>
> You can test that the build completed properly by using the build to run the various demos that you will find in the `control/build/<platform>/j2sdk-image/demo` directory. For example to run the Java 2DTM technology demo, change directories to `control/build/<platform>/j2sdk-image/demo/jfc/Java2D` and launch the demo with this command:
>
> > ```
> > ../../../bin/java -jar Java2Demo.jar
> > ```

Troubleshooting Build Problems

> A build can fail for any number of reasons. Most failures are a result of trying to build in an environment in which all the pre-build requirements have not been met. The first step in troubleshooting a build failure is to recheck that you have satisfied all the pre-build requirements for your platform.
>
> You can validate your build environment by building the `sanity` target from the `control/make` directory. Any errors listed will stop the build from starting, and any warnings may result in a flawed product build.We strongly encourage you to evaluate every sanity check warning and fix it if required, before you proceed further with your build.
>
> Some of the more common problems with builds are briefly descibed below, with suggestions for remedies.
>
> - Can't find the ODBC library
>
>    
>
>   -- Use the
>
>    
>
>   ALT_ODBCDIR
>
>    
>
>   environment variable to point to the location of the appropriate ODBC library (ISLIodbc 2.11).
>
>   
>
> - If your build machine seems to be overloaded from too many simultaneous C++ compiles, try setting the
>
>    
>
>   HOTSPOT_BUILD_JOBS
>
>    
>
>   environment variable to
>
>    
>
>   1
>
>    
>
>   (or, if you're using a multiple CPU machine, set it to the largest integer that is less than or equal to 1.5 times the number of CPUs).
>
>   
>
> - Error message:
>
>    Your Microsoft Visual C++ compiler predates the 6.0 release
>
>    
>
>   -- (Windows only) To do a build on a Windows system, you must use version 6.0 of Microsoft Visual C++ with Visual C++ Service Pack 3 (
>
>   not
>
>    
>
>   Service Pack 4). If you have version 6 on your path and you still get this error, if may be due to your tools having been mounted via the Solstice NFS client rather than by a local installation. Install the tools locally, update your path, and the problem should go away.
>
>   
>
> - Warning message:
>
>    File `xxx' has modification time in the future.
>
>   Warning message:
>
>    
>
>   Clock skew detected. Your build may be incomplete.
>
>   These warnings can occur when the clock on the build machine is out of sync with the timestamps on the source files. Other errors, apparently unrelated but in fact caused by the clock skew, can occur along with the clock skew warnings. These secondary errors may tend to obscure the fact that the true root cause of the problem is an out-of-sync clock. For example, an out-of-sync clock has been known to cause an old version of javac to be used to compile some files, resulting in errors when the pre-1.4 compiler ran across the new
>
>    
>
>   assert
>
>    
>
>   keyword in the 1.4 source code.
>
>   If you see these warnings, reset the clock on the build machine, run "`gnumake clobber`" or delete the directory containing the build output, and restart the build from the beginning.
>
>   
>
> - Warning message:
>
>    as of release 1.4, assert is a keyword 
>
>   -- See note on clock skew above.
>
>   
>
> - *Error message:*` internal: Space Manager: Trouble writing out table to disk`
>   *Error message:*` make[3]: *** [ad_i486.o] Error 154`
>   (Solaris only) Increase the amount of swap space on your build machine.

------

Copyright 2004 Sun Microsystems, Inc., 4150 Network Circle, Santa Clara, California 95054, U.S.A. All rights reserved.

![Sun](https://soucod.github.io/jdk5/sunlogo64x30.gif)

```

```