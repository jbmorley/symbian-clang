Symbian Toolchain includes the lastest compiler and techniques that help you
in Symbian C++ development. Including in this are:
- Clang (8.0.0) with support for Symbian target ELF (arm-nokia-symbian-eabi)
- CMake modules that help you easily add and make your executable, make an SIS package.

## Requirements:
- SDK installed. On Linux, install it using Wine. Only the native Symbian link libraries
are used. Also on Linux, requires additional to install *arm-none-symbianelf* native
toolchain.
- CSL toolchain with binutils for GCC 4.x or upper

## Symbian driver:
- The driver uses CSL toolchain's **arm_none_symbianelf_as** and **arm_none_symbianelf_ld**
to assemble and link to Symbian ELF. Make sure you have CSL toolchain added to **PATH** environment variable.
- The driver also lurks for the *EPOCROOT* environment variable to get the root of the SDK. If you don't
have it, you can specified the root through the compiler argument: **--epocroot=<path_to_epoc_root>**
- Driver will also lurks for *EPOCLIBSEARCHDIR* to get additional linking libraries. This variable was used
to get *libsupc++* and *libgcc*, but this should be decrepated.

## Symbian vs morden EABI exception:
- Due to GCC issue, GCCE only supports with DWARF exception handling, while ARMCC supports EHABI(?).
To compatible with morden compiler, a newer binutils and libgcc must be provided. GCC 4.x binutil for
Symbian is needed.
- Clang will build EPOC dll/executable with libgcc.a and libgcc_eh.a

## Building
- Make LLVM's cmakelist and make clang (llvm/tools/clang)
- After that, run build_unwind script. A libunwind.a should be procedued in **libunwind/build**
- The install folder contains clang executable after build. Put that anywhere you want
- Next, create two environment variable: EPOCROOT (points to the SDK folder) and EPOCLIBSEARCHDIR (CSL toolchain
libraries folder). 
- Copy libunwind.a into the CSL toolchain libraries folder.
- Try to build the example to see if its work. Note that export CC and export CXX must point to the Clang exectable
that you just build.

## Native usage of Clang with CMake on Windows
- It sucks. I don't make a build file on other platforms is because it doesn't have problem with vanilla clang like on Windows.
CMake tied Win32 with Clang x MSVC, so i have to use some trick to get around that.
- Make sure to set the following variable when passing to CMake to get clang with CMake:
  - CMAKE_C_COMPILER_ID and CMAKE_CXX_COMPILER_ID to Clang x.x.x
  - CMAKE_SYSTEM_NAME to Linux