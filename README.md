# color_based_sorting
color_based_sorting
Color based sorting
system ─ Group Mentors:
MANASVI
MEET SONI
RIYA
Group members:
Dhruv Oza
Vinit
Vinit Mairal
Anshoo Rajput
 Prashant Dodiya
 Prabhakar Jaiswal
 
 
 
 Introduction to our project
In this project we are going to learn and build a prototype of a machine which sort mixed color
objects according to their color. For this we are going to use a color sensor TCS 3200 which
senses the color and converts intensity into frequency which a microcontroller can understand.
The basic function of its working will be like we have to give mixed object of RED, GREEN and
BLUE color and it will put them in three different buckets.
Basic things we learned:
1. The basics of a timer/counter function of microcontroller ATmega 32
2. embedded C programming
3. The working of TCS 3200 color sensor
4. PWM generation and implementation via atmega32
5. UART communication using serial usb ttl
Major Components Required:
1. Atmega32
2. 2 Servo Motors
3. A Color sensor - TCS3200
4. 12V Power source and voltage regulator circuit
5. Plywood for mechanism
6. PL2303(USB -TTL) 



Software used:
Atmel studio
 Xtreme burner AVR
 XCTU

Introduction of Timer and Counter function of ATmega32

For working with TCS3200 color sensor and servos we must learn to work with timer clock
function of a microcontroller as we are working with frequencies and PWM (pulse width
modulation) which is necessary for actuation of servo motor
Goals

I. How to learn Datasheet
For getting familiar with a microcontroller and different sensors and to learn its whole
working and application datasheet is a must as it comprises of the pinout diagram and
specification.

II. To learn timer counter registers
 ATmega 32 microcontrollers come with an inbuilt clock of frequency 16MHz and
 4 clock pins 2 8-bit timer/counter and 2pins of 16bit
 8-bit Timer/counter register is TCCR0 and for 16 bit it is TCCR1A/B and few more
III. Different methods of data manipulation, left shift and Right shift
 For making different bits high or low without affecting other pins we use left shift or
right shift like we want to high pin A1 of port A high letting other pins as it is we can write
PORTA |= (1<< PINA1);
 
Generating PWM with timer/counter
PWM
It stands for pulse width modulation.
Nowadays, it is widely used to control electronic circuits. For example:- 1) it can be used to
control the brightness of LED 2) Controlling speed of motor 3) Efficient power supply etc..
For modulating LED we apply pulsing power supply (on and off) at a certain frequency and with
certain pulse width.
By adjusting the pulse width we decide the duty cycle of load, which is the time period of signal
for which it is high.
TIMER WITH PWM
Timer/counter1 is needed to use pwm. It basically counts oscillation going on.
According to resolution of timer ( 8bit or 16bit) we have to encode the value of duty cycle.
8bit counter/timer counts from 0 to 255
16bit counter/timer counts from 0 to 65535
Prescaling of this timer is also done to increase the time duration of one count of the timer.
As 8bit timer counts up to 255 only we can prescale it upto 255 only (considering 255 as 1
count).Due to which the output compare match that we wanted for servo motor(explained in 
 detail later) to work did not come into range. So we went for 16bit timer/counter1 with
prescaling of 64. We configured timer1 using microcontroller datasheet(atmega32).
MODES OF OPERATIONS
There are 4 modes in timer1:-
1) Normal mode
2) CTC mode
3) Fast PWM mode
4) Phase correct PWM mode
We have configured timer1 in fast pwm mode which provides a high frequency PWM waveform
generation option. The fast PWM differs from the other PWM options by its single-slope
operation. The counter counts from BOTTOM to TOP then restarts from BOTTOM. In noninverting Compare Output mode, the Output Compare (OC1x) is cleared on the compare match
between TCNT1 and OCR1x, and set at BOTTOM. In inverting Compare Output mode output is
set
on
compare match and cleared at BOTTOM.
 

Activating servo motor
A servo motor is an electrical device which can push or rotate an object with great
precision. If you want to rotate an object at some specific angles or distance, then you
use servo motor. It is just made up of simple motor which runs through servo
mechanism.
 
TCS3200 RGB color sensor

The sensor has four different types of filter covered diodes. In the 8 x 8
array of photodiodes, 16 photodiodes have Red filters, 16 have Blue filters,
16 have Green filters and the rest 16 photodiodes are clear with no filters. Each type can be
activated using the S2, S3 selection inputs. Since each photodiode are coated with different filters
each of them can detect the corresponding colours. For example, when choosing the red filter,
only red incident light can get through, blue and green will be prevented. By measuring the
frequency, we get the red-light intensity. Similarly, when choose other filters we can get blue or
green light.
We can also set the frequency scaling option by using the S0, S1 select lines.
Pin Configuration
PIN
NAME
PIN
NUMBER DESCRIPTION
GND 4 Power supply ground. All voltages are reference to the ground.
VCC 5 Supply voltage
OE 3 Enable for FO (Active low)
OUT 6 Output frequency (fo)
S0, S1 1, 2 Select lines for output frequency scaling
S2, S3 7,8 Select lines for photodiode type.
 7
S0 S1 OUTPUT FREQUENCY SCALING(f0)
L L Power down
L H 2%
H L 20%
H H 100%
S2 S3 PHOTODIODE TYPE
L L RED
L H BLUE
H L CLEAR (NO FILTER)
H H GREEN
This TCS3200 color sensor module consist of a TAOS TCS3200 RGB sensor chip and 4 white
LEDs. The main part of the module is the TCS3200 chip which is a Color Light-to-Frequency
Converter. The white LEDs are used for providing proper lighting for the sensor to detect the
object colour correctly. This chip can sense a wide variety of colours and it gives the output in
the form of corresponding frequency. This module can be used for making colour sorting
robots, test strip reading, colour matching tests etc.
To determine the colour of an object, we’ve to measure the frequency from 6th pin OUT when each
filter is activated.
Set both S2 and S3 to LOW, measure the frequency. Now we get the intensity of RED component in
the object.
Set S2 to LOW and S3 to HIGH in order to get the intensity of BLUE component in the object.
Set both S2 and S3 to HIGH and get the intensity of GREEN component in the object.
 8
Compare the frequencies of the three components to get the actual colour of the object.
 9
USART and UART-
• what is USART?
• Difference between USART and UART
• what was the need to use UART?
• problem faced during the initial stage of using UART
1. A USART (Universal Synchronous/Asynchronous Receiver/Transmitter) is a
microchip that facilitates communication through a computer's serial port using
the RS-232C protocol. ... However, unlike a UART, a USART offers the option of
synchronous mode.
2. Difference between USART and UART
a. The first difference between a USART and a UART is the way in which the
serial data may be clocked. A UART generates its data clock internally to
the microcontroller and synchronizes that clock with the data stream by
using the start bit transition. ... A USART, on the other hand, can be set up
to run in synchronous mode
b. In synchronous mode, the data is transmitted at a fixed rate. In
asynchronous mode, the data does not have to be transmitted at a fixed
rate.
c. Synchronous data is normally transmitted in the form of blocks, while
asynchronous data is normally transmitted one byte at a time.
d. Synchronous mode allows for a higher DTR (data transfer rate) than
asynchronous mode does, if all other factors are held constant.
3. The main reason behind using UART communication is to know the exact
frequency required to operate TCS3200.
4. Along with great communication data buses UART also play a major role in
debugging phase. Using UART enables us to use GUI softwares like X-CTU or
realterm which helps in receiving the characters or number that we needed to
pass. It makes easy for us to interpret the bug in the code.
 10
Problems faced –
A. Determining of baud rate to be worked on was a tough job. To know that
we tried both 9600 and 1200 baud rate.
B. Getting a character value on the X-CTU cannot be done directly by using
the "char" keyword so we had to initialize the variable using uint8_t which
means undefined variable with memory space allotted 8 bits.
C. The major problem is to print get an integer value on the x-ctu. To get that
tried to
1. Initialize the variable directly as int.
2. Initialize the variable as uint8_t.
3. Initialize the variable as char and the passing the integer
value indirectly.
4. The first problem faced by us was writing the receiving and transmission
function. So, by
5. using the datasheet of atmega32 and some web links, we gathered ideas and
information and
6. with a lot of effort, we finally coded it. After that while we were writing the rest of
the code and
7. compiling it, the error was popping up about the unknown names of registers
and bits used such
8. as UCSRB, UCSRC, UDRE, etc. which are commonly used registers. We asked our
mentors
9. about this and they advised us to make the executable file for the UART code as
the compiler
10. don't know what they are.
11. After that, we tested our code by sending a character by setting baud_rate:
2400 and
12. frequency of microcontroller F_CPU: 16 Mhz and accordingly setting the value of
UBRR as per
13. the formula:
14. UBRR= (F_CPU/16*baud_rate) -1;
15. but the test result was just an infinite series of dot which was obviously not
expected. We tried 
 11
16. with various baud_rate and accordingly setting the value of UBRR but all in
vain.Then after our
17. mentor advised us by their past experience to set the baud_rate: 1200 bps and
UBRR= 51 and
18. we were getting the perfect result.
19. It was contradictory to the formula mentioned in the datasheet. For the
baud_rate: 1200 bps the
20. calculated value of UBRR must be 832, but we can't neglect the fact and still we
couldn't find
21. out why it was happening.
22. After transmitting a character, we then moved forward to transmit the
integer. We
23. declared uint8_t num variable and assigned 32 to it and on testing it we were
getting the its
24. correct hex value but the integer 32 was not getting printed. We are still working
on this error.
