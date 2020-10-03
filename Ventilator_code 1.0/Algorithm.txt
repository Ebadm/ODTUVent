Code Outline

Calibrate_machine: 

1.We will make use of a struct data type to compare and store all of the following values in a header file and use them to initialise.
2.Read and store the delta time taken for a breathing cycle to be completed, stored value then used to vary BPM ratio.
3.Read and store the I/E ratio, stored value then used to accurately measure the ratio of length of inspiration to the length of expiration in a breathing cycle (range is from 1:1 to 1:3)
4.Read and store the current positive end-expiratory pressure(PEEP),which is then used to monitor and vary how much the air inlet valve opens at the start of the breathing cycle(PEEP:5-10 cmH20 ).
5.Read and store the highest peak pressure reached, which is then used to flatten the graph(if inside the limits of pressure allowed)  and eventually plot it on the LCD.
6.Read the potentiometer’s Inspiratory Pressure, it is used to determine how much the inspiratory valve is opened during graph plateau.
7.Read the potentiometer’s Expiratory Pressure, it is used to determine how much the expiratory valve is opened during graph plateau. (We shall ideally aim for the expiratory value to pass less than half the pressure by inspiratory pressure by varying the valve’s opening)
8.Finally, these values will be stored to be then used to compare with constant time intervals when the system is in “operating mode”.
9.This initialization will then calibrate and send an error/alarm in case any unacceptable/odd value is noted.

Initialise_sensors:

1.Call each sensor(e.g BME280/Honeywell) individually, read the timing by millis() and read the calibration parameters(if they exist)
2.Check if each sensor isn’t taking too long to respond, if yes, then display a “bad sensor” error and close function.
3.Set a flag in pressure sensor to identify a possible failure

Update_sensors:

1.Set timer to 0, and trigger the function at regular intervals(20 ms)
2.Trigger a power measurement from the pressure sensor after the inlet valve and save the measurement
3.Calculate the pressures needed in the pipe for display and alarms
4.Check that all the measured values are realistic and if needed, set the pressure sensor alarm flag and trigger the buzzer.
5.Display the pressure and position of the valves through the Serial Plotter in Arduino IDE. 

Setup:

1.Functions of initialisation of sensors and calibration switched called.(explained above)
2.Initialization of buzzer output.
3.Opening of the Serial communication at desired speed.
4.Attachment of Servos to instances of the <Servo.h> library
5.Initial reading of the adjustments made in the potentiometers.


VENTILATION MECHANISM/ACTIVATION:
<img width="402" alt="soft" src="https://user-images.githubusercontent.com/64616825/95000818-89a2ef00-05dd-11eb-99e7-e52652682a55.png">


1)Two Arduino UNOs used in initial design, MASTER AND SLAVE.
  1.1 MASTER: responsible for initializing sample 20×4 LCD Display with sample text and alarms, starting transmission after reading values from Potentiometers and ON/OFF and alarm button from interface.
  1.2 SLAVE: responsible for running the process in three states in the loop() function as ON, OFF and RESET.
  In the loop function of the slave Arduino, An alarm library is initialized whose function checks for errors such as exceeded inhalation processor, mechanical failure, volume monitoring issues etc, and also checks for assistive breathing mechanism in case of pressure differences as recorded by the relevant sensor.


2.)SLAVE:

1.OFF: calls a stop() function to halt all processes.
  ON: continuous reproduction of previous calculated breathing cycles and actuation pattern
  RESET: a new reading is received from the User Interface:
2.Trajfactory() function initializes the sample cycles, like the initial concave, convex patterns, calculates the number of cycles, velocities and acceleration of motors. It uses a delta_t method to break down every cycle to smaller intervals relating to smaller and precise movements of the actuating motors. These smaller movements are cumulatively added to produce continuous exhalation and inhalation cycles.
