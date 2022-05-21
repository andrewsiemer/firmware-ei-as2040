# Edge Impulse firmware for AS2040

Edge Impulse enables developers to create the next generation of intelligent device solutions with embedded Machine Learning. This repository contains the Edge Impulse firmware for the AS2040 (a Raspberry Pi RP2040 based development board). This device supports Edge Impulse device features, including ingestion and inferencing.

## Requirements
### Hardware

- AS2040 with USB-C cable
- (Optional) External I2C sensor boards can be daisy chained using the StemmaQT/Qwiic system (I2C1).

### Tools
The below instructions assume you are using macOS. Alternative instructions for those using Debian-based Linux distribution or Microsoft Windows are provided in [Getting started with Pico guide](https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf) (Sections 2.1 and 9.2).

To build firmware, you will need CMake, a cross-platform tool used to build the software, the GNU Embedded Toolchain for Arm, and the pico-sdk. You can install the first two via [Homebrew](https://brew.sh) using the Terminal. 

To start, use Homebrew to install the tools you will need:

```bash
brew install cmake
brew install git
```

**Intel-based Macs:** Install the GNU Embedded Toolchain using this command: `brew install gcc-arm-embedded`.

**M1-based Macs:** You will need to download an older version of the GNU Embedded Toolchain ([10.3-2021.10](https://developer.arm.com/downloads/-/gnu-rm)) and then follow the installation wizard. Once the tool is installed, specify PICO_TOOLCHAIN_PATH environmental variable to point the location of the tools on your system using `export PICO_TOOLCHAIN_PATH=/Applications/ARM/bin`.

Lastly, you'll need the PICO SDK to compile the firmware. Use the commands below to download the SDK and then set the PICO_SDK_PATH environmental variable to specify the location of the SDK on your system.

```bash
cd ~/
git clone --recurse-submodules https://github.com/raspberrypi/pico-sdk
export PICO_SDK_PATH="~/pico-sdk"
```

Alternatively, the PICO SDK can be obtained from https://github.com/raspberrypi/pico-sdk.

## Building the application
Then from the firmware folder execute:
```bash
mkdir build && cd build
cmake ..
make -j4
```

If you want to enable debug build (with lots of printing, specifically in data acquisition and flash read/write parts of the code), run 
```bash
mkdir build && cd build
cmake .. -DDEFINE_DEBUG=ON
make -j4
```

To flash the firmware file (`.uf2`) onto the AS2040 board, connect the device to your computer using a USB-C cable. Next, while holding down the `BOOT` button, press and release the `RUN` button. You should see the device show up as a USB Mass Storage Device named `RPI-RP2`. Drag the `ei_rp2040_firmware.uf2` file from `/build` folder to newly appeared USB Mass Storage Device. After a few seconds the file should be copied to the device and it will then disappear. The device is now ready to go!

## Troubleshooting

- Ubuntu and Debian users might additionally need to also install ```libstdc++-arm-none-eabi-newlib```.

## Refernces

- [Getting started with Pico guide](https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf)
- [Developing C on the RP2040 on macOS](https://wellys.com/posts/rp2040_c_macos/)

