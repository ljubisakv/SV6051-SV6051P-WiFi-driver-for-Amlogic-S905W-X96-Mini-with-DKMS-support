# SSV6051P Linux Wi-Fi Driver

An experimental out-of-tree Linux driver project for the
SSV6051P / SSV6200-family SDIO Wi-Fi chipset.

The goal is to document and maintain a working driver adaptation
for Armbian and compatible Linux systems.

## Current status

The driver has been built and tested on the following system:

| Component | Tested configuration |
|---|---|
| Device | X95 Android TV box |
| Operating system | Armbian 26.11.0 (Debian Bookworm) |
| Kernel | 6.12.112-ophub |
| Architecture | ARM64 (aarch64) |
| Wi-Fi chipset | SSV6051P |
| Bus | SDIO |
| SDIO ID | 3030:3030 |
| Wireless band | 2.4 GHz |

### Verified results

- Driver module builds successfully.
- Chip is detected and firmware is uploaded.
- Wireless interface `wlan0` is created.
- Nearby 2.4 GHz access points are detected.
- Connection to a wireless network succeeds.
- Network connectivity and DNS resolution were tested.
- Driver loading after reboot was reported successful.

A throughput result of approximately 37.6 Mbit/s was observed
during testing. This is an observed result, not a verified
maximum-performance benchmark.

## Kernel compatibility work

The source has been adapted for the tested Linux 6.12 kernel.

The currently documented source changes include:

- Use `from_timer()` for the watchdog timer callback.
- Update the `ssv6200_config()` callback signature to match
  the mac80211 API used by the tested kernel.

The Makefile also contains an SDIO slow-clock setting:

`SDIO_USE_SLOW_CLOCK`

This setting was retained for stability during testing on
the tested Meson-based TV box hardware.

## Firmware

Firmware files are not included in this repository.

The driver expects:

- `ssv6051-sw.bin`
- `ssv6051-wifi.cfg`

These files must be obtained separately from a suitable
redistribution source. Their licensing and redistribution
terms must be checked before bundling or redistributing them.

An optional SAR firmware file was not present in the tested
setup. The driver continued to operate without it.

## Build and installation status

A manual module build was successful on the tested system
using the matching kernel build environment and toolchain.

A fully reproducible build procedure and a verified DKMS
installation procedure are still being prepared.

Do not assume that this driver works on every SSV6051P device,
board, kernel version, or Armbian image.

## Original authors and license

The source files retain their original copyright notices,
including:

- South Silicon Valley Microelectronics Inc.
- iComm Corporation

The original source headers specify the GNU General Public
License version 3 or, at your option, any later version.

See [LICENSE](LICENSE).

This project documents community adaptation and testing.
It does not claim ownership of the original driver.

## Contributions and testing

Reports from other devices and kernel versions are welcome.

Please include:
- Device and board model
- Armbian release
- Kernel version
- Architecture
- Relevant `dmesg` output
- Build errors or wireless test results

## Disclaimer

This is an experimental community project. Use it at your
own risk. Keep a working network connection or another
recovery method before changing wireless drivers.
