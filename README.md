# Battery-Management-System-LTC6811-STM32
This repository contains a Battery Management System slave board using the LTC6811 Battery Monitoring IC and STM32F446RE Microcontroller. The system was designed during the 2nd year (2018-2019) of my participation in [Aristotle University Racing Team Electric - Aristurtle](www.aristurtle.gr) as the Low Voltage Chief Engineer and also Electrical System Officer of the Electric Vehicle. 

## System characteristics
The vehicle has an Accumulator Container which consists of the Battery Cells, BMS Slave boards, electrical safety circuits, Precharge circuit, BMS Master, HV Measurements, HV Relays and HV Fuse. The cells are distributed in 7 segments of 36 cells each in a configuration of 12s3p in each segment contributing to a **total of 252 cells in configuration 84s3p**. Each cell had a nominal voltage at **3.7V** and maximum voltage at **4.25V** resulting in a **total maximum voltage of the accumulator at 357V**. This board is designed in order to fit exactly on top of each segment resulting in a total number of 7 of these boards inside the Accumulator. 

