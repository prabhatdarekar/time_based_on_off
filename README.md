*/
time_based_on_off
This code is used for slave device(Arduino) and to control on and off time using the Timer interrupt.
This code is designed to control a circuit of 4 relay modules from master device.
By expanding, we can make it to worrk for as many relay modules considering the available i/o pins on Arduino. 
Using small modification one can use this code to control from serial monitor. 
/*

#include<Wire.h>                      // I2C library for communication 
#include<TimedAction.h>               // This library used for timer inerrupt

#define slave_addr 0x06               // Its your slave address
#define on_state LOW                  // since i have used relay module, it switch on when pin is LOW 
#define off_state HIGH
#define timer 60000                   // set time to call interrupt in milisecond 
#define count 10                      // set the counter for 10 i.e for 10 minutes

volatile unsigned int L1,L2,L3,L4 ;  

void ISR_1(){                         // define function to call when interrupt raised
 L1++;
}

void ISR_2(){
  L2++ ;
}

void ISR_3(){
  L3++ ;
}

void ISR_4(){
  L4++ ;
}

TimedAction timer_1 =TimedAction(timer,ISR_1);          //create an object  
TimedAction timer_2 =TimedAction(timer,ISR_2);
TimedAction timer_3 =TimedAction(timer,ISR_3);
TimedAction timer_4 =TimedAction(timer,ISR_4);

void pin_off(){
if (L1>count) digitalWrite(pin_1,off_state);
if (L2>count) digitalWrite(pin_2,off_state);
if (L3>count) digitalWrite(pin_3,off_state);
if (L4>count) digitalWrite(pin_4,off_state);
}

void setup(){
pinMode(pin_1,OUTPUT);                                    \\set the pin to output mode and off the relay
digitalWrite(pin_1,off_state);
pinMode(pin_2,OUTPUT);
digitalWrite(pin_2,off_state);
pinMode(pin_3,OUTPUT);
digitalWrite(pin_3,off_state);
pinMode(pin_4,OUTPUT);
digitalWrite(pin_4,off_state);
                                                           
Wire.begin(slave_addr);                                  \\setup i2c 
Wire.onReceive(receiveData);
}

void loop(){
\\starts the timer
timer_1.check();
timer_2.check();
timer_3.check();
timer_4.check();

pin_off();
}

\\ this fuction is called when data received by i2c

void receiveData(int byteCount){
while(Wire.available()) {
number = Wire.read();

\\In each if statement there are sentence which on the relay and set the counter to zero
if (number==1){
digitalWrite(pin_1,on_state);
L1=0;
}

if (number==2){
digitalWrite(pin_2,on_state);
L2=0;
}

if (number==3){
digitalWrite(pin_3,on_state);
L3=0;
}

if (number==4){
digitalWrite(pin_4,on_state);
L4=0;
}
}}
