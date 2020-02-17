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