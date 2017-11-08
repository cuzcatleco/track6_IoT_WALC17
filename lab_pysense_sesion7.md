**Pysense**: Medicion de aceleracion, temperatura, presion, ...

# Pysense

## Introduccion
En este ejemplo, utilizaremos el modulo Lopy montado sobre la tarjeta base Pysense para acceder a varios sensores incluidos en esta placa, tales como: acelerometro, termometro, barometro ...

## Objetivos

Aprenderemos como:
* acceder a los sensores dentro de la placa Pysense haciendo uso de modulos en nuestros programas en Python

## Componentes de hardware

Para este ejemplo necesitaremos:

- un modulo Lopy sobre la placa Pysense
- un cable microUSB
- computador con Atom

El material de codigo fuentes de los ejemplos, carpeta del Drive
`WALC_2017_TRACK_6_IoT_MATERIAL/RECURSOS/SESION 6/pysense`


## Modulos Python / Pycom para acceder a los sensores del Pysense

En esta sesion de laboratorio, estudiaremos los ejemplos:

* acelerometro `src/pysense/acceloremeter`
* medicion de luz ambiental `src/pysense/ambient-light`
* medicion de temperatura y presion atmosferica`src/pysense/temp-bar`
* medicion de temperatura y humedad `src/pysense/temp-hum` 

Pycom proporciona libreria (conjunto de modulos Python) abtrayendonos de los detalles de implementacion sobre los chips sensores. 


demos un vistazo a codigo del manejo ser sensor acelerometro `/pysense/acceloremeter` :

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

Es de notar que parece simple y directo! gracias a la abstraccion de codigo posible en Python.

Veamos paso a paso:

1. primero se crea un objeto `Pysense` que sera utilizado para la comunicacion I2C entre el microcnotrolador y el sensor (en nuestro caso el chip **LIS2HH12** ). 

```python
py = Pysense()
```

Para mas informacion sobre comunicacion I2C:
* [wikipedia entry](https://en.wikipedia.org/wiki/I%C2%B2C)
* [Pycom `I2C` class](https://docs.pycom.io/pycom_esp32/library/machine.I2C.html)
* [I2C briefing by Marco Rainone, ICTP](references/i2csensors.pdf)

2. luego creamos un objeto `LIS2HH12` pasandole como argumento el objeto Pysense como argumento. Mientras el objeto `Pysense` oproveera metodos generales para comunicarnos sobre el bus I2C , este nuevo objeto especificara la direccion del propio chip de sensor (`LIS2HH12` sensor) y las direcciones de registros a utilizar para acceder y calcular las mediciones del acelerometro.

`acc = LIS2HH12(py)`

3. y simplemente realizar las mediciones en un ciclo cada segundo:

```python
while True:
    print('----------------------------------')
    print('X, Y, Z:', acc.read())
    print('Roll:', acc.roll())
    print('Pitch:', acc.pitch())
    print('Yaw:', acc.yaw())
    time.sleep(1)
```

Asi de sencillo!

### Practicas

1. implementa en tu placa los ejemplos
    #### accelerometer
    #### ambient-light
    #### temp-hum
    
de nuevo, los codigos fuentes estan disponibles en 
`WALC_2017_TRACK_6_IoT_MATERIAL/RECURSOS/SESION 6/pysense`

## Desafios 

Basado en lo aprendido al realizar las practicas anteriores, desafiate a realizar los siguientes:

1. Cambia el color del LED basado en la medicion del Acelerometro (verde, naranja, rojo).
2. Implementa un semaforo de 7 colores basado en el nivel de temperatura del sensor del pysense.
2. Cuando se presione el boton del Pysense, las mediciones de los sensores se comienzan a guardar en la memoria SD, en el folder  `/flash/log` cada 1 segundo (al mismo tiempo el LED Azul parpadea) y luego cuando se presione de nuevo el boton se dejan de almacenar en la SD y el LED cambia a Verde.

