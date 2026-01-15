# How to Integrate Real Sensors - Office Energy System

## Overview

This guide explains how to connect real hardware sensors to replace the simulated sensor data for office energy monitoring.

## Required Hardware

### Core Components
- **ESP32-S3** development board
- **USB cable** for programming and power
- **Breadboard** and jumper wires

### Sensors
1. **Occupancy Sensor**
   - **Recommended**: PIR Motion Sensor (HC-SR501)
   - **Alternative**: Ultrasonic Distance Sensor (HC-SR04)
   - **Interface**: Digital GPIO (PIR) or GPIO (Ultrasonic)

2. **Motion Detection**
   - **Recommended**: PIR Motion Sensor (HC-SR501)
   - **Alternative**: Microwave Motion Sensor (RCWL-0516)
   - **Interface**: Digital GPIO

3. **Light Level Sensor**
   - **Recommended**: BH1750 Light Sensor
   - **Alternative**: Photoresistor (LDR) with ADC
   - **Interface**: I2C (BH1750) or Analog ADC

4. **Temperature Sensor**
   - **Recommended**: DHT22 or DHT11
   - **Alternative**: DS18B20 or LM35
   - **Interface**: Digital GPIO (DHT) or OneWire/I2C

5. **Energy Consumption Monitor**
   - **Recommended**: PZEM-004T Energy Monitor
   - **Alternative**: INA219 Current Sensor
   - **Interface**: UART (PZEM) or I2C (INA219)

## Wiring Diagram

### PIR Motion Sensor (Occupancy/Motion)
```
ESP32-S3    HC-SR501
--------    -------
5V      ->  VCC
GND     ->  GND
GPIO27  ->  OUT
```

### BH1750 Light Sensor - I2C
```
ESP32-S3    BH1750
--------    -------
3.3V    ->  VCC
GND     ->  GND
GPIO21  ->  SDA
GPIO22  ->  SCL
```

### DHT22 Temperature Sensor
```
ESP32-S3    DHT22
--------    -------
3.3V    ->  VCC
GND     ->  GND
GPIO4   ->  DATA
```

### PZEM-004T Energy Monitor - UART
```
ESP32-S3    PZEM-004T
--------    ----------
5V      ->  VCC
GND     ->  GND
GPIO17  ->  RX
GPIO16  ->  TX
```

## Code Modifications

### Step 1: Disable Sensor Simulation

In `firmware/main/Kconfig.projbuild` or via `idf.py menuconfig`:
```
Office Energy System
  [ ] Enable synthetic sensor simulation
```

### Step 2: Add Sensor Libraries

Add to `firmware/main/CMakeLists.txt`:
```cmake
idf_component_register(
    SRCS "sensors.cpp" "main.cpp" ...
    INCLUDE_DIRS "."
    REQUIRES driver i2c driver uart driver gpio
)
```

### Step 3: Implement Real Sensor Reading

Create/update `firmware/main/sensors.cpp`:

```cpp
#include "sensors.hpp"
#include "driver/gpio.h"
#include "driver/i2c_master.h"

// PIR Motion Sensor
float readOccupancy() {
    // Read GPIO pin for motion detection
    // Return 0.0-1.0 occupancy score
}

float readMotion() {
    // Read PIR sensor
    // Return 0.0-1.0 motion level
}

// BH1750 Light Sensor
float readLightLevel() {
    // I2C communication with BH1750
    // Read lux value
    // Return normalized 0.0-1.0
}

// DHT22 Temperature
float readTemperature() {
    // Read DHT22 sensor
    // Return temperature in Celsius
}

// PZEM Energy Monitor
float readEnergyConsumption() {
    // UART communication with PZEM
    // Read power consumption
    // Return watts
}
```

## Calibration

### Occupancy Sensor
1. Adjust sensitivity potentiometer on PIR
2. Set detection range
3. Test with actual occupancy

### Light Sensor
1. Calibrate for office lighting conditions
2. Set thresholds for day/night
3. Adjust for different room types

### Energy Monitor
1. Calibrate current transformer (CT) if used
2. Verify voltage readings
3. Test with known loads

## Testing

### Step 1: Build and Flash
```bash
cd firmware
idf.py build
idf.py flash monitor
```

### Step 2: Monitor Serial Output
```bash
# Should see real sensor values
Occupancy: 0.75 | Motion: 0.60 | Light: 0.80 | Temp: 22.5°C | Energy: 450W
```

### Step 3: Verify ML Inference
- Check that classification changes with sensor data
- Verify energy efficiency states
- Test ThingSpeak upload

## Troubleshooting

### "Sensor not detected"
- ✅ Check wiring connections
- ✅ Verify I2C address (scan with `i2c_scanner`)
- ✅ Check power supply (3.3V/5V stable)

### "Readings unstable"
- ✅ Add filtering/averaging in code
- ✅ Check for electrical interference
- ✅ Verify sensor placement

### "Energy readings incorrect"
- ✅ Calibrate CT sensor
- ✅ Verify voltage/current calculations
- ✅ Check sensor specifications

## Safety Considerations

- **Electrical Safety**: Use proper isolation for energy monitoring
- **Power Ratings**: Ensure sensors can handle office power levels
- **Installation**: Follow local electrical codes
- **Calibration**: Regularly calibrate energy sensors

## Next Steps

1. Test each sensor individually
2. Integrate all sensors together
3. Calibrate for your office environment
4. Deploy to production

