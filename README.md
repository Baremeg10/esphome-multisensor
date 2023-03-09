# esphome-multisensor
<a href="https://www.buymeacoffee.com/feb980y" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>


An esphome sensor for home assistant with motion, temperature, humidity, pressure and light level sensors.

![](/black-and-white-sensor.jpg)

## Versions
### v1.0
First sensor. Proof of concept. This sensor has a large temperature and humidity error and should be upgraded to v2.0.

### v2.0 - WORK IN PROGRES!
Updated design. Calculating offset for temperature and hunidity based on testing and is much more accurate. Can be upgraded from V1.0 without any soldering

#### updates/changes:
* New housing
  - More air vents for the ESP32
  - Seperate room for BME280 with dedicated air vents
  - Added notch to support wallmount
* Added wall mount
* Added temperature offest curve based on testing
* Added humidity offest calculated based on temperature curve



## List of parts used
* wemos D1 mini esp32 (controller)
* BME280 (Temperature, Humidity & Barometric Pressure)
* BH1750 (Ligh intensity)
* AM312 (PIR motion)


bme280             	       |  bh1750                   |  am312                    |  wemos D1 mini esp32
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
![](/img/bme280.jpg)       | ![](/img/bh1750.jpg)      | ![](/img/am312.jpg)       | ![](/img/d1mini-esp32.jpg)

## Wiring diagram
![wiring diagram](/Wiring.png)

![](/img/presoldering-sensors.jpg)
![](/img/pre-soldering.jpg)
![](/img/solding-on-controller.jpg)
![](/img/combining-ground-leads.jpg)
![](/img/soldered-together.jpg)


## esphome config
Add this to your esphome config:
```
substitutions:
  devicename: multisensor-esp32-2 #the main name of the sensors
  update_interval: 10s  #the update interval for all sensors

i2c:
  sda: 21
  scl: 22
  scan: true

sensor:
  - platform: bh1750  #illuminance sensor
    name: ${devicename}-illuminance
    address: 0x23
    update_interval: ${update_interval}

  - platform: bme280
    temperature:
      name: ${devicename}-temperature-raw
      id: "temp_raw"
      oversampling: 16x
      accuracy_decimals: 2
      on_value:
        - sensor.template.publish:
            id: "temp"
            #temperature curve offset based on testing
            state: !lambda 'return (pow((1.0+0.02),(id(temp_raw).state+177.3))-34.0);'
    pressure:
      name: ${devicename}-pressure
    humidity:
      name: ${devicename}-humidity-raw
      id: "humidity_raw"
      on_value:
        - sensor.template.publish:
            id: "humidity"
            #humidity offset besed on temperature curve
            state: !lambda 'return ((((6.11*10.0*((7.5*id(temp_raw).state)/(237.3+id(temp_raw).state))*id(humidity_raw).state)/(100.0)))/(6.11*10.0*((7.5*id(temp).state)/(237.3+id(temp).state))))*100.0;'
    address: 0x76
    update_interval: ${update_interval}

  - platform: template #temperature sensor with offset
    name: ${devicename}-temperature
    id: "temp"
    unit_of_measurement: "°C"
    update_interval: 60min #this should be high because the sensor are being pushed ipdates from sensor above
  - platform: template #humidity sensor with offset
    name: ${devicename}-humidity 
    id: "humidity"
    unit_of_measurement: "%"
    update_interval: 60min #this should be high because the sensor are being pushed ipdates from sensor above
    
binary_sensor:
  - platform: gpio
    pin: 27
    name: ${devicename}-occupancy
    device_class: motion
```

## 3D printed case

I have printed the case in ABS on a Voron 2.4. Other plastics are not tested, but should work fine aswell. Be awere that PLA can melt if left in the sun for too long.

The case is designed to snap in place without any hardware. There are slots for opening the case with a small flat screwdriver.



Parts:
* top-part.stl
* bottom-part.stl
The parts are oriented in the correct print orientation.



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


## Mounting the case

1. Make sure all the wires going thru the PCB's are not protruding past the components on the board. Cut if nessesary as these will prevent the parts to snap correctly in place.

2. Begin with mounting the BME280 and the BH1750. These shoud snap in place when aliging the edge oposite of the notch for the scredriver into the slot, then push the other end into the slot. 
3. Then mount the PIR sensor. Take of the cap and the rim (if it doesn't come lose with the cap). Then put the PIR thru the hole and mount the rim and cap on the pir again. 
4. To mount the controller to the bottom pice, slot the end of the controller with the wifi antenne under the angled parts on the bottom part. Then push on the corners on the USB side of the controller. It shoud snap in place with substancial force. 

5. The last part is to attach the bottom piece to the top piece. Be careful not to bend the PCB of the PIR as the soldring attaching the PIR sensor to the PCB can come loose. Put one of the sides if the bottom into the notches on the side of the top, then push the other side firmly in place. There should be a click and the bottom should be flat with the top if mounted correctly.

if you like my work, consider buying me a coffee.
<a href="https://www.buymeacoffee.com/feb980y" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>

Pictures of notches to open case and take out sensors. On the sensors you can also put the end of the screwdriver into the mounting holes and pry them out.
![](/img/opening-bottom.jpg)
![](/img/notches-for-controller.jpg)
![](/img/notches-for-sensors.jpg)


## More pictures

![](/img/innside.jpg)
![](/img/front-and-side.jpg)
![](/img/top.jpg)
![](/img/side-and-back.jpg)
![](/img/back.jpg)
