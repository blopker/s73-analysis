# Super73 Decompile Project

The goal of this project is to reproduce a bit-perfect firmware image from C code that reproduces the application.

## Device Description

The N52832 is part of the "Diamond" display device that sits on the handlebars of the Super73 ebike. The display device has a Bluetooth interface that interacts with a phone app. The phone app is able to update settings on the Diamond, as well as get data from it about the bike. The Diamond is also contains a CAN bus interface, however the N52832 communicates with an STM chip on the same PCB over UART to actually talk to the CAN bus.

Via the CAN bus, the Diamond talks with the motor controller to change settings such as the Drive mode and Assist level. It is also able to send firmware updates to the motor controller from the phone app by using the buttonless DFU mode.

The N52832 is also able to send updates to the STM chip via the DFU method. To do this, it saves the firmware to an IS SPI memory chip on the Diamond PCB, then updates the STM chip over UART.

This device can set various settings to the motor controller: Assist, Mode, Light. The mode changes based on what country (US or EU) the motor controller is set to.

## Hardware
Chip: N52832

## Software
SDK: v14.2.0
SoftDevice: 0xa5, S132 v5.1.0
Compiler: gcc-arm-none-eabi-4_9-2015q3
Application Version: 221122
Type: Little-endian

## Firmware Segments
App image size: 0x24fac
0x0 - 0x22fff: SoftDevice
0x23000 - ~0x39fdc: Application Code (RX)
~0x39fde - 0x47fac: Application Data (R)
0x47fad - 0x72fff: Writable data (RW)
0x73000 - 0x80000: Bootloader (RX)
0x7f000 - 0x7f164: Bootloader settings (nrf_dfu_settings_t)

## RAM
RAM access is from 0x20000000 to 0x20010000

## Workflow
Use the binary_ninja_mcp MPC server to interact with Binary Ninja, which has the application section of the firmware dump loaded in (flash-6-221122-0-application.bin). The goal is to go through and annotate the database with as much information as we can figure out. 

- Find and name vector tables
- Find, name, and create accurate signatures for all methods
- Create accurate variable names and types
- Import structs from SDK header files as needed
- Define RO sections
- Add comments when needed to store extra information. Specifically, comment if a function is from the SDK, specify which file they are from.
- Identify and rename global state that lives in RAM, including structs or types used

Additionally, the SDK used to build this applications is in the "sdk" folder. Make sure to point out when functions are likely from the SDK files and examples.
