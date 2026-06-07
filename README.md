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
