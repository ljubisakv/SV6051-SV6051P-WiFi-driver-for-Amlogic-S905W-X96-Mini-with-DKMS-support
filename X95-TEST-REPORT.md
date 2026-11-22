# SSV6051P testing report — X95 / Armbian

## Test environment

- Device: X95 Android TV box
- Wi-Fi chipset: SSV6051P
- Interface: SDIO
- SDIO ID: 3030:3030
- OS: Armbian 26.11.0 (Bookworm / Debian 12)
- Kernel: 6.12.112-ophub
- Architecture: aarch64

## Driver build and compatibility

The driver source was adapted for Linux kernel 6.12 with these compatibility changes:

- `smac/init.c`: replaced the timer container lookup with `from_timer()`.
- `smac/dev.c`: updated the `ssv6200_config()` function signature to match the kernel mac80211 API.

The module `ssv6051.ko` was successfully built and loaded on the test system.

The build used GCC 15.3.1 and a separately prepared host `modpost` utility.

## Firmware

Firmware files were installed separately from the Armbian firmware repository:

- `ssv6051-sw.bin`
- `ssv6051-wifi.cfg`

The firmware binaries are not included in this source contribution.

The optional SAR firmware was not present. The driver continued to operate with SAR disabled.

## Functional tests

The following results were observed:

- Kernel detected the SSV6051P device.
- Firmware upload and verification completed.
- Calibration completed successfully.
- The `wlan0` interface was created.
- A scan detected six nearby 2.4 GHz access points.
- The device connected to `OpenWrt-2.4Ghz`.
- NetworkManager reported `wlan0` as connected.
- DHCP assigned `192.168.1.217/24`.
- Ping to the local gateway succeeded: 4/4 packets.
- Ping to `1.1.1.1` succeeded: 4/4 packets.
- Ping to `google.com` succeeded: 4/4 packets.
- Wi-Fi worked after reboot with the module configured for automatic loading.

Ethernet remained available during testing.

Observed throughput was approximately 37.6 Mbit/s. This is a test observation, not a claimed maximum speed.

## SDIO clock

The build includes `-DSDIO_USE_SLOW_CLOCK`, which keeps the SDIO clock at 25 MHz in the tested configuration.

This setting was already present in the upstream-derived source and is credited in the project README to @lukas-kuna.

## Scope and limitations

- These results confirm operation on the tested X95 device and kernel configuration.
- DKMS build and automatic rebuild after kernel upgrades have not yet been verified for this kernel/toolchain combination.
- The manual kernel-module build was verified.
- Firmware redistribution and repository-wide licensing require separate review.
