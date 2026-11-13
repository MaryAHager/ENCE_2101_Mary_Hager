Starting Point - Used Lab 09 FSM Single Stoplight Simulation as a Template 

**(1)** Use the FSM as a central control for both traffic lights. 
This implemented the logic that the three states from traffic light A could be used to control states in traffic light B. 
- Red_A and Green_B - 00 
- Yellow_A and Yellow_B - 01
- Green_A and Red_B - 10

This logic preliminarily works but causes a yellow state to be skipped when going from 10 to 00. 
![[20261111-0526-50.3538167 1.mp4]]

![[20261111-0514-35.1519285.mp4]]

Added a fourth state to the lights to create a yellow stage in between Light 2 and Light 1.
![[Pasted image 20261113095220.png]]

The FSM switches between the 4 states at the rate of the clock. 
The MUX if EN=0 the inputs point to holding the same state, if EN=1 the inputs point to the next state. 
![[20261113-1736-52.8821072.mp4]]


Behavior Fix - Stopights only go to yellow when moving from Green to Red
![[20261113-1743-29.8222874.mp4]]
A light holds RED when the other light is both GREEN and YELLOW. 
![[20261113-1745-38.8327290.mp4]]

** (2) IMPLEMENATION - HOLD GREEN UNTIL PED BUTTON IS PRESSED**

Creation of PED Button to trigger a 10 second count down 
![[Pasted image 20261113105932.png]]


Pedestrian buttons trigger a 10 second count down

**(3) EN is triggered when PED Counter = 00**
![[20261113-1841-13.4998726.mp4]]
Currently Cycle_Complete gives 1 when counter reaches zero. When both Cycle_Counters are at 0 the stoplghts continue through their automatic cycle. 


**(4) When a traffic light reaches RED it holds until the opposite PED button is pressed 

![[20261113-1859-20.9199928.mp4]]

EN Pin is only 1 if 
* Stoplights are yellow -> move to red state
* Cycle complete = 1 AND the opposite light is Red 
	* ISSUE - If a pedestrians press both buttons the system doesn't know how to "wait" for the other cycle to conclude
	* GOOD - If a pedestrian presses the same button twice the light holds its position so the pedestrian can cross
	* ISSUE - The timing of the cycle is off -> the pedestrian should only cross when the oposite light is RED not yellow. 



![[Pasted image 20261113122005.png]]
Created an "ALLOW" crossing function that should only be triggered when the opposite light from the PED signal is pressed. However, currently when PED B is pressed no countdown is triggered 

Possible Issues 
* Timing mechanism is still tied to a three second transition between states and there is unnecessary hardware. 

![[20261113-1937-07.9211588.mp4]]
Counter only starts once the Green for the same crossing direction is achieved

**GOAL ACHIEVE**D

Limitation - Cycle that traffic light goes through is FAST, however it does signal a traditional traffic cycle prior to the button being pressed then prioritizes the pedestrian crossing. 