.. _doc_firmware_vesc:

Configuring the VESC
==========================
.. warning:: **Important Safety Tips**

    * Put your car on an elevated stand so that its wheels can turn without it going anywhere. If you don’t have an RC car stand, you can use the box that came with your Jetson.
    * Make sure you hold on to the car while testing the motor to prevent it from flying off the stand.
    * Make sure there are no objects (or people) in the vicinity of the wheels while testing.
    * Use a fully-charged LiPO battery instead of a power supply to ensure the motor has enough current to spin up.
    
**Equipment Required:**
	* Fully built F1TENTH  vehicle
	* Box or `Car stand <https://www.amazon.com/Duratrax-Tech-Deluxe-Truck-Stand/dp/B0014T74MS/ref=sr_1_6?keywords=rc+car+jack&link_code=qs&qid=1584393402&sr=8-6>`_  to put vehicle on
	* Laptop/computer (does not need to running Linux)

**Approximate Time Investment:** 1 hour

Installing the VESC Tool
-------------------------
We need to configure the VESC so that it works with the ROS driver package. Before you start, you'll need to install the latest `VESC Tool <https://vesc-project.com/vesc_tool>`_ on a laptop or a PC.

A pre-built VESC Tool for MacOS can be found `here <https://github.com/rpasichnyk/vesc_tool/releases>`_ .

Powering the VESC
-------------------------
First we need to power the VESC. Plug the battery in. 

.. image:: img/vesc/vesc01.JPG

Note that you don't need to turn on the Powerboard for configuring the VESC. 

Next, unplug the USB cable of the VESC from the TX2 and plug the USB into your laptop. You may want to use a longer cable.

.. image:: img/vesc/vesc02.JPG

Connecting the VESC to your laptop
-----------------------------------------
Launch the VESC Tool. On the Welcome page, press the **AutoConnect** button on bottom left of the page. After the VESC is connected, you should see an updated status on the bottom right of the screen.

.. image:: img/vesc/connect.png

Updating the firmware on the VESC
-----------------------------------------
The first thing you'll need to do is to update the firmware onboard the VESC. On the left side of the screen, click on the **Firmware** tab. On bottom left of the page, check the **Show non-default firmwares** check box. On the right, you should see extra firmware options show up. Select the **VESC_servoout.bin** option. Afterwards, on the bottom right of the page, press the button with the down arrow to update the firmware on the connected VESC. A status bar at the bottom of the page will show firmware update status. After it's finished, follow the prompt on screen.

.. image:: img/vesc/firmware.png

Upload the motor configuration XML
-----------------------------------------
After firmware update, Select **Load Motor Configuration XML** from the drop down menu and select the provided XML file from `here <https://drive.google.com/file/d/1-KiAh3hCROPZAPeOJtXWvfxKY35lhhTO/view?usp=sharing>`_ . After the XML is uploaded, click on the **Write Motor Configuration** button (the button with a down arrow and the letter M) on the right side of the screen to apply the motor configuration. Note that in the future, you'll have to press this button whenever you make a change in motor configuration.

.. image:: img/vesc/xml.png

Detect and Calculate motor parameters
--------------------------------------------
To detect and calculate the FOC motor parameters, navigate to the **FOC** tab under **Motor Settings** on the left. At the bottom of the screen, follow the direction  of the arrows and clck on the four buttons one by one, and follow the on screen prompt. Note that during the measuring process, the motor will make noise and spin, make sure the wheels of your vehicle are clear.

.. image:: img/vesc/detect_motor.png

After the motor parameters are measured, the fields at the bottom of the screen should turn green. Click on the **Apply** button, and click the **Write Motor Configuration** button.

.. image:: img/vesc/apply_motor.png

Change the Openloop Hysteresis and Openloop Time
-------------------------------------------------------
Navigate to the **Sensorless** tab on top of the screen. Change the **Openloop Hysteresis** and **Openloop Time** to 0.01, and click the **Write Motor Configuration** button.

.. image:: img/vesc/open_loop.png

Tune the PID controller
---------------------------------
Now you can start tuning the speed PID controller. To see the RPM response from the motor, navigate to the **Realtime Data** tab under **Data Analysis** on the left. Click **Stream Realtime Data** button on the right (the button with letters RT), and navigate to the **RPM** tab on the top of the screen. You should see RPM data streaming now.

.. image:: img/vesc/realtime.png

To create a step response for the motor, you can set a target RPM at the bottom of the screen (values between 2000 - 10000 RPM). Click the play button next to the text box to start the motor. Note that the motor will spin, so make sure the wheels of your vehicle are clear from objects. Click the Anchor or STOP button to stop the motor.

.. image:: img/vesc/response.png

You want to look for a clean step response that has a quick rise time and zero to very little steady state error. Adjust the gains accordingly by navigating to the **PID Controllers** tab under **Motor Settings** on the left, and change the Speed Controller gains. General rules of tuning PID gains apply. If you're seeing a lot of oscillations, try changing the Speed PID Kd Filter.

.. image:: img/vesc/pid_gains.png

Hopefully you've tuned it well enough that your car will run better than this:

.. image:: img/vesc/vesc03.gif
	:align: center