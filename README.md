# Compiled OpenJDK 25 for Windows ARM64 (aarch64)

**Disclaimer**: This is an unofficial repository. I’ve compiled OpenJDK 25 (pre-release) for Windows on ARM64 to save you the effort of building it yourself. If you don’t trust my precompiled binaries, follow the steps below to compile it from source.

## Overview
This guide provides instructions to build OpenJDK 25 for Windows ARM64 (aarch64) on a Windows ARM machine (e.g., a Copilot+ PC). The process uses Cygwin and Microsoft Visual Studio 2022, with OpenJDK 24 as the Boot JDK.

*Note*: This builds the latest development snapshot of OpenJDK 25 (as of March 2025), not a stable release. The version string includes `-internal-adhoc` because it’s a custom build from the mainline branch.

## Prerequisites
Before starting, ensure you have the following installed:
1. **OpenJDK 24 (Windows x64)**:
   - Download from [jdk.java.net/24/](https://jdk.java.net/24/).
   - Install to `C:\Program Files (Arm)\Microsoft\jdk-24`.
   - *Note*: The Boot JDK must be x64 due to Cygwin’s x86_64 emulation on Windows ARM.

2. **Cygwin**:
   - Download the installer from [cygwin.com/install.html](https://www.cygwin.com/install.html) (`setup-x86_64.exe`).
   - Install with required packages:
     ```bash
     <path_to_cygwin_setup>/setup-x86_64.exe -q -P autoconf,make,unzip,zip,git
     ```
   - This installs `autoconf`, `make`, `unzip`, `zip`, and `git`.

3. **Microsoft Visual Studio 2022**:
   - Install the “Desktop development with C++” workload.
   - Include the “MSVC v143 - VS 2022 C++ ARM64 build tools” component (or latest version).

## Build Instructions
Follow these steps in a Cygwin terminal to compile OpenJDK 25 for Windows ARM64.

### 1. Clone the OpenJDK Repository
```bash
git clone https://git.openjdk.org/jdk.git
cd jdk
```

*Note*: This clones the latest development branch (OpenJDK 25 as of March 2025). For a specific release like JDK 23, use `git checkout jdk-23+<build_number>` instead (see [jdk.java.net/23/](https://jdk.java.net/23/)).

### 2. Configure the Build
Run the `configure` script with the ARM64 target:
```bash
./configure --with-boot-jdk="/cygdrive/c/Program Files (Arm)/Microsoft/jdk-24" --openjdk-target=aarch64-w64-mingw32
```

- **`--with-boot-jdk`**: Specifies the x64 Boot JDK.
- **`--openjdk-target`**: Sets the target to Windows ARM64.

If you encounter toolchain issues (e.g., Visual Studio not detected), set the environment manually:
1. Open a Windows Command Prompt (CMD):
   ```cmd
   "C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvarsall.bat" arm64
   ```
2. Run `configure` from CMD:
   ```cmd
   C:\cygwin64\bin\bash.exe -c "cd /home/volko/jdk && ./configure --with-boot-jdk=\"/cygdrive/c/Program Files (Arm)/Microsoft/jdk-24\" --openjdk-target=aarch64-w64-mingw32"
   ```

**Expected Output**:
```
A new configuration has been successfully created in
/home/volko/jdk/build/windows-aarch64-server-release
using configure arguments '--with-boot-jdk=/cygdrive/c/Program Files (Arm)/Microsoft/jdk-24 --openjdk-target=aarch64-w64-mingw32'.

Configuration summary:
* Name:           windows-aarch64-server-release
* Debug level:    release
* JVM variants:   server
* OpenJDK target: OS: windows, CPU architecture: aarch64, address length: 64
* Version string: 25-internal-adhoc.volko.jdk (25-internal)
* Toolchain:      microsoft (Microsoft Visual Studio 2022)
* C Compiler:     Version 19.43.34808 (ARM64)
* Build jobs:     12
* Memory limit:   15727 MB
```

### 3. Compile the JDK
Build the JDK image:
```bash
make images CONF=windows-aarch64-server-release
```

- **Duration**: On a 12-core ARM64 machine (e.g., Snapdragon X Elite), expect **20-30 minutes**.
- **Output**: The compiled JDK will be in `build/windows-aarch64-server-release/images/jdk`.

### 4. Verify the Build
Check the JDK version:
```bash
./build/windows-aarch64-server-release/images/jdk/bin/java -version
```

**Expected Output**:
```
openjdk version "25-internal" 2025-09-16
OpenJDK Runtime Environment (build 25-internal-adhoc.volko.jdk)
OpenJDK 64-Bit Server VM (build 25-internal-adhoc.volko.jdk, mixed mode)
```

### 5. Run Basic Tests (Optional)
Test the build with tier-1 tests:
```bash
make test-tier1
```
*Note*: Requires `jtreg` installation. Skip if not needed.

## Troubleshooting
- **Toolchain Error**: If `configure` fails to find Visual Studio, ensure ARM64 build tools are installed and use the CMD method.
- **Boot JDK Issue**: Verify the Boot JDK is x64:
  ```bash
  "/cygdrive/c/Program Files (Arm)/Microsoft/jdk-24/bin/java" -version
  ```
- **Slow Build**: Confirm `Build jobs: 12` in the config summary. Adjust with `make images JOBS=12` if needed.

## Precompiled Binaries
Download my prebuilt OpenJDK 25 for Windows ARM64 by cloning the repo (the files on the repo are the compiled ones, not the source files). Use at your own risk!

## License
This project is governed by the OpenJDK license. See [LICENSE](https://openjdk.org/legal/gplv2+ce.html) for details.
