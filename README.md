# Compiled-Java-OpenJDK-23-Windows-ARM64-aarch64

Please mind that this repo is unofficial, it's just me trying to save you some troubles in yourself compiling the openjdk 23 for windows on arm.
I just compiled it myself but if you don't trust my compiled files you can compile it yourself :

Steps to reproduce :
Install openjdk24 windows x64  sous c/Program Files (Arm)/Microsoft/jdk-24 https://jdk.java.net/24/
Install cygwin and his packages via <path to Cygwin setup>/setup-x86_64 -q -P autoconf -P make -P unzip -P zip 
The cygwin installer can be found here : https://www.cygwin.com/install.html

Open cygwin and clone the repo of openjdk : git clone https://git.openjdk.org/jdk

Launch the configure :
./configure --with-boot-jdk="/cygdrive/c/Program Files (Arm)/Microsoft/jdk-24" --openjdk-target=aarch64-w64-mingw32

Hopefully you will see something like :
```
A new configuration has been successfully created in
/home/volko/JDKKKK/jdk/build/windows-aarch64-server-release
using configure arguments '--with-boot-jdk='/cygdrive/c/Program Files (Arm)/Microsoft/jdk-24' --openjdk-target=aarch64-w64-mingw32'.

Configuration summary:
* Name:           windows-aarch64-server-release
* Debug level:    release
* HS debug level: product
* JVM variants:   server
* JVM features:   server: 'cds compiler1 compiler2 epsilongc g1gc jfr jni-check jvmci jvmti management parallelgc serialgc services shenandoahgc vm-structs zgc'
* OpenJDK target: OS: windows, CPU architecture: aarch64, address length: 64
* Version string: 25-internal-adhoc.volko.jdk (25-internal)
* Source date:    1742822830 (2025-03-24T13:27:10Z)

Tools summary:
* Environment:    cygwin version 3.6.0-1.x86_64, 2025-03-18 17:01 UTC; windows version 10.0.26120.3576; prefix "/cygdrive"; root "C:\cygwin64"
* Boot JDK:       openjdk version "24" 2025-03-18 OpenJDK Runtime Environment (build 24+36-3646) OpenJDK 64-Bit Server VM (build 24+36-3646, mixed mode, sharing) (at /cygdrive/c/progra~4/micros~1/jdk-24)
* Toolchain:      microsoft (Microsoft Visual Studio 2022)
* C Compiler:     Version 19.43.34808 (at /cygdrive/c/progra~1/micros~1/2022/commun~1/vc/tools/msvc/1443~1.348/bin/hostx64/arm64/cl.exe)
* C++ Compiler:   Version 19.43.34808 (at /cygdrive/c/progra~1/micros~1/2022/commun~1/vc/tools/msvc/1443~1.348/bin/hostx64/arm64/cl.exe)

Build performance summary:
* Build jobs:     12
* Memory limit:   15727 MB
```
Then compile it :  make images CONF=windows-aarch64-server-release


