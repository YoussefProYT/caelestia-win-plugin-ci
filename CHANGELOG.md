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
