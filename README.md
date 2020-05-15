# The Things Network decoder function for TEKTELIC Agriculture Sensors
TEKTELIC Communications manufacture LoRaWAN Agriculture Sensors that measure soil moisture, and other environmental parameters, including light level and ambient temperature.

There are two versions of this sensor, one with built in probes and one with an external Watermark sensor. This function supports both types. This is targeted at TTN, but with minor changes will work in other network servers or Node-RED.

We sell the sensors online, here:
 * https://connectedthings.store/gb/tektelic-agriculture-soil-moisture-sensor-surface-version.html
 * https://connectedthings.store/gb/tektelic-agriculture-soil-moisture-sensor-external-probe.html

## Usage with The Things Network
To use this function:
* create a TTN application and register your devices using the TTN console
* in your application, choose "Payload Formats" from the navigation
* paste the decoder function into the textarea and save it

You can test the function by pasting in this example payload: 01040532020203780965096A0B6700AA0B6851

This should be turned into a JSON object that looks like this:
```
{
  "ambient_humidity": 40.5,
  "ambient_light": 2410,
  "ambient_temp": 17,
  "soil_gwc": 0.8,
  "soil_moisture_raw": 1330,
  "soil_temp": 888
}
```
(actual data from our test sensor)

## NOTE: Buggy temperature values
We're working with TEKTELIC to fix the soil temperature calculations for the built-in probes version.


## Calibration notes
The sensor version with built in probes needs to have its moisture reading calibrated for best accuracy. This is done by taking a "dry" reading from the sensor before deploying it. Let the unit send one uplink with the probes facing up into free space. This is the reference reaing you will see in the decoded payload as `soil_moisture_raw`. Multiply this value by 1000 and put it in the decoder as the `moisture_calibration` number at the top.

This calibration value is used in the calculation of the Gravimetric Water Content of the soil, returned as `soil_gwc`.

If you have several sensors, you may want to perform the GWC calcluation in your application and track the calibration values there.


## Note to ChirpStack (formerly LoraServer) users

If you are using this codec with ChirpStack then change the name of the method and the orders of parameters to Decode(port, bytes)



