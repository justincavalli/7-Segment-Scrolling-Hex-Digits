# 7-Segment Scrolling Hex Digits

## Introduction
For this project we were tasked with creating a multiplexed 7-segment display that would count seconds using hexadecimal digits. This effect is accomplished by using a common technique of refreshing the display at a fast enough rate giving the illusion of all digits being on at the same time and counting as desired. This illusion is important for many of the displays that we see everyday.

## Plan
We were given code that scrolled across the 7-segment display one digit at a time while adding 1 every second. From there our first goal was to alter this scroll to match the pattern given to us which was 0,3,1,2. Next using the second register we wanted to split it up into 4 parts each containing 4 bits which would be bound to each of the 4 numbers on the 7-segment display. The reason we use 4 bits to display one number is that our numbers are stored in hexadecimal, which uses 4 bits to store its value. Finally we want to change the refresh rate to something that looks as if all the bits are displayed at the same time, but not too fast because each LED does not have time to fully power making them look dim. 

## Implementation
The second state machine is used to counts seconds. The FPGA board uses a 12 Mhz clock so the second_timer_state is initially set to 11,999,999 and is counted down to 0 each clock cycle on the positive edge. When the state_timer state reaches 0, the second register is incremented one to keep track of the seconds that has elapsed since the first initialization. In this case, the seconds are initially set to the value ACDC to more easily assess the functionality of the state machine to 7-segment display.</br>

Separated from this counting state macine is code to control the 7-segment display. The first case statement is the one from the last project that reads the hex2Display register to ascertain which value to display. The second and third case statements are added for this project. The first case statement controls the order in which the 4-digits display on the 7-segment display comes on. For our team that order was 0,3,1,2. The time between the segments turning on and turning off is controlled by the dwell rate as indicated by the refreshcounter[15:14] dividing this value by the speed of the clock 12 Mhz it indicates that the segments refresh every 1.37ms. A faster speed would look like a strobe light, and a slower speed would allow us to see the scrolling pattern. The third case statement takes the correct 4 bit segment of the second register to display on the corresponding digit on the 7-segment display. 

## Conclusion
The project works just as we intended it to, and we didn’t encounter too many issues during our implementation. One problem we did discover, however, was that our display was dimmer than we wanted it to be. We discovered that our dwell time was too short and wasn’t giving the LEDs enough time to heat all the way up before refreshing once again. By adjusting the rate, we were able to find a good value in which the display was not being refreshed too fast or too slow so that the numbers were solid (not blinking) and bright.

