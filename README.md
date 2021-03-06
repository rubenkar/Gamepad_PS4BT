
# Gamepad_PS4BT

Arduino library for standard PS3 or PS4 gamepad controller in conjunction with a [Hobbytronics USB Host adapter loaded with PS3/4 Bluetooth software running 
in I2C mode](http://www.hobbytronics.co.uk/usb-host/ps3-ps4-controller-bluetooth). 

![Configuration](/Configuration.png)

**This is a personal project and I am in no way affiliated to Hobbytronics.**

Two example sketches are provided for the receiving MCU - gamepad_RX.ino provides a basic test facility; gamepad_servo.ino illustrates how to control a servo and LED 
using a gamepad joystick and button.

Some of the code has been adapted from the ps4_hex header example provided by Hobbytronics, 
with the intention of providing a comparable gamepad class library to the companion [DFRobot (Leonardo/XBee) gamepad 
library](https://github.com/semuconsulting/Gamepad_DFRobot) and allowing end users to adopt one or other type of controller more or less interchangeably. 

**SEMU Consulting 2018**

## Wiring Connections for receiving device

   * 5V on USB Host board  --> 5V on Arduino
   * 0V on USB Host board  --> GND on Arduino
   * SDA on USB Host board  --> SDA on Arduino (pin A4 on Uno)
   * SCL on USB Host board  --> SCL on Arduino (pin A5 on Uno)
   * USB on USB Host board --> USB Bluetooth dongle

**NB:** if you are stacking multiple I2C devices, ensure:
   * Each device has a unique I2C address. You can use the [I2C_Scanner](https://playground.arduino.cc/Main/I2cScanner/) utility to check current
   I2C device addresses.
   * The SDA and SCL lines have adequate PULL-UP provision. It may be necessary to 
   add 4k7ohm resistors between VCC (3.3V / 5V) and the SDA and SCL lines for stability.

## Enabling I2C mode on the USB Host board

**NB:** the Hobbytronics USB Host boards normally come pre-configured in I2C mode. 
If this has been changed to Serial at any point, it will be necessary to change the 
settings via the USB Host board's UART port, typically via an [FTDI USB-Serial adapter](http://www.hobbytronics.co.uk/prototyping/usb-serial-adapter/ftdi-basic). 
If a suitable FTDI adapter is not available, the configuration can be done using an Arduino UNO as a makeshift USB-Serial adapter.

### Configuring the USB Host board via an FTDI USB-Serial adapter:

Remove any USB device from the Host board. Connect to the FTDI adapter as follows:

   * 5V on FTDI adapter --> 5V on Host board
   * 0V on FTDI adapter --> GND on Host board
   * TX on FTDI adapter --> RX on Host board
   * RX on FTDI adapter --> TX on Host board
   * USB on FTDI adapter --> USB port on PC (make a note of the COM port used)

Connect to the relevant COM port using a terminal utility like PuTTY (Windows) or Screen (MacOS), 
configured for Serial comms on the relevant port at 9600 baud. Type ? or HELP at the 
command prompt - you should see a settings report as follows.

```
USB Host Ps4 Dual Shock Controller v1.04

  DEVICE <value>          - Select Playstation controller that will be connected
   (PS4)                    [PS3|PS4]
  SERIAL <value>          - Set Serial Data Output On/Off
   (ON)                  - [ON|OFF]
  HEX <value>             - Set Serial Data Output Hexadecimal On/Off
   (ON)                  - [ON|OFF]
  BAUD <value>            - Set Serial Port Baud Rate
                            [2400|4800|9600|14400|19200|38400|57600|115200]
  I2C <value>             - Set I2C Address
   (41 )                    [1 - 127]
  RGB <value>             - Set PS4 LED RGB value when connected (PS4 only)
   (0000A0)  
  SYNC                    - Display SYNC how-to information for specified controller   
  HELP or ?               - display help   
```

   1. Enter the command BAUD 57600 and click Send. Then reset the baud rate on the terminal session to 57600 and reconnect.
   2. Enter the command SERIAL OFF and click Send.
   3. Enter the command HEX OFF and click Send.
   4. Then type ? or HELP again and you should see that SERIAL and HEX modes are now both ON.
```
  SERIAL <value>          - Set Serial Data Output On/Off
   (OFF)                   - [ON|OFF]
  HEX <value>             - Set Serial Data Output Hexadecimal On/Off
   (OFF)                   - [ON|OFF]
```

### Configuring the USB Host board using an Arduino as a USB-Serial adapter: 

**(This temporary configuration is only necessary if not using an FTDI adapter)**

   * 5V on Arduino --> 5V on Host board
   * 0V on Arduino --> GND on Host board
   * TX on Arduino --> TX on Host board (i.e. opposite way round to normal)
   * RX on Arduino --> RX on Host board
   * RESET pin on Arduino --> GND on Arduino (this is to bypass the Arduino's internal UART chip so we're talking directly to the Host board)

   1. Remove any device from the USB port of the Host board
   2. Open up the Serial Monitor in the Arduino IDE and set the line mode to "Carriage return" and the baud rate to 9600.
   3. Follow the series of commands listed above in the FTDI configuration section.
   4. Once complete, turn everything off, insert the Bluetooth dongle in the USB Host board and reconnect as per the normal wiring connections shown above.

<!-- START COMPATIBILITY TABLE -->

## Compatibility


MCU                 | Tested Works | Doesn't Work | Not Tested  | Notes
------------------- | :----------: | :----------: | :---------: | -----
Arduino UNO         |      X       |              |             | 
Arduino Micro       |      X       |              |             |
Arduino Zero        |      X       |              |             |
Arduino DUE         |      X       |              |             | 
Teensy 3.2 @ 72MHz  |      X       |              |             | 
Teensy 3.6 @ 96MHz  |      X       |              |             |
Teensy 4.0 @ 600MHz |      X       |              |             |

<!-- END COMPATIBILITY TABLE -->
