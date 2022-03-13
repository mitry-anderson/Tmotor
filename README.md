# TControl
An API in progress for controlling the AK-series Tmotor Actuators from CubeMars over the CAN bus.
The project is geared towards the control of the AK80-9 actuator using a raspberry pi CAN hat, but
could eaisly be adapted for use with a different CAN interface. The main library is in the file
TControl.py, and sample scripts can be found in the test folder. 

## Calibrating and Configuring the Motor
Before you can use the motor, you may need to run an encoder callibration routine and you may
desire to change the motor's CAN ID from 1 (the default) to a number of your choosing. This can be
done by connecting to the UART port on the motor and sending a few commands. CubeMars sells a custom
device called [R-Link](https://store.tmotor.com/goods.php?id=1185) that can be used with their GUI 
(downloadable from the same page) to connect to the motor over serial, or you can connect using an 
FTDI usb to serial chip, or something similar. If you use your own serial connection, the baud rate is 921600.
The advantage of the R-Link device is that it can be used to send test CAN messages to verify that 
the motor is working, but the same thing can be achieved with the TControl library if it is eaiser 
to connect to the UART port via another method. 

Whichever method you choose to connect to the serial port, turn the motor on with around 24V, and
you should see that the motor has sent messages offering a menu of configuration choices. It's okay
if a list of possible error codes prints out one time, but if they keep printing turn the motor off and
try to identify the problem. From the menu of options, send the code to calibrate the encoders, which
will start a short routine in which the motor turns slowly and records encoder positions. When this
routine finishes, you can change things such as the CAN ID in the settings menu by sending the codes that
it prompts you. For a more detailed guide on the setup and configuration of the motor, see the 
[AK-series motor manual](https://store.cubemars.com/images/file/20211201/1638329381542610.pdf)
from the TMotor site.

## Setting up the PiCAN 2 Hat and TMotor
We used the [PiCAN 2 CAN Bus Hat](https://copperhilltech.com/pican-2-can-bus-interface-for-raspberry-pi/) 
from CopperHill technologies to interface between the raspberry pi and the motor's CAN network.
Before using the hat, you will need to activate the termination resistor by soldering a 2 pin 
header to JP3 on the hat and connecting the leads. Of course, you will also need to connect
the hat's CAN high and CAN low screw terminals to the motor's CAN port. You also need to connect
the hat's ground screw terminal to a common ground with the motor--connecting to the ground pin
on the UART port on the motor will work for this purpose.

With the elctronics set up, you can follow the [instructions](https://copperhilltech.com/blog/pican2-pican3-and-picanm-driver-installation-for-raspberry-pi/)
given on the CopperHill website to set up the software on the pi to drive the PiCAN 2 Hat. 
Before connecting the motor, you can verify that the PiCAN hat is functioning properly by
starting up the network in loopback mode, with the following command, as recommended in the
[troubleshooting guide](https://copperhilltech.com/blog/pican2-can-bus-board-for-raspberry-pi-functionality-test/)
on the CopperHill website. 

If the loopback mode test worked successfully, then the PiCAN hat is working properly, and you
can begin to test the motor. To verify that the CAN connection to the motor is working properly,
run the "test_motor_connection.py" script in the test folder, with the "ID" and "type" variables
set to the proper CAN ID and motor type (default 1 and AK80-9) for your setup. If all is working
correctly, you should see the motor print out it's current position and velocity information. If 
you see an error related to the CAN bus, then check the connection and verify the CAN hat is working.
If you see an error that seems to be related to the TControl API implementation, then let us know
so we can help you troubleshoot.

With the motor configured and connected, you can now start programming! For some code examples,
see the test folder in this repository. For some more detailed explainations, see the section
below or check out the API full documentation. (insert link to that when I make it)

## API usage


## Notes to include later:

1. You need to calibrate the motors before you can use them (or possibly just after tampering with them), this is done over serial
2. If the serial interface is giving you OTW fault, it's a temperature fault, wait for the chip to cool off
3. Serial baud rate: 921600
4. CAN bus data rate: 1MBps
5. You need to use the 120 Ohm Termination resistor for proper communication
6. The zeroing function of the TMotor control board causes about a second of delay time before the board will send state updates again :/



This work is performed under the [Neurobionics Laboratory](https://neurobionics.robotics.umich.edu/) 
under Drs. Elliott Rouse and Gray Thomas.
