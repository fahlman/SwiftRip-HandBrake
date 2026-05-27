# SwiftRip HandBrake Fork

This fork tracks upstream HandBrake releases for SwiftRip's bundled `HandBrakeCLI`.

The fork may automatically build and release SwiftRip-patched HandBrakeCLI candidate
artifacts when upstream HandBrake publishes a new stable release. SwiftRip itself
does not float to those artifacts automatically. The `SwiftRip-Tools` repository
pins the exact SwiftRip HandBrake tag and commit that the app release should use.

## Release Flow

1. The `SwiftRip HandBrake Release` workflow checks the latest stable upstream
   HandBrake tag.
2. If this fork does not already have `swiftrip-handbrake-<version>`, the workflow
   creates `swiftrip-<version>` from the upstream tag.
3. The workflow verifies the libdvdread hardened-runtime patch and changes its
   libdvdcss lookup path to:

   ```text
   @executable_path/../Frameworks/libdvdcss.2.dylib
   ```

4. The workflow builds `HandBrakeCLI` for `arm64` and `x86_64`.
5. The workflow publishes those artifacts as a release on this fork.

To ship a candidate in SwiftRip, update `SwiftRip-Tools` deliberately:

- `HANDBRAKE_VERSION`
- `HANDBRAKE_SWIFTRIP_TAG`
- `HANDBRAKE_SWIFTRIP_COMMIT`
- artifact checksums, if publishing new SwiftRip-Tools artifacts

That separation is intentional: this fork can prepare candidates automatically,
but SwiftRip only consumes a version after we choose and pin it.
