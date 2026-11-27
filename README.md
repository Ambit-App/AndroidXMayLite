# AndroidXMayLite

**AndroidXMayLite** is the open-source Android JNI / `gomobile` binding wrapper for [XMay](https://github.com/Ambit-App/XMay) (a fork of [XTLS/Xray-core](https://github.com/XTLS/Xray-core)), forked from [2dust/AndroidLibXrayLite](https://github.com/2dust/AndroidLibXrayLite) for use in [Ambit](https://ambitx.org).

- **License:** GNU Lesser General Public License v3.0 ([LICENSE](./LICENSE) + [COPYING.GPL3](./COPYING.GPL3))
- **Third-Party Notices:** [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md)
- **Website:** https://ambitx.org

## Modifications from Upstream AndroidLibXrayLite

- **`go.mod` / `go.sum`:** Points `github.com/xtls/xray-core` to the local `../Xray-core` (`Ambit-App/XMay`) checkout and updates compatible Go dependencies (`quic-go`, `golang.org/x/mobile`, `golang.org/x/*`, `blake3`).
- **`libv2ray_main.go`:** Updated `CheckVersionX()` to return `XMay <version> (<buildHash>)`.
- **`libv2ray_utils.go`:** Added `MeasureOutboundDelayWithTimeout(ConfigureFileContent, url, timeoutMs)` and `measureInstDelayWithTimeout(...)` for bounded context-aware HTTP outbound delay probes.

## Build Requirements

- **JDK** (17 or 21)
- **Android SDK & NDK** (`ANDROID_HOME` / `ANDROID_NDK_HOME` configured)
- **Go 1.27+**
- **gomobile** (`go install golang.org/x/mobile/cmd/gomobile@latest`)

## Reproducible Build Instructions (`libv2ray.aar`)

`go.mod` uses `replace github.com/xtls/xray-core => ../Xray-core`, so clone **XMay** into a sibling directory named `Xray-core`:

```bash
mkdir xmay-workspace && cd xmay-workspace
git clone https://github.com/Ambit-App/XMay.git Xray-core
git clone https://github.com/Ambit-App/AndroidXMayLite.git
cd AndroidXMayLite
gomobile init
go mod tidy -v
gomobile bind -v -androidapi 24 -trimpath -ldflags='-s -w -buildid= -checklinkname=0' -o libv2ray.aar ./
```

## Replacing `libv2ray.aar` (LGPL-3.0 Compliance)

In accordance with **LGPL-3.0 §4**, users have the right to modify this library, rebuild `libv2ray.aar` using the instructions above, and relink or debug modifications to this LGPL-3.0 library. The proprietary license of the Ambit Android client does not restrict your rights under LGPL-3.0 with respect to `AndroidXMayLite`.

## License

This project is licensed under the **GNU Lesser General Public License v3.0 (LGPL-3.0)**, which incorporates the terms of the **GNU General Public License v3.0 (GPL-3.0)**. See [LICENSE](./LICENSE) and [COPYING.GPL3](./COPYING.GPL3).
