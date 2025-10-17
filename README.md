# large-bolt-tests

This is a testsuite containing large binaries for
[BOLT](https://github.com/llvm/llvm-project/tree/main/bolt).

This repository is Arm-specific and includes only tests targeting the AArch64
architecture. All non-AArch64 (e.g., X86) tests have been removed.

## Usage
### Prerequisites
- Install binary tests prerequisites:
```
sudo apt-get install libtinfo5 zstd
```
- Install perf tools:
```
sudo apt-get install linux-tools-`uname -r`
```
### LLVM CMake configuration
Configure LLVM with the `LLVM_EXTERNAL_PROJECTS` and
`LLVM_EXTERNAL_PROJECTS_SOURCE_DIR` cmake flags. Example:

```
$ git clone https://github.com/llvm/llvm-project
$ git clone https://github.com/arm/large-bolt-tests
$ cmake -B bolt-build -G Ninja llvm-project/llvm \
   -DCMAKE_BUILD_TYPE=Release \
   -DLLVM_TARGETS_TO_BUILD="AArch64" \
   -DLLVM_ENABLE_PROJECTS="clang;lld;bolt" \
   -DLLVM_EXTERNAL_PROJECTS="bolttests" \
   -DLLVM_EXTERNAL_BOLTTESTS_SOURCE_DIR=$(pwd)/large-bolt-tests
$ cmake --build bolt-build --target check-large-bolt
```

When this repo is configured as an external project, it will add itself as an
extra target in LLVM named "check-large-bolt". Just build that target to run
this testsuite.

## Pull Request Requirements
Before submitting a pull request (PR) to this repository, please make sure the following requirements are met:
- **Compression:** All binaries must be compressed before being added.  
- **Testing:** Each new binary must have a corresponding test that verifies its functionality.
- **SBOMs:** Each new binary must have an associated Software Bill of Materials (SBOM) file added to the `SBOMs` directory.
