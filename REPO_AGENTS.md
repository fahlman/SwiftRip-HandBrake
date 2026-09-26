# SwiftRip-HandBrake — Repository Instructions

Read the shared `AGENTS.md` first. This repository is the HandBrake source fork
used to build SwiftRip's bundled HandBrakeCLI, not the SwiftRip application.

## Upstream and integration boundaries

- Integrate SwiftRip fork changes, including these guide files, into
  `swiftrip-1.11.2` on `fahlman/SwiftRip-HandBrake`. The tools repository pins
  that fork line; `master` is not the integration base for this migration.
  Keep published source tags immutable.

- Read `README.markdown`, `CONTRIBUTING.md`, and the upstream source documentation.
  Preserve the existing cross-platform languages, configure/build system, and
  upstream layout; general SwiftUI defaults do not authorize rewriting this fork.
- The SwiftRip-specific libdvdread patch is
  `contrib/libdvdread/A03-macOS-hardened-runtime-dlopen.patch`. It loads
  `@executable_path/../Frameworks/libdvdcss.2.dylib` to match SwiftRip's bundle.
- Keep app-specific changes narrow and reviewable against the upstream revision.
  Retain upstream license files and notices.
- SwiftRip-Tools owns artifact construction, architecture checks, packaging,
  and the exact fork revision consumed by SwiftRip. A local source edit is not
  a published tool update; coordinate source pins and releases through that repo.

## Verification

- `.github/workflows/mac.yml` describes the upstream macOS toolchain and build
  sequence. Its full application build is different from the HandBrakeCLI-only
  build in SwiftRip-Tools' `Scripts/build-handbrakecli.zsh`.
- Use the affected build path for a source change; preserve both Apple Silicon
  and Intel support when changing the bundled tool. Do not invent a SwiftPM or
  app-project test gate for this source fork.
- Keep published source tags available for the binaries they identify. Publishing
  an updated fork tag and rebuilding the consumer are separate release actions.

## Toolchain and platforms

Configuration inspected on 2026-09-25; this is an upstream C/C++/Objective-C
build with its existing support tools, not a Swift package. Swift compiler and
Swift language-mode requirements do not apply.

- Selected local tools: Xcode 27.0 (`27A266a`), Apple Clang 21.0.0
  (`clang-2100.3.34.2`), macOS SDK 27.0. These are observations, not upstream pins.
- SDK: the macOS project selects `macosx` without an exact SDK version.
- Deployment settings vary by build path: `macosx/HandBrake.xcodeproj/project.pbxproj`
  contains macOS 10.13 settings; `macosx/xcconfig/base/os.osx1013.xcconfig`
  specifies 10.13.4. Check the chosen target/configuration before claiming a minimum.
- SwiftRip's CLI build uses `../SwiftRipTools/Scripts/build-handbrakecli.zsh`
  in the sibling tools repository with `--disable-xcode`; its generated
  configuration determines the CLI's target, not the GUI project's setting.

Preserve these upstream and consumer contracts during documentation work.
Follow the shared latest-stable policy where applicable; toolchain or deployment
upgrades are separate tasks.
