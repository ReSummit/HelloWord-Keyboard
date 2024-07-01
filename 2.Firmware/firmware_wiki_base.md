# Current Knowledge of tools and bootloaders

## DFU Bootloader
From (here)[https://github.com/xingrz/stm32-dfu-bootloader]

* Originally from xingrz, a bootloader can be flashed to the first 4KB / 16KB of the ROM so the DFU can flash to it
* If using 4KB bootloader, starting flash address is `0x08001000`
* If using 16KB bootloader, starting flash address is `0x08004000`
* You may need to launch Zadig to install the driver for the DFU bootloader as it shows up on Windows as unidentified
* FN key on keyboard is directly connected to STM32, so you must hold this down for DFU to start (also requires unpluging and repluging power, unideal and needs to implement on Keyboard / Dynamic Firmware!)

## General Firmware Information
* ***To use with DFU, you must change your FLASH.ld file so that the FLASH also uses the right start ORIGIN***
* This entire thing is using SW4STM32 and built using CLion
* A way to translate this to STM32CubeIDE / Makefile exists but will break library includes
    * Reference CLion includes to know what to do
* If you plan on using this as is, you must use STM32CubeMX version <=6.5.0 or else it will not be compatible with newer versions
* Also don't forget to install dependencies for Windows (arm-none-eabi from ARM)
* ***Recent HelloWord keyboards are version v1.3, works with GitHub firmware but lacks changes that would be for v1.2***
* ***Request for archival of v1.2 changes if available would be greatly appreciated***

## HelloWord-Keyboard
* Handles debounce and keyboard input
* Keymap found in HelloWord/hw_keyboard.h
    * Here you can also define other keys as well but must define logic somewhere else if key is pressed
* **By default, the touchbar is not used!**
* Logic for keyboard found in `UserApp/main.cpp` under `OnTimerCallback()`
    * Maybe this is where you implement new keys?
* Uses STM32F103 Middle line of MCUs (has DFU)

### RGB
* Handled in `UserCode/main.cpp`
* Located in a while true loop (b/c everything else runs in a separate thread)
* Does not do rainbow effect like new firmware
* Follow format of if statements for RGB control
* You may want reactive RGB lights, can be accomplished using variable triggers between `OnTimerCallback()` and `Main()`

## HelloWord-Dynamic
* Handles e=ink display, OLED, and motor control
* Uses a different microcontroller (STM32F407)
* ***Software to upload e-ink is NOT compatible for some reason!***
* Current code just shows "hello" on the OLED but cycles through three different Motor control modes