# Actividad 2.2

### Codigo:

```python
import machine
import time
import framebuf, sys
from ssd1306 import SSD1306_I2C
adcpin = 4
sensor = machine.ADC(adcpin)

i2c=machine.I2C(0,sda=machine.Pin(0),scl=machine.Pin(1),freq=400000)
oled=SSD1306_I2C(128,64,i2c)

def ReadTemperature():
    adc_value = sensor.read_u16()
    volt = (3.3/65535) * adc_value
    temperature = 27 - (volt - 0.706)/0.001721
    return round(temperature, 1)

def display_logo(oled, temperature):
    if temperature < 19: #En caso de temperatura fria
        buffer = bytearray([0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0x7f, 0xff, 0xff, 0xff, 0x3f, 0xff, 0xff, 0xff, 0x1f, 0xff, 0xff, 0xff, 0x0f, 0xff, 0xff, 0xff, 0x07, 0xff, 0xff, 0xff, 0x07, 0xff, 0xff, 0xf8, 0x03, 0xff, 0xff, 0xf8, 0x03, 0xff, 0xff, 0xf8, 0x03, 0xff, 0xff, 0xf0, 0x03, 0xff, 0xff, 0xf0, 0x03, 0xff, 0xff, 0xf0, 0x07, 0xff, 0xff, 0xf0, 0x03, 0xff, 0xff, 0xf0, 0x03, 0xff, 0xff, 0xf0, 0x01, 0xff, 0xfd, 0xf0, 0x00, 0xff, 0xfc, 0x40, 0x01, 0xff, 0xfe, 0x00, 0x00, 0xff, 0xff, 0x00, 0x00, 0xff, 0xfe, 0x00, 0x00, 0xff, 0xff, 0x00, 0x00, 0xff, 0xff, 0x00, 0x00, 0xff, 0xff, 0x00, 0x00, 0x7f, 0xff, 0x00, 0x00, 0x7f, 0xff, 0x80, 0x00, 0xff, 0xff, 0x80, 0x01, 0xff, 0xff, 0xc0, 0x01, 0xff, 0xff, 0xc0, 0x03, 0xff, 0xff, 0xe0, 0x0f, 0xff, 0xff, 0xf8, 0x3f, 0xff])
        
    else:
        if temperature >= 19 and temperature <= 27: #En caso de temperatura se promedio
            
            buffer = bytearray([0xff, 0xee, 0x7f, 0xff, 0xfe, 0x00, 0x00, 0x3f, 0xfc, 0x00, 0x00, 0x1f, 0xf8, 0x00, 0x00, 0x0f, 
0xf0, 0x00, 0x00, 0x07, 0xe0, 0x00, 0x00, 0x07, 0xc0, 0x00, 0x00, 0x03, 0xc0, 0x00, 0x00, 0x03, 
0x80, 0x00, 0x00, 0x01, 0x80, 0x00, 0x00, 0x01, 0x90, 0x00, 0x00, 0x01, 0xf0, 0x18, 0x00, 0x01, 
0xe0, 0x18, 0x00, 0x13, 0xe0, 0x38, 0x08, 0x3f, 0xe0, 0x7f, 0x08, 0x1f, 0xe0, 0xff, 0x0e, 0x3f, 
0xe1, 0xff, 0x0e, 0x3f, 0xe1, 0xff, 0x07, 0x7f, 0xf3, 0xff, 0x07, 0xff, 0xff, 0xff, 0x07, 0xff, 
0xff, 0xff, 0x07, 0xff, 0xff, 0xff, 0x07, 0xff, 0xff, 0xff, 0x07, 0xff, 0xff, 0xff, 0x07, 0xff, 
0xff, 0xff, 0x07, 0xff, 0xff, 0xfe, 0x07, 0xff, 0xff, 0xfe, 0x07, 0xff, 0xff, 0xfe, 0x07, 0xff, 
0xff, 0xfe, 0x07, 0xff, 0xff, 0xfc, 0x07, 0xff, 0xff, 0xfc, 0x07, 0xff, 0xff, 0xfe, 0x0f, 0xff, ])
        else:
        #EN caso de temperatura caliente
            buffer = bytearray([0xff, 0xff, 0xff, 0xff, 0xff, 0xe7, 0xff, 0xff, 0xff, 0xe3, 0x3f, 0xff, 0xff, 0xe2, 0x39, 0xff, 0xfe, 0x20, 0x31, 0xff, 0xfe, 0x00, 0x71, 0x9f, 0xff, 0x80, 0xf0, 0x1f, 0xf3, 0xc0, 0xf8, 0x3f, 0xf1, 0xf0, 0xf8, 0x3f, 0xf1, 0xf8, 0xf0, 0x07, 0xf8, 0xf8, 0x60, 0x07, 0x80, 0xf8, 0x01, 0x8f, 0x80, 0x30, 0x03, 0xff, 0xc0, 0x00, 0x07, 0xff, 0xf0, 0x00, 0x07, 0xe3, 0xe0, 0x00, 0x03, 0xc3, 0xc3, 0xe0, 0x00, 0x07, 0xc7, 0xe0, 0x00, 0x0f, 0xff, 0xe0, 0x00, 0x03, 0xff, 0xc0, 0x1e, 0x01, 0xf0, 0x80, 0x1f, 0x01, 0xe0, 0x07, 0x1f, 0x1f, 0xf0, 0x0f, 0x1f, 0x8f, 0xfc, 0x1f, 0x0f, 0x8f, 0xfc, 0x1f, 0x03, 0xcf, 0xf8, 0x0f, 0x00, 0xff, 0xf9, 0x8e, 0x00, 0x7f, 0xff, 0x8c, 0x04, 0x7f, 0xff, 0xdc, 0x47, 0xff, 0xff, 0xfc, 0xc7, 0xff, 0xff, 0xff, 0xe7, 0xff, 0xff, 0xff, 0xff, 0xff])
    fb = framebuf.FrameBuffer(buffer, 32, 32, framebuf.MONO_HLSB)
    oled.blit(fb, 0, 0)
    
while True:
    oled.fill(0)
    
    temperature = ReadTemperature()
    #print(temperature)
    oled.text("TEMP: " + str(temperature) + " C", 0,50)
    display_logo(oled, temperature) 
    oled.show()#Muestra la informacion en el Oled
    time.sleep(5)