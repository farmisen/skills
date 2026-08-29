## Stack notes — Mobile

### Toolchain & commands
- Platform(s): {{platforms}}; framework {{mobile_framework}} (Swift/Kotlin/RN-Expo/Flutter);
  min OS {{min_os}}. Build/run: `{{run_cmd}}`. Signing/provisioning is owned by
  {{signing_owner}} — never modify entitlements, profiles, or bundle IDs casually.

### Agent failure modes
- Simulator-passing as proof for device-only areas ({{device_only_areas}}: camera, push,
  biometrics, background modes, real-network conditions) — those need a device.
- Ignoring the lifecycle: state lost on backgrounding, rotation, or process death —
  restore paths are part of the feature, not an edge case.
- Blocking the main/UI thread with IO or decoding; jank is a bug.
- Hardcoded dimensions vs safe areas / dynamic type / density buckets.
- Changes that trip store review (private APIs, new permissions without usage strings,
  background modes) shipped as if they were free.

### Verification norm
Proven = builds for every target platform + the changed flow run on simulator/emulator,
on-device for device-only areas, including one lifecycle pass (background → restore).

### Interview add-ons
- Platform(s) + framework? (infer: xcodeproj, build.gradle, app.json, pubspec)
- Min OS versions? (infer: project config)
- Device-only test areas in this app? (camera/push/BLE — sets the device gate)
- Release pipeline: who owns signing, how do store builds happen? (infer: fastlane/eas configs)
