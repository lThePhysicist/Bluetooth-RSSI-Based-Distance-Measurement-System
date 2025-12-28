# Bluetooth RSSI-Based Distance Measurement System using MSP430

## Description
In this project, the **MSP430G2553** microcontroller and a Bluetooth module are used to estimate the distance to a device with a specific MAC address by measuring the **Received Signal Strength Indicator (RSSI)**. The system communicates via **UART** and displays the estimated distance on an LCD screen. The implementation includes **AT command processing**, **interrupt-driven serial communication**, and the conversion of hexadecimal RSSI data into approximate distance values.

## Key Features
* **Target Specific Tracking:** Filters incoming Bluetooth signals to locate a device with a specific MAC address.
* **RSSI-Based Ranging:** Converts raw Hexadecimal RSSI values into decimal format to estimate physical distance.
* **Interrupt-Driven UART:** Utilizes MSP430 hardware interrupts for efficient, non-blocking serial communication.
* **AT Command Parsing:** Automates the configuration and inquiry process of the Bluetooth module.
* **Real-Time Feedback:** Displays the calculated distance and connection status on a 16x2 LCD.

## Hardware Components
* **Microcontroller:** Texas Instruments MSP430G2553 (LaunchPad)
* **Wireless Module:** HC-05 / HC-06 Bluetooth Module (Master Mode)
* **Display:** 16x2 Character LCD (HD44780 Controller)
* **Power:** 3.3V System Voltage

## Software & Tools
* **Language:** Embedded C
* **IDE:** Code Composer Studio (CCS) / IAR Embedded Workbench
* **Communication:** UART (9600 Baud Rate)

## How It Works
1.  **Initialization:** The MSP430 initializes the UART peripheral, LCD interface, and system clocks.
2.  **Scanning:** The system sends standard `AT` commands to the Bluetooth module to initiate a device inquiry.
3.  **Data Processing:**
    * The Bluetooth module returns a list of devices with MAC addresses and RSSI values.
    * The **UART Interrupt Service Routine (ISR)** captures the data stream.
    * The code parses the stream to find the target MAC address.
4.  **Distance Calculation:**
    * Once the target is identified, the corresponding RSSI value (in Hex) is extracted.
    * The value is converted to a distance estimate using a path loss model.
5.  **Display:** The result is formatted and printed to the LCD screen.

## Distance Calculation Logic
The system estimates distance using the Log-Distance Path Loss Model:

`d = 10 ^ ((TxPower - RSSI) / (10 * n))`

* **d:** Estimated distance.
* **TxPower:** RSSI value measured at 1 meter (Calibrated).
* **n:** Path loss exponent (Environmental constant, typically 2-4).

## Pin Configuration (Example)
| MSP430 Pin | Component | Function |
| :--- | :--- | :--- |
| P1.1 (RX) | Bluetooth TX | UART Receive |
| P1.2 (TX) | Bluetooth RX | UART Transmit |
| P2.0 | LCD RS | Register Select |
| P2.1 | LCD EN | Enable Signal |
| P2.2 - P2.5 | LCD D4-D7 | Data Bus |

## Future Improvements
* Integration of a Kalman Filter to reduce RSSI signal noise and improve accuracy.
* Adding an audible alarm (Buzzer) for proximity alerts.
* Optimizing code for MSP430 Low Power Modes (LPM).
