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

Burst Read means reading several memory registers continuously and all at once, instead of going register by register.

How does it normally work without burst read?
Without burst read, you would do something like this:

"Hey BMP280, give me register 0xF7" → wait for response
"Hey BMP280, give me register 0xF8" → wait for response
"Hey BMP280, give me register 0xF9" → wait for response
"Hey BMP280, give me register 0xFA" → wait for response
"Hey BMP280, give me register 0xFB" → wait for response
"Hey BMP280, give me register 0xFC" → wait for response

That's 6 separate transactions. Slow and risky. 

How does it work WITH burst read?
With burst read, you would do:

"Hey BMP280, start from 0xF7 and give me 6 bytes in a row" → you receive everything at once 

The sensor automatically sends you 0xF7, 0xF8, 0xF9, 0xFA, 0xFB, 0xFC without you having to request each one.

Why does the datasheet strongly recommend it?
There are 2 key reasons:
1.Avoid data mix-up
Imagine the sensor is in the middle of a new conversion while you are reading register by register. You could read:

Bytes 0xF7 and 0xF8 from the old measurement
Bytes 0xF9 to 0xFC from the new measurement

You would end up with mixed garbage data!  With burst read, you capture everything in one instant, so all bytes belong to the same measurement.
2. Reduce interface traffic
Fewer transactions = less overhead on I²C or SPI = more efficient and faster. 

3.11.2 Trimming parameter readout
The trimming parameters are programmed into the devices non- volatile memory (NVM) during production and cannot be altered by the customer. Each compensation word is a 16-bit signed or unsigned integer value stored in two's complement.
As the memory is organized into 8-bit words, two words must always be combined in order to represent the compensation word.
The 8-bit registers are named calib00..calib25 and are stored at memory addresses 0x88...0xA1. The corresponding compensation words are named dig_T# for temperature compensation related values and dig_P# for pressure compensation related values.
The mapping is shown in Table 17.

Table 17: Compensation parameter storage, naming and data type

| Register Address LSB / MSB | Register Content | Data Type      |
|----------------------------|------------------|----------------|
|    0x88 / 0x89             | dig_T1           | unsigned short |
|    0x8A / 0x8B             | dig_T2           | signed short   |
|    0x8C / 0x8D             | dig_T3           | signed short   |
|    0x8E / 0x8F             | dig_P1           | unsigned short |
|    0x90 / 0x91             | dig_P2           | signed short   |
|    0x92 / 0x93             | dig_P3           | signed short   |
|    0x94 / 0x95             | dig_P4           | signed short   |
|    0x96 / 0x97             | dig_P5           | signed short   |
|    0x98 / 0x99             | dig_P6           | signed short   |
|    0x9A / 0x9B             | dig_P7           | signed short   |
|    0x9C / 0x9D             | dig_P8           | signed short   |
|    0x9E / 0x9F             | dig_P9           | signed short   |
|    0xA0 / 0xA1             | reserved         | reserved       |
