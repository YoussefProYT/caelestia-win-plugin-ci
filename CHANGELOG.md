# CI changelog

## Round 1

- Run: `43529899103`
- Failing step: `Configure plugin`
- Error excerpt:

  ```text
  CMake Error at CMakeLists.txt:11: VERSION is not set and failed to get from git
  Could NOT find Qt6ShaderTools (missing: Qt6ShaderTools_DIR)
  Failed to find required Qt component "ShaderTools"
  ```

- Fix: install the available `qtshadertools` module and pass explicit `VERSION=0.1.0` plus the pinned `GIT_REVISION` to the root configure command.
- Commit: 337ba9867f6698faa84aeb9dfaf511a8436f8f18

## Round 2

- Run: `43530399539`
- Failing step: `Build plugin`
- Error excerpt:

  ```text
  cl : Command line error D8021 : invalid numeric argument '/Wextra'
  ninja: build stopped: subcommand failed.
  ```

- Fix: add `04-cmake-msvc.patch`, keeping the existing GCC warning flags for non-MSVC compilers and using `/W4` for MSVC. Update the workflow to apply patch 04.
- Commit: 27ef73c873776584fd4810190614256f137ce06b
