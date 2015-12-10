# Enable 6-Channel ADC on pcDuino8 UNO with AD7997 Module

By Xiaohai Li (haixiaolee@gmail.com)

## Why do I need that?

Here is a picture shows the GPIO definition of pcDuino8 UNO. As you can see there should be 6 ADC channels on Arduino header J7. But the truth is the Allwinner H8 processor on UNO doesn't have any available ADCs.

![pcDuino8 UNO GPIO description](https://raw.githubusercontent.com/nightseas/pcduino_applications/master/pics/pcDuino9_GPIO_Def.JPG)

The implementation is made by another header J10 (not soldered), connecting to an additional ADC board which has ADI AD7997 12-bit ADC on it. 

## Add AD7997 ADC module to UNO

Here comes the ADC module:

![ADC Module](https://raw.githubusercontent.com/nightseas/pcduino_applications/master/pics/AD7997.jpg)

You got three options to connect UNO with ADC module:

 1. Just solder the module onto UNO;
 2. Solder a 90° header on UNO and connect it to the module with wires (may cause unstable readings);
 3. Solder 90° headers on both UNO and ADC module, male and female. Like this picture:

![Connection between UNO and ADC](https://raw.githubusercontent.com/nightseas/pcduino_applications/master/pics/pcDuino8_UNO_ADC.jpg)

## Compile the arduino lib for UNO

Additional package(s) are needed before using libarduino for UNO:
```sh
sudo apt-get install libi2c-dev i2c-tools git
```

Fetch the Arduino lib for UNO:
```sh
git clone https://github.com/nightseas/libarduino_uno
```

Clean and Compile the lib:
```sh
cd libarduino_uno
make clean
make
```

## Create a test program

Create new .c files in sample folder and modify Makefile to compile it. For example I want to create helloADC.c to read the values from all six ADC channels.
```C
#include <core.h>
int adc_id = 0;

void setup()
{
}

void loop()
{
    for(adc_id=0; adc_id<6; adc_id++)
    {
        int value = analogRead(adc_id); // get adc value
        printf("ADC%d value is %d\n",adc_id, value);
        delay(1000); // wait for one sec
    }
}
```

Add a line after OBJS=... and add the name of your program (without suffix '.c').
```sh
OBJS = i2c_rtc_test spi_nfc_test adc_test led_test
OBJS += helloADC
```

Compile and run:
```sh
make
sudo output/helloADC
```
