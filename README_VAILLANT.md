# OpenThread Simplelink

This directory contains the platform drivers necessary to run OpenThread on the Texas Instruments CC13XX_CC26XX family of
Connected MCUs. These drivers use the TI SimpleLink™ SDK for the RTOS enabled platform drivers. The example applications are
built with FreeRTOS to enable an environment for the standard device drivers to operate.

For list of supported devices in a release please refer to the [release notes](docs/ti-openthread-release-notes.md).

## Toolchain

In a Bash terminal, follow these instructions to install the GNU toolchain and other dependencies.

```bash
$ cd <path-to-ot-ti>
$ git submodule update --init --recursive
$ ./script/bootstrap
```

\***\*Attention:\*\*** In case of error, restart command  `./script/bootstrap`.

## Building

In a Bash terminal:

```bash
$ ./script/build LP_CC1352P7_4 -DCMAKE_POLICY_VERSION_MINIMUM=3.5 -DOT_DIAGNOSTIC=ON -DOT_APP_CLI=OFF -DOT_APP_NCP=OFF -DOT_FTD=OFF -DOT_MTD=OFF
```

\***\*Attention:\*\*** In case of error, restart command  `cmake --build build`.

## Flash Binaries

If the build completed successfully, the `bin` files may be found in `<path-to-ot-ti>/build/bin/ot-rcp.bin`.

In a Bash terminal:
```bash
$ scp build/bin/ot-rcp.bin  root@192.168.0.4:/lib/firmware/ccxx52
```

In a CCU terminal:
```bash
$ cd /lib/firmware/ccxx52/
$ pkill -9 otbr-agent
$ ./ccxx52frmw-update.sh ot-rcp.bin
$ SERIAL_INTERFACE="/dev/ttymxc2"
$ otbr-agent -d 5 -v -I wpan0 -B eth0 spinel+hdlc+uart://$SERIAL_INTERFACE?uart-baudrate=115200 trel://eth0 &

// Exemple of diagnostic command
$ ot-ctl diag start
$ ot-ctl diag channel 11
$ ot-ctl diag power 20
$ ot-ctl diag send 127 100
```


TODO