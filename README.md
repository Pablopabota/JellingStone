# JellingStone
This repository is part of the [Fieldtracks](https://fieldtracks.org/) project, which aims at creating a tracking system to be used in field exercises by relief organizations.

## Building
First make sure you are cloning the project including its submodules:
```
git clone --recursive https://github.com/FieldTracks/JellingStone.git
```

You will need to setup ESP-IDF and a toolchain for the ESP32 in order to compile the project. For further instructions look [here](https://esp-idf.readthedocs.io/en/latest/get-started/).

To configure the build run `make menuconfig` from the project's root directory and change settings under "JellingStone configuration".

Finally run `make flash` to compile the project and flash it to an attached ESP32. Debug output can be watched with `make monitor` (or run them together with `make flash monitor`).

### Flashing wifi and mqtt credentials to the ESP32
This will not run automatically during `make flash`.

1) Copy `nvs_data.csv.example` to `nvs_data.csv`
2) Edit the credentials
3) Run `make flash_nvs`

NOTE: The path to the certificate must be an absolute path
IMPORTANT: If the offset of the nvs partition gets updated the offset in the Makefile needs to be updated as well

## Release

As of 1.0.0 binary releases are available at [https://fieldtracks.org/JellingStone/release](https://fieldtracks.org/JellingStone/release).
Flashing does not require building the software; however, nvs_data.csv must be converted into a working flash partition. Flashing can be done using esptool:

```bash
esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 115200 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0x1000 bootloader.bin 0x10000 jellingstone.bin 0x8000 partitions.bin
```

Creating and flashing the NVS partition is done using::
```bash
nvs_partition_gen.py --input nvs_data.csv --output build/nvs.bin --size 2457
esptool.py write_flash 0x9000 build/nvs.bin
```


## License
This file is part of JellingStone - (C) The Fieldtracks Project

    JellingStone is distributed under the civilian open source license (COSLi).
    Military usage is forbidden.

    You should have received a copy of COSLi along with JellingStone.
    If not, please contact info@fieldtracks.org

