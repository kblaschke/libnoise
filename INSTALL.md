# Build and install instructions

## Prerequisites

libnoise itself has no external dependencies on other C/C++ libraries apart from the STL and
C++ runtime libraries. To build it, some external tools are required though. 

### General

These are the general requirements to build all parts of libnoise on any platform:

#### Building
- CMake version 3.14 or higher
  
#### HTML source code documentation
- Doxygen
- Perl (required by Doxygen)
  
#### Tests
- Googletest

### Windows

To build on Windows, in addition to the general requirements you need to install
Visual Studio. The freely available Community Edition is sufficient.

### Linux

On Linux, use your distribution's package manager to install the essential build
tools and libraries, e.g. the `build-essential` package on Ubuntu.

### macOS

To build on macOS, you need to register an Apple Developer ID on
[developer.apple.com](https://developer.apple.com/) and download Xcode,
which includes the toolchain and SDK files.

# Building

## TL;DR

Quick instructions to build and install the library with optimizations:

```bash
cd /path/to/libnoise
mkdir cmake-build-release
cd cmake-build-release
cmake -DCMAKE_INSTALL_PREFIX=install -DCMAKE_BUILD_TYPE=Release ..
cmake --build . --config Release
```

## Using the command line

These instructions are generic and identical on all platforms. Replace the paths as
you see fit.

### Configuring the build

First, you need to run CMake once to create the build directory and the project files
for your local build system. Open a terminal and `cd` to the top-level directory
containing this file. Also make sure the `cmake` executable is in your PATH.

Then execute the following commands:

```bash
mkdir cmake-build-release
cd cmake-build-release
cmake -DCMAKE_INSTALL_PREFIX=../install -DCMAKE_BUILD_TYPE=Release -DCMAKE_VERBOSE_MAKEFILE=ON ..
```

If all went well, you'll see the output "Build files have been written to ...". Your
build directory is now configured.

#### Windows notes

To specify the Visual Studio SDK version and platform architecture, you may want to
specify a specific generator for Visual Studio. The available generators strongly depend
on your local CMake and the installed Visual Studio and SDK versions. To get a list
of available options you can choose from, run CMake with this command:

```bash
cmake -G
```

Select an appropriate generator from the list and pass it to CMake using the `-G` argument.
Newer Visual Studio generators (2019+) take an additional argument `-A` for the architecture,
older ones provide different generator for each.

For example to build a 64 bit library using VS2019:

```bash
cmake -G "Visual Studio 16 2019" -A Win64 [other arguments]
```

Or build for ARM with VS2017:

```bash
cmake -G "Visual Studio 15 2017 ARM" [other arguments]
```

#### macOS notes

On macOS, you may want to use the `Xcode` generator, which uses `xcodebuild`
instead of `make` and uses automatic code signing. Add `-G Xcode` to the CMake parameters
to select this project file generator.

## Visual Studio

In a recent Visual Studio version, open the top-level `CMakeLists.txt` file. Visual Studio will
automatically configure the project.

For older Visual Studio version, either use the command line instructions above to create
a solution first or run the CMake GUI and use it to configure the project. Then open the
generated .sln file in Visual Studio.

To build, simply use the "Build solution" command.

## CLion

With CLion, just open the `libnoise` directory. CLion will automatically detect the
`CMakeLists.txt` and configure the project using the bundled CMake version.

To build the project, select `Build` -> `Build project` from the menu or use the automatically
generated build configurations to build individual targets.

## Xcode

Xcode has no native CMake support. Use the command line instructions above and add
`-G Xcode` the the CMake arguments to create a project which can be opened in Xcode.

After opening the project, you can build it directly from within Xcode.

# Installing

CMake provides an automatically generated `install` target which will copy the libraries,
includes and the CMake package config files into the installation prefix. Invoking the
`install` target will automatically build the project, so this build command will suffice:

```bash
cmake --build . --target install --config Release
```

In the output directory, the `include` directory will contain all header files (in a `noise`
subdirectory), the shared and static libraries in `lib` and, if Doxygen was found, the HTML
docs in `usr/share/libnoise/doc`.

Additionally, the CMake package config files are located in `lib/cmake/Noise`.

# Using the library

## Non-CMake-based projects

If not using CMake, add the `include` and `lib` directories as include and linker library
search paths respectively. Then link against the `noise` library, e.g. with `-lnoise`.

On most UNIX platforms, you'll need to also link against libm with `-lm`. 

## CMake-based projects

If the library is installed in a system path (or one of CMake's default search paths), no
additional setup is required in the project's CMakeLists.txt.

If the library files are located elsewhere, add the install path (the one containing the
`ìnclude` and `lib` directories) to the `CMAKE_PREFIX_PATH` list.

In the project's CMake file, use `find_package()` to find the library and load the targets:

```CMake
find_package(Noise REQUIRED)
```

The imported package provides two targets either of which can be linked, the shared and
the static library. The targets are named `Noise::shared` and `Noise::static`.

**Note:** If you use libnoise in non-(L)GPL/closed-source projects, only link the shared library!

Example:

```CMake
add_executable(MyExe
        ...
        )

target_link_libraries(MyExe
        PRIVATE
        Noise::shared
        )
```

Linking the target will automatically add the include path, so all sources defined in the target
definition will find the libnoise headers.

On platforms requiring libm, the library will be automatically linked as a dependency.