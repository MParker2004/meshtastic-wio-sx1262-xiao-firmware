# meshtastic-wio-sx1262-xiao-firmware
Custom Meshtastic firmware for XIAO ESP32S3 + standalone Wio-SX1262 (pin header version) - remapping pin headers

# Custom Firmware for XIAO esp32s3 and standalone wio-SX1262

Custom firmware for use with the XIAO esp32s3 microcontroller and the wio-SX1262 LoRa chip - remapping header pins to align with the standalone stacking as opposed to the proprietary connector on the kit version.

Key Difference:

The wio-SX1262 sold in the “XIAO esp32s3 LoRa Kit” uses a proprietary snap fit connector to communicate with the microcontroller and allow for transmission of LoRa messages and signals. Whereas, the stand-alone wio-SX1262 chip does not have this connector, instead opting for a pin header system. 

This means that the mapping of pins used to communicate between the two chips is misaligned when using the standard “Seeed XIAO esp32s3 LoRa kit” flash file on meshtastic.

The Fix:

The only files in the firmware changed in the fix are “pins_arduino.h” and ”variant.h”, these are the files that contain the information for pin mapping between the two units.

Pin changes:

(in “pins_arduino.h”) 
static const uint8_t SS = 5; // was 41

(in “variant.h”)
#define LORA_CS 5 // was 41 
#define LORA_RESET 3 // was 42 
#define LORA_DIO1 2 // was 39 
#define SX126X_BUSY 4 // was 40 
#define SX126X_RXEN 6 // was 38

How to flash:

The custom firmware file is located in the project repository here. Download the .bin firmware file, install esptool in cmd, shell or terminal, ensure that the XIAO is in bootloader mode (hold the B button as you power on) and then run the flash command: 

“python3 -m esptool --chip esp32s3 --port YOUR_PORT --baud 921600 write-flash 0x0 firmware-seeed-xiao-s3-2.7.21.d3a8684.factory.bin”

Mac: YOUR_PORT = something like /dev/cu.usbmodem101 — find it with ls /dev/cu.*
Linux: YOUR_PORT = something like /dev/ttyUSB0 — find it with ls /dev/tty*
Windows: YOUR_PORT = something like COM3 — find it in Device Manager

After flashing, install the “Meshtastic” app, connect to the device via BLE (the standard Meshtastic password is 123456) and then set your frequency to your region (868MHz for me).

Hardware used: (all purchased from PiHut UK)

- XIAO esp32s3
- Wio - SX1262
- 868MHz radio antenna
- 2.4GHz wifi/ BLE antenna

NOTE: This is specifically for the standalone version of the Wio chip with the pin headers, as opposed to the snap connector Wio sold in the XIAO LoRa kit.

Credits:

Credit to GitHub issue #6478 and user “puflet” for the origional solution on the listed issue post, and the correct pin mapping suggestions! (https://github.com/meshtastic/firmware/issues/6478)

Credit to GitHub user “MParker2004” for compiling and testing the custom firmware with the listed hardware.
