# ESPHome Filament Humidity Controller (P/N 000001)
## Overview
This project is for an ESPHome-based 3D printer filament box. The purpose of the PCB is to automatically control a fan which will drive air through dessicant in a 3D printer filament drybox. Using ESPHome allows for easy configuration & automation customization through ESPHome and Home Assistant. It also allows for notifications to be configured when the dessicant has been depleted. The PCB is powered from a USB-C port requiring a 5V, 1A capable USB source. The USB port can be used for flashing the onboard ESP, although after ESPHome firmware has been installed, over the air updates can be used instead.
<p align="center">
  <img src="https://github.com/user-attachments/assets/fe235306-0792-43b7-bbaf-96aedf9506f5" />
</p>

## File Layout
### [KiCAD](KiCAD/)
The KiCAD folder contains the entire KiCAD project. This can be used to modify the board.
### [Manufacturing Documents](Manufacturing%20Documents/)
The manufacturing documents contains all output files of the KiCAD project. These files fully define the finished PCB. The main document is 000001.pdf. This file has notes defining the other files.
### [Calculations](Calculations/)
Calculations & design criteria can be found in the calculations folder. SMath (.sm) files are used to generate the output files in this folder (.pdf).
### [Simulations](Simulations/)
The simulations folder contains LTSpice simulations generated during the project.
## Board Architecture
A block diagram of the board architecture is pictured below. Each block in the diagram is described in this section.

![Humidity Controller Block Diagram](https://github.com/user-attachments/assets/2abd4bbb-881f-4515-8b6b-d2ff0941547a)

### USB-C Connector
The board takes in power from the USB-C connector and runs it to both a 12V boost converter and 3.3V linear regulator. The CC pins are used by the upstream device to communicate how much power can be supplied.
### 12V Boost Converter
The 12V boost converter receives 5V (4.75-5.25V) from the USB-C connector & converts it into 12V for the fan. The boost converter is only active when the enable signal is triggered by the ESP32.
### 3.3V Linear Regulator
The 3.3V linear regulator receives 5V from the USB-C connector and regulates it to 3.3V for the humidity sensor & ESP32.
### Humidity Sensor
The humidity sensor receives 3.3V from the linear regulator & is an I2C slave device on the I2C bus connected to the ESP32.
### ESP32
The ESP32 runs ESPHome firmware with a driver to perform fan control with speed feedback & to receive humidity sensor data. Using the custom humidity sensor & fan drivers, the firmware for the ESP32 can be generated using a standard ESPHome yaml file. The ESP32 interfaces with an ESPHome server allowing for easy configuration of control parameters. Upon power on, the ESP32 reads the voltage of the CC pins to ensure adequate power is available from the USB supply prior to enabling the boost converter.
### Fan Connector
The fan connector is a standard 4-Pin computer fan connector which can be used with any 12V computer fan which has a maximum current draw of 200mA or less.
## TODO
1. Write custom ESPHome driver.
2. Create example yaml for ESPHome using custom driver.
## Future Improvements
To make this product consumer-ready, the following improvements should be considered:
1. Add fuse to 5V input.
2. Add ESD protection on USB & fan connectors.
