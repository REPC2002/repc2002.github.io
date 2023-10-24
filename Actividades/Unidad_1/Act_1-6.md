# Actividad 1.6

# Codigo de la Actividad:
```python
from machine import Pin
import time
led_1 = Pin(25, Pin.OUT)
while True:
    led_1.value(1)
    time.sleep(0.5)
    led_1.value(0)
    time.sleep(0.5)
    print("Hola Mundo!!")
