**Pysense**: Medicion de aceleracion, temperatura, presion, ...

# Pysense

## Introduccion
En este ejemplo, utiluzaremos el Lopy montado sobre la tarjeta base Pysense para acceder a varios sensores que teien esta placa, tales como: Acelerometro, termometro, barometro ...

## Objetivos

Aprenderemos como:
* acceder a los sensores dentro de la placa Pysense haciendo uso de modulos en nuestros programas en Python

## Componentes de hardware

Para este ejemplo necesitaremos:

- un modulo Lopy sobre la placa Pysense
- un cable microUSB
- computador con Atom

The source code is in the `src/pysense` directory.


## Modulos Python / Pycom para acceder a los sensores del Pysense

En esta sesion de laboratorio, estudiaremos los ejemplos:

* acelerometro `src/pysense/acceloremeter`
* medicion de luz ambiental `src/pysense/ambient-light`
* medicion de temperatura y presion atmosferica`src/pysense/temp-bar`
* medicion de temperatura y humedad `src/pysense/temp-hum` 

Pycom proporciona libreria (conjunto de modulos Python) abtrayendonos de los detalles de implementacion sobre los chips sensores. 


Let's take a look a the  `src/pysense/acceloremeter` first:

```python
from pysense import Pysense
from LIS2HH12 import LIS2HH12
import pycom
import micropython
import machine
import time

py = Pysense()
acc = LIS2HH12(py)
while True:
    print('----------------------------------')
    print('X, Y, Z:', acc.read())
    print('Roll:', acc.roll())
    print('Pitch:', acc.pitch())
    print('Yaw:', acc.yaw())
    time.sleep(1)
```

You notice that it looks particularly simple and straightforward! That's the power of code abstraction and Python. 

Let's go trough each step:

1. first create a `Pysense` object which will be used to communicate over the I2C serial bus between the microcontroller and sensor devices (in our specific case the **LIS2HH12** accelerometer - refer to the `src/pysense/datasheet` folder for sensors' documentation). 

```python
py = Pysense()
```

For further information on I2C:
* [wikipedia entry](https://en.wikipedia.org/wiki/I%C2%B2C)
* [Pycom `I2C` class](https://docs.pycom.io/pycom_esp32/library/machine.I2C.html)
* [I2C briefing by Marco Rainone, ICTP](references/i2csensors.pdf)

2. then create a `LIS2HH12` object passing the previously created Pysense object as argument. While the `Pysense` object provides general purpose methods to communicate over the I2C bus, this object will further specify the address of the device (`LIS2HH12` sensor) and register addresses of interest to be used to calculate and give access to the 3-axis accelerometer measurements.

`acc = LIS2HH12(py)`

3. and simply loop over measurements and readings every second:

```python
while True:
    print('----------------------------------')
    print('X, Y, Z:', acc.read())
    print('Roll:', acc.roll())
    print('Pitch:', acc.pitch())
    print('Yaw:', acc.yaw())
    time.sleep(1)
```

It's that easy!

**But less us be clear**, in many real implementation, as soon as you'd like to finetune the setups of your sensor, calibrate or sometimes even fix the firmware provided, you will have to immerse yourself in all the technical details of the chip sensor.

## Exercises

1. Change the color of the LED based on accelerometer measurements (green, orange, red).
2. When you push the button, the measurements are logged/saved into the `/flash/log` folder (while LED blinking blue every second) and when you push again logging is stopped and LED's colour is back to green.

## Advanced

We have provided two versions of the temperature and pressure example:
* `src/pysense/temp-bar` 
* `src/pysense/temp-bar-alt` 

It illustrates the situation referred to earlier when the firmware provided (here `MPL3115A2 class` in `/lib` folder) needs to be fixed.

Synchronize and run the first implementation `src/pysense/temp-bar` and you will notice that measurements do not look correct.

Instead, an alterative to the default implementation is provided under `src/pysense/temp-bar-alt` folder. Try to grasp the general idea.
