# SENSOR_ATMOSPHERIC_PRESSURE
Refering to datasheet BMP280 Digital Pressure Sensor.
3. Functional description
The BMP280 consists of a Piezo-resistive pressure sensing element and a mixed-signal ASIC.
The ASIC performs A/D conversions and provides the conversion results and sensor specific compensation data through a digital interface.

ASIC stands for Application-Specific Integrated Circuit.
Simply put: it's a chip designed to do ONE single thing very well, instead of being a general-purpose chip like a microcontroller or CPU.

In the context of the BMP280, the ASIC is responsible for:
Receiving the analog signal from the piezoresistive element (the physical pressure sensor).
Converting it to digital using an internal ADC (Analog-to-Digital Converter).
Applying compensation using calibration coefficients stored in memory to correct the reading.
Delivering the already processed data through the digital interface (I²C or SPI).

3.9 Data readout
To read out data after a conversion, it is strongly recommended to use a burst read and not address every register individually. This will prevent a possible mix-up of bytes belonging to different measurements and reduce interface traffic.
Data readout is done by starting a burst read from 0xF7 to 0xFC.
The data are read out in an unsogned 20-bit format both for PRESSURE and for TEMPERATURE.
It is strongly recommended to use the BMP280 API, available from Bosch Sensortec for readout and compensation. For details on memory map and interfaces, please consult chapters 3.12 and 5 respectively.
