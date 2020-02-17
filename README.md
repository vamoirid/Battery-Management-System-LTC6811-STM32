# Battery-Management-System-LTC6811-STM32
This repository contains a Battery Management System slave board using the LTC6811 Battery Monitoring IC and STM32F446RE Microcontroller. The system was designed during the 2nd year (2018-2019) of my participation in [Aristotle University Racing Team Electric - Aristurtle](www.aristurtle.gr) as the Low Voltage Chief Engineer and also Electrical System Officer of the Electric Vehicle. 

## System characteristics
The vehicle has an Accumulator Container which consists of the Battery Cells, BMS Slave boards, electrical safety circuits, Precharge circuit, BMS Master, HV Measurements, HV Relays and HV Fuse. The cells are distributed in 7 segments of 36 cells each in a configuration of 12s3p in each segment contributing to a **total of 252 cells in configuration 84s3p**. Each cell had a nominal voltage at **3.7V** and maximum voltage at **4.25V** resulting in a **total maximum voltage of the accumulator at 357V**. This board is designed in order to fit exactly on top of each segment resulting in a total number of 7 of these boards inside the Accumulator. 

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/thetis.JPG">

## Schematic Overview

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_Main.png">

The Schematics of the PCB are divided into sections with respect to their responsibilities. There are 4 main connectors on the PCB and 5 secondary. **H1 4-pin** & **H2 4-pin** interconnect the 7 segments carrying the Power Supply and CAN signals while **H3 6-pin** have the signals appropriate for programming of the MCU and **H4 14-pin** carries the 13 cell tabs outside of the segment for further use. The further schematics are:

* **BMS_CAN**: Isolates galvanically the CAN signals.
* **BMS_PowerManagement**: Isolates the Input Power and supplies the various Low Voltage Circuits on the High Voltage side of the PCB.
* **BMS_Microcontroller**: The MCU that controls the LTC6811.
* **BMS_BatteryStackMonitor**: The LTC6811 circuit.
* **BMS_CellCircuit**: A _BMS_BatteryStackMonitor_ sub-schematic which is used for every cell.
* **BMS_BatteryThermistors**: Thermistors which are used for temperature monitoring.
* **BMS_SafetyCircuit**: All the fuses and switches which are used for safety.
* **Cells_Unfused**: it is not really a sub-schematic but these are the holes which transfer the voltage measurements from the cells. M4 bolts with positive locking.

## Safety Circuit

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_SafetyCircuit.png">

The safety Circuit has the unfused voltage measurements as **input**. The paths use 13 fuses at 2A medium-blow in order to fuse the measurement as it is required by the _FSAE Rules_. Apart from this, switches are used for 2 reasons. The first reason is in order to be able to cut-off the measurement of any cell and see if the LTC6811 is going to generate an error as it is required by the _FSAE Rules_. The second reason is that during the assembly, if the PCB does not make a good contact with the Cell Tabs and M4 bolts then this could generate spikes that can harm the LTC6811 so these switches are used to limit the cell voltages before the switches. The first **output** is the cell voltages **before** the switches which is directed to **H4 14-pin** connector outside of the Accumulator and the second **output** are the cell voltages **after** the switches which are routed to the rest of the PCB. The other **2 outputs** are the most positive voltage of the segment and the most negative voltage of the segment respectively.

## Battery Stack Monitor 

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_BatteryStackMonitor.png">

The **inputs** of this circuit are the **outputs** of the **Safety Circuit**. Most of the components on this schematic are taken from the [Analog Devices LTC6811 Dev-Board](https://www.analog.com/en/design-center/evaluation-hardware-and-software/evaluation-boards-kits/dc2260a.html). The **outputs** of this circuit are the **SPI Communication** pins for the communication with the MCU and the **Address pins** of the LTC in order to use in the software. The rest are inside the **BMS_CellCircuit**.

## Cell Circuit

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_CellCircuit.png">

These are all the components that are used for every cell in the segment. The **inputs** of course are the positive and the negative tab of each cell. A Ferrite-Bead is used for each voltage measurement as it is suggested by the Dev-Board. Analog Devices proposes 2 filters one with the C200 capacitor and one with the C201 capacitor with respect to either faster measurements or more accurate measurements. There is also a MOSFET for **Active Dissipative Balancing** with 3 0612 resistors connected in parallel. There is also a LED indicator for showing whether a cell is discharging or not. The **outputs** are the filtered Voltage measurement, the Gate of the MOSFET and the capacitor pin to be used in the 2nd proposed Analog Devices' filter.

## Thermistors

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_BatteryThermistors.png">

There are 5 thermistors used for Battery Temperature Monitoring. These Thermistors are connected with **2-pin** right angle connectors with pull-up resistors. These measurements are monitored by the MCU in order to be done simultaneously with the LTC6811 measurements in order to achieve higher speeds. If there is any implausibility in the temperature measurement connection, the pull-up resistor will ensure that the measurement will be driven HIGH so the MCU will generate an error. 

## Microcontroller Unit

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_Microcontroller.png">

The MCU is the STM32F446RET6U with 64-pins and 180MHz clock. Almost every one of the components are taken from the ST official schematic for the respective Nucleo-Board. The signals that are processed by the MCU are:

* CANTX and CANRX signals with the CAN Transceiver
* SWD signals for the programming of the MCU
* SPI signals for communication with the LTC6811
* Thermistors' signals for the temperature measurements
* Address signals for software manipulation
* Voltage Monitoring of the output of the Isolated DC/DC Converter

## CAN

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_CAN.png">

Each segment inside the Accumulator Container communicates with the BMS-Master via CAN protocol. This is done because it is requiring only 2 wires instead of 4 for the SPI and because it is also used in every other part of the car that interfaces an MCU. The CAN Transceiver is the **ISO1050DWR** from **Texas Instruments**. It is an Isolated CAN Transceiver that is used in applications requiring galvanic isolation between the input and the output signals. The components used in the bus can be used for testing but inside the car only 2 120 Ohms resistors can be used in the whole bus.

## Power Management

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/BMS_PowerManagement.png">

The most controversial circuit of the board is the **Power Management** circuit. There are 2 main ways in order to design the Power Distribution of the board. Either directly from the batteries just like the LTC6811 or through the Low Voltage System (LVS) of the car. There were plenty advantages and disadvantages for both ways. 

#### Power from the Low Voltage 

This is the design that was followed for this board. The main **advantage** is that the board does not consume any power when the LVS is off (Car is off). This means that the Accumulator can be left alone and not worry if the batteries are drained or not. The main **disadvantage** is the fact that except for the Isolated CAN Transceiver, we also need to use an Isolated DC/DC converter to transfer the power from the LVS to the HVS of the PCB.

#### Power directly from the Batteries

The other design is a little bit more controversial but needed a lot more time, studying and testing in order to be fully functional and safe. One **disadvantage** of this is that there are not many voltage converters that can convert 36-51V to 5V with almost 500mA of current in the output and also be in a reasonable size and cost. If we overcome this problem somehow then we reach to the second and a lot more important **disadvantage** of this board. If the MCU is directly supplied by the Batteries, this means that the MCU will always be working even if the LVS is off or even if the Accumulator Container was out of the car. This is a huge problem because the batteries would need a lot more times to be charged because even though an MCU draws too little current compared to the total capacity of the Accumulator Container but 7 of them in a 24 hour basis means that the container could not be left without charging for several weeks. Moreover an Isolated CAN Transceiver with also Isolated Power Supply should be used in order to power the Isolated side of the IC but that is not a problem because such there are plenty of such ICs. The amazing **advantage** of this board is that it would need only 1 IC to interface with the LVS side, meaning a lot less components on this side but the most important feature is that it would need only 2 wires (CANH, CANL) for interconnection with the segments and the BMS-Master.

## Conclusion

The PCB was fully functional inside and outside of the vehicle after a lot of testing. There are a lot of thing that could be done to be improved such as getting Power directly from the Batteries but somehow disabling the MCU when there are no CAN messages to reach the Power consumption to almost 0 A. If someone has been helped by this project, do not forget to mention me and also if you find a way to improve it I would be glad to hear it!

#### Images

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/PCB_2D_All.png">

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/PCB_2D_Top.png">

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/PCB_2D_Bottom.png">

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/PCB_3D_Front.png">

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/PCB_3D_Back.png">

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/PCB_Real_Front.png">

<img src="https://github.com/vamoirid/Battery-Management-System-LTC6811-STM32/blob/master/images/PCB_Real_Side.png">