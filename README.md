# esphome-multisensor
An esphome sensor for home assistant with motion, temperature, humidity, pressure and light level sensors.


Here is a short description of how to assemble the multisensor.

### List of parts used
* wemos D1 mini esp32 (controller)
* BME280 (Temperature, Humidity & Barometric Pressure)
* BH1750 (Ligh intensity)
* AM312 (PIR motion)

## Wiring diagram
![wiring diagram](/Wiring.png)


## esphome config
"""
i2c:
  sda: 21
  scl: 22
  scan: true

sensor:
  - platform: bh1750
    name: "multisensor-esp32-0-illuminance"
    address: 0x23
    update_interval: 10s

  - platform: bme280
    temperature:
      name: "multisensor-esp32-0-temperature"
      oversampling: 16x
    pressure:
      name: "multisensor-esp32-0-pressure"
    humidity:
      name: "multisensor-esp32-0-humidity"
    address: 0x76
    update_interval: 5s

binary_sensor:
  - platform: gpio
    pin: 27
    name: "multisensor-esp32-0-occupancy"
    device_class: motion
"""
