# Actividad 1.7

# Codigo de la Actividad:
```python
# Palacios Camacho Ruben Eduardo
from machine import Pin
from time import sleep
from dht import DHT22
from gpio_lcd import GpioLcd

# creating a DHT object
dht = DHT22(Pin(15))

# creating an LCD object
lcd = GpioLcd(rs_pin=Pin(16),
              enable_pin=Pin(17),
              d4_pin=Pin(18),
              d5_pin=Pin(19),
              d6_pin=Pin(20),
              d7_pin=Pin(21),
              num_lines=2, num_columns=16)
 
while True:
    # getting sensor readings
    dht.measure()
    temp = dht.temperature()
    hum = dht.humidity()
    x = 1

    if temp < 30: 
        if temp > 8:
            print("Temperature: {}°C   Humidity: {:.0f}% 🏖️".format(temp, hum))
        else:
            print("Temperature: {}°C   Humidity: {:.0f}% 🥶".format(temp, hum))
    else:
        print("Temperature: {}°C   Humidity: {:.0f}% 🥵".format(temp, hum))

    # displaying to LCD
    lcd.clear()
    lcd.putstr('Temp: ' + str(temp) + " C")
    lcd.move_to(0,1)
    lcd.putstr('Hum: ' + str(hum) + "%")

    # delay of 2 secs for the reading of the sensor
    sleep(2)
