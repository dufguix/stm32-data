# Trying to get STM32C071 working

Check:
- [stm32 hal](https://github.com/embassy-rs/embassy/blob/main/embassy-stm32/README.md)
- [stm32 data](https://github.com/embassy-rs/stm32-data)
- [stm32 sources](https://github.com/embassy-rs/stm32-data-sources)

Notes:
- clone https://github.com/embassy-rs/stm32-data
- ./d download-all. this will create a clone of https://github.com/embassy-rs/stm32-data-sources in the sources directory
- download/install latest stm32cubemx 6.13.0
- look at its installation folder for the mcu directory, put it in sources/cubedb replacing the old one
    copy Home/STM32CubeMX/db/mcu folder
    paste/replace to stm32-data/sources/cubedb/mcu
- you need to get the C headers and put them in sources/headers
    download it within cubemx. or https://github.com/STMicroelectronics/cmsis-device-c0/tree/main/Include
    copy it from HOME/STM32Cube/Repository/Drivers/CMSIS/Device/ST/STM32C0xx/Include
    dont take stmc0xx.h and system_stm32c0xx.h files
- run ./d gen 
    need gcc compiler 
- fix errors that pop up
    - add some regex to chips.rs/chip_name_from_package_name function
    - maybe add chip to chip.rs/NOPELIST array, add a comment with TODO for later 
    - add memory map to memory.rs/MEMS RegexMap
    - add rcc to perimap.rs
- now ./d gen complete. Take a look at ./build/data for generated json files.
    - check them, compare with others
- run stm32-metapac-gen
    - output the PAC in ./build/stm32-metapac
- clone embassy-stm32
- can edit embassy-stm32/Cargo.toml
    - change stm32-metapac in dependencies (in two places) to path = "...." to point to the one you just created
    - add cargo features for the new chips
- try to build a new project for the new chip
    you might get build failures due to stuff being wrong in the jsons, in which case you have to fix it (usually in stm32-data-gen), regenerate jsons, regenerate metapac, try building again
