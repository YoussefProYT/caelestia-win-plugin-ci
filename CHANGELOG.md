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

## Round 3

- Run: `43530807981`
- Failing step: `Build plugin`
- Error excerpt:

  ```text
  windows/qalculator.cpp(6): error C2440: 'return': cannot convert from 'const char16_t [4]' to 'QString'
  windows/qalculator.cpp(11): error C3445: copy-list-initialization of 'std::pair<QString,QString>' cannot use an explicit constructor
  windows/qalculator.cpp(13): error C2664: cannot convert argument 2 from 'initializer list' to 'const std::pair<QString,QString> &'
  ```

- Fix: use `QStringLiteral("N/A")` and explicit `QPair<QString, QString>` construction in the Qalculator Windows stub.
- Commit: aa61ce84d1ee6c4bc742e71b825fce532012418c

## Round 4

- Run: `43531216252`
- Failing step: `Build plugin` (link)
- Error excerpt:

  ```text
  qalculator.cpp.obj : error LNK2001: unresolved external symbol Qalculator::metaObject
  qalculator.cpp.obj : error LNK2019: unresolved external symbol Qalculator::resultChanged
  lib\\caelestia-core.dll : fatal error LNK1120: 6 unresolved externals
  ```

- Fix: add `05-qalculator-moc.patch`, listing `qalculator.hpp` explicitly in the `caelestia-core` QML module sources so AUTOMOC generates the Qalculator meta-object code.
- Commit: 0613387e2f3f478e85d7441b120f067a2d3eb0c9

## Round 5

- Run: `43531670512`
- Failing step: `Build plugin` (link)
- Error excerpt:

  ```text
  CircularBuffer::staticMetaObject: unresolved external symbol
  Settings::Node::setValue: unresolved external symbol
  lib\\caelestia-components.dll : fatal error LNK1120: 4 unresolved externals
  lib\\caelestia-state.dll : fatal error LNK1120: 23 unresolved externals
  ```

- Fix: add `06-windows-export-symbols.patch`, enabling `WINDOWS_EXPORT_ALL_SYMBOLS` for each backing QML module target when using MSVC.
- Commit: 781af8638935859e32b460035076020a79bf8bb3

- Follow-up: corrected the patch hunk context after local `git apply --check` reported a malformed patch.
- Follow-up commit: 0abe038601b99c95cf408b881e9b771bcadaff33
- Final patch regeneration: generated patch 06 from the exact source file and revalidated ordered application through patch 06.
- Final patch regeneration commit: f97beec016710d44d971d10e9be8e904c935c6ef
