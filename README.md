# esphome-multisensor
An esphome sensor for home assistant with motion, temperature, humidity, pressure and light level sensors.


![sensor](/black and white sensor.png)

### List of parts used
* wemos D1 mini esp32 (controller)
* BME280 (Temperature, Humidity & Barometric Pressure)
* BH1750 (Ligh intensity)
* AM312 (PIR motion)

## Wiring diagram
![wiring diagram](/Wiring.png)


## esphome config
Add this to your esphome config:
```
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
```

## 3D printed case

I have printed the case in ABS on a Voron 2.4. Other plastics are not tested, but should work fine aswell. Be awere that PLA can melt if left in the sun for too long.

The case is designed to snap in place without any hardware. There are slots for opening the case with a small flat screwdriver.


My print settings with 0.4 nozzle in Cura:
```
Layer hight = 0.2 mm
Line width = 0.4 mm
Wall line count = 4
Print thin walls = on
Top/bottom thickness = 1 mm
infill pattern = Cubic
Infill Density = 40%
```


## Mounting in the case

1. Make sure all the wires going thru the PCB's are not protruding past the components on the board. Cut if nessesary as these will prevent the parts to snap correctly in place.

2. Begin with mounting the BME280 and the BH1750. These shoud snap in place when aliging the edge oposite of the notch for the scredriver into the slot, then push the other end into the slot. 
3. Then mount the PIR sensor. Take of the cap and the rim (if it doesn't come lose with the cap). Then put the PIR thru the hole and mount the rim and cap on the pir again. 
4. To mount the controller to the bottom pice, slot the end of the controller with the wifi antenne under the angled parts on the bottom part. Then push on the corners on the USB side of the controller. It shoud snap in place with substancial force. 

5. The last part is to attach the bottom piece to the top piece. Be careful not to bend the PCB of the PIR as the soldring attaching the PIR sensor to the PCB can come loose. Put one of the sides if the bottom into the notches on the side of the top, then push the other side firmly in place. There should be a click and the bottom should be flat with the top if mounted correctly.
