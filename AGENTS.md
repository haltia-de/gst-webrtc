# AGENTS.md

Guidance for coding agents in this repository. `CLAUDE.md` only includes it.
Project-wide decisions (ADRs), plans and conventions live in
[haltia-de/haltia](https://github.com/haltia-de/haltia) (`docs/conventions.md`
for releases and packages).

## What this is

One APKBUILD (`packaging/alpine/`) that builds gst-plugins-bad with every
feature off except `webrtc` and packages only `libgstwebrtc.so` (webrtcbin)
and `libgstwebrtcnice-1.0.so.*`. Everything else (`libgstwebrtc-1.0`,
`libgstsctp-1.0`, the dtls, srtp and sctp plugins, the typelibs) comes from
Alpine's gst-plugins-bad, so the version must be exactly Alpine's.

## Rules

- `pkgver` is Alpine's gst-plugins-bad version on the image; the
  dependency `gst-plugins-bad~$pkgver` keeps them together. When Alpine
  updates GStreamer, release this package at the new version.
- Build dependency `libnice-dev>=0.1.23` comes from haltia-de/libnice
  through the Haltia apk repository: release libnice first.
- A release is a `vX.Y.Z` tag equal to `pkgver` (with `pkgrel=0`). It
  builds armv7 and aarch64 (`release.yml`). To add an architecture to a
  release already out, run `release.yml` by hand with that `tag` and `arch`.
  x86_64 needs libnice released for x86_64 first.
- After a release, pin it in device-lenovo-b8000h's
  `linux-port/apps/packages.lock`.
- Check on the tablet: `gst-inspect-1.0 webrtcbin nicesrc opusenc pulsesrc`.
- Retire this repository when Alpine's gst-plugins-bad ships webrtcbin.

## Build by hand

```sh
docker run --rm --platform linux/arm/v7 -e WANT_ARCH=armv7 \
  -v "$PWD:/src" -v /path/to/haltia-de/ci:/ci:ro alpine:3.24 sh /ci/build-apk.sh
```

(armv7 on x86 needs qemu-arm in binfmt_misc; before libnice is in the Haltia
repository, install its apk files into the container first.)
