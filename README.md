# time_based_on_off
This code is used for slave device(Arduino) and to control on and off time using the Timer interrupt.

#include<Wire.h>                      \\ I2C library for communication 
#include<TimedAction.h>               \\ This library used for timer inerrupt

#define slave_addr 0x06               \\ Its your slave address
#define on_state LOW                   \\ since i have used relay module 
#define off_state HIGH
#define timer 30000                   \\ set time to call interrupt in milisecond
