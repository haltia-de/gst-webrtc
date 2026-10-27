# gst-webrtc

`haltia-gst-webrtc`: GStreamer's WebRTC element (`webrtcbin`, the plugin
`libgstwebrtc.so`) and its ICE library `libgstwebrtcnice-1.0` for Alpine
3.24 armv7 and aarch64, for voice and video calls on the Haltia tablets
(Lenovo YOGA Tablet 10, [device-lenovo-b8000h](https://github.com/haltia-de/device-lenovo-b8000h);
Lenovo Tab P11, [device-lenovo-tb-j606l](https://github.com/haltia-de/device-lenovo-tb-j606l)).

Alpine's gst-plugins-bad 1.28.3 is built without them: they need libnice
0.1.23 or newer, and Alpine 3.24 has 0.1.22. This package builds the same
gst-plugins-bad release against [haltia-de/libnice](https://github.com/haltia-de/libnice)
and ships only those two files. It depends on Alpine's gst-plugins-bad of
the same version (`gst-plugins-bad~1.28.3`), so the two never drift apart:
when Alpine moves to a new GStreamer, this package is rebuilt for it.

Packages come from the signed Haltia apk repository (haltia-de/packages).
A `vX.Y.Z` tag builds both architectures. Run by hand (`workflow_dispatch`
with a released `tag` and `arch`), the release workflow adds an
architecture's packages to a release already out; that is how aarch64
joined `v1.28.3`. x86_64 waits for haltia-de/libnice to publish x86_64.
License: LGPL-2.1-or-later, as GStreamer.
