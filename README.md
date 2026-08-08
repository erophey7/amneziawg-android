# Android GUI for [AmneziaWG](https://amnezia.org/learn-more/31_amneziawg)

**[Download from the Play Store](https://play.google.com/store/apps/details?id=org.amnezia.awg)**

This is an Android GUI for [AmneziaWG](https://amnezia.org/learn-more/31_amneziawg).

## Fork notice

This is a **community fork** of [amnezia-vpn/amneziawg-android](https://github.com/amnezia-vpn/amneziawg-android).
It tracks upstream and adds fixes for kernel-mode tunnels on Android. Changes
are published as pull requests upstream where possible.

### What this fork fixes

- **Kernel-mode tunnels now prefer the AmneziaWG kernel module over userspace
  `amneziawg-go`.** Previously, configurations using AmneziaWG obfuscation
  parameters (Jc, Jmin, Jmax, S1–S4, H1–H4, I1–I5) were always routed to the
  userspace binary, which is not shipped with the app — kernel-mode tunnels
  failed with `amneziawg-go: inaccessible or not found` (exit 127). The
  selection is now kernel-first, with `amneziawg-go` kept only as a fallback
  for devices without the kernel module. (see
  [amneziawg-tools](https://github.com/erophey7/amneziawg-tools) fork).
- **Native tool binaries are now shipped as `libawg.so` / `libawg-quick.so`**
  so that `SharedLibraryLoader` can find them. Upstream built `libwg.so` /
  `libwg-quick.so`, which were never extracted from the APK, causing
  `FileNotFoundException` during tool install and broken kernel mode.
- **Connection status is tracked per tunnel** instead of a single
  `currentTunnel`. With multiple tunnels enabled, the first tunnel's status
  stuck at CONNECTING forever and status updates were applied to the wrong
  tunnel. The backend now polls every running tunnel, reports CONNECTED on a
  fresh handshake, and CONNECTING when a handshake goes stale (180 s, matching
  WireGuard's Reject-After-Time).

## Building

```
$ git clone --recurse-submodules https://github.com/amnezia-vpn/amneziawg-android
$ cd amneziawg-android
$ ./gradlew assembleRelease
```

For this fork:

```
$ git clone --recurse-submodules https://github.com/erophey7/amneziawg-android
$ cd amneziawg-android
$ ./gradlew assembleRelease
```

macOS users may need [flock(1)](https://github.com/discoteq/flock).
