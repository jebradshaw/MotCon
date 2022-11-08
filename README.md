# MotCon
This MotCon motor driver class can be used with a variety of motor driver integrated circuits for driving PM DC motors. The MotCon class is overloaded to accommodate either one or two direction pins and a PwmOut pin for speed control/motor enable .

```
#include "MotCon.h"     //uses the MotCon.h library for controlling the motor ports

//PC serial connection
Serial pc(USBTX, USBRX);    //tx, rx via USB connection
DigitalOut led(LED1);
MotCon m1(p25, p27);        //uses p25 for PWM and p27 for direction
MotCon m2(p26, p29, p30);   //uses p26 for pwm and p29 and 30 for direction (complimentary)

//------------ Main ------------------------------
int main() {    
    pc.baud(921600);//fast baud rate for USB PC connection
    while(1) {
        //iterate through 2*pi cycles in .01 increments
        for(float cycle=0;cycle<3.14159*2.0;cycle+=.01){
            float m1_dc = .85*sin(cycle);            
            m1.mot_control(m1_dc);
            
            float m2_dc = .85*cos(cycle);
            m2.mot_control(m2_dc);
                        
            pc.printf("cycle=%.3f  m1_dc = %.2f  m1_dc = %.2f\r\n", cycle, m1_dc, m2_dc);
            wait(.01);      //determines period
            led = !led;     //toggle LED1 to indicate activity
        }
    }
}
```
