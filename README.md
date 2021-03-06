<img align="right" src="https://github.com/MarkusIppy/raspexi-viewer-linux/blob/master/images/raspexi-logo.png?raw=true" width="366" height="91" />
#<big>__Raspexi__<br></br></big>
<big>_A'PEXi Power FC Interface for Raspberry Pi_</big>




Summary
-------

Digital Gauge Display for A'PEXi Power FC which runs on Raspberry Pi. Raspexi is capable of communicating with the Power FC to retrieve diagnostic information and display the information in the form of aesthetic guages and dashboards. Raspexi has been tested for Mazda Power FC's and is currently in beta testing for Nissan, Subaru and Toyota Power FC's.

The Gauges and Dashboards used in this software are adapted from <a href="https://github.com/djandruczyk/MegaTunix">__MegaTunix__</a> by _David J. Andruczyk_<br>
The A'PEXi Serial interface is adapted from <a href="http://kaele.com/~kashima/car/pfcadp/FCLoggerFD3S.xls">__FCLogger__</a> by _Hitoshi Kashima_


Installation
------------
#### Pre-Requisite

Rasberry Pi with Raspbian OS. Tested versions:<br>


* 2016-05-10-raspbian-jessie works on PI 1 , PI 2 and PI 3
* 2016-03-18-raspbian-jessie works on PI 1 , PI 2 and PI 3
* 2015-11-21-raspbian-jessie works on PI 1 , PI 2 and PI 3
* 2015-02-18-wheezy-raspbian
* 2014-01-07-wheezy-raspbian
* 2013-09-25-wheezy-raspbian
* 2013-05-25-wheezy-raspbian
 

WIFI dongle for use with GoPro

Works on Raspberry Pi 1/2/3 (you can use built in WIFI for Pi3)

#### Tested Vehicles :                          Engine:
* Mazda RX7 FD3S                                (13B-REW)          <big>__✓__
* Mazda RX7 S5 Engine with AP Engineering ECU   (13B)              <big>__✓__
* Nissan Silvia  S15 D-Jetro                    (SR20DET)          <big>__✓__
* Nissan Skyline R33 GTS-T                      (RB25)             <big>__✓__
* Nissan Skyline R34 GT-T                       (RB25)             <big>__✓__ 
* Subaru Impreza WRX GF8                        (EJ205G)           <big>__✓__ 


#### Install
Extract the archive `raspexi-yyyymmdd.tar.gz` to `/home/pi`:

```
$ cd /home/pi
$ tar xvzf raspexi-yyyymmdd.tar.gz
```


Configuration
-------------
There are a few configurable settings in `raspexi.cfg`:

```
[default]
port = /dev/ttyUSB0
baud = 57600,8,n,1
interval = 35
model = Mazda
vehicle_mass = 1300
gear_judge_nums = 120, 70, 45, 35, 26
speed_correction = 1.03
original_tyre = 195/55 R15
current_tyre = 225/65 R17
dash1 = Analogue_Dash_1280_720.xml
dash2 = Commander_1280_720.xml
dash3 = Digital_Dash_1280_720.xml
dash4 = Race_1280_720.xml
analog_eq1 = 1.4 * (AUX1 - AUX2) + 9.0
analog_eq2 = 0.5*AUX3+2.5
gopro_ip = 10.5.5.9
gopro_pass = Markus123
gopro_wifi_type = camera
camera_record = r
csvfile = /home/pi/raspexi/log/raspexi.csv
```

* port ==> Serial port device name

* baud ==> Serial port baud rate

* interval ==> Data refresh interval

* model ==> Mazda, Nissan, Subaru, Toyota

* vehicle_mass ==> Mass of the vehicle in kilograms (required for __`Power`__ datasource)

* gear_judge_nums ==> Gear Judge numbers to estimate the current gear. In order 1st-4th, 1st-5th or 1st-6th, calculated from Rpm/Kph. (Can be calculated from a log file) (required for __`Gear`__ datasource)

* speed_correction ==> A simple multiplier for correction of the speedometer. For instance, can be calculated from a GPS comparison. (Takes precedence over tyre size calculation) (adjusts __`Speed`__ datasource)

* original_tyre, current_tyre ==> Original/Stock tyre size (speedo calibrated tyre size) and the current tyre size for speedometer correction. (adjusts __`Speed`__ datasource) 

* dash1, dash2, dash3, dash4 ==> Dashboard XML file

* analog_eq1, analog_eq2, analog_eq3, analog_eq4 ==> Analog equations for auxiliary input relations. (See section _'Analog Auxiliary Input Relationship Equations'_ for more details)

* gopro_ip ==> Ip adress of your GoPro (seems to be always 10.5.5.9 )

* gopro_pass ==> Your GoPRo WIFI password 

* gopro_wifi_type ==> camera,  bacpac 
( Camera is used for GoPro 3 and 3+ with build in WFIF , use bacpac for Hero 2 with WIFI backpack ) 

*camera_record ==> r (mapped keypoard button to stop and start recording ) 

* csvfile ==> CSV log output file

*Note: If the 'csvfile' location is set to the `/tmp` directory then it will be removed by
the system on (re)start*

*Note: The 'model' setting is in testing and has only been confirmed for Mazda (March/2015)*


How To Run
----------
You can run this release with the following command:

```
$ cd /home/pi/raspexi
$ sudo ./run.sh
```


Gauge Datasources
-----------------
The following _Descriptions_ are for information which the Power FC will return for a given model. The _Datasource Name_ can be used in the dashboard XML files to define what information a particular gauge will display:

*Note: The current version is in testing for support for vehicles other than Mazda (March/2015)* 

Description										|Datasource Name		|Mazda			|Nissan			|Subaru			|Toyota
------------------------------------------------|:---------------------:|:-------------:|:-------------:|:-------------:|:-------------:
__NEW !__ _GoPro recording status (0/1)_					|__`Rec`__			|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
__NEW !__ _Vehicle power (kW)_					|__`Power`__			|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
__NEW !__ _Acceleration (+100km/h)(s)_			|__`Accel`__			|<big>__✓*__	|<big>__✓__		|<big>__✓*__	|<big>__✓*__
__NEW !__ _Acceleration (G)_					|__`GForce`__			|<big>__✓*__	|<big>__✓__		|<big>__✓*__	|<big>__✓*__
__NEW !__ _Force exerted (N)_					|__`ForceN`__			|<big>__✓*__	|<big>__✓__		|<big>__✓*__	|<big>__✓*__
__NEW !__ _Current gear number (#)_				|__`Gear`__				|<big>__✓*__	|<big>__✓__		|<big>__✓*__	|<big>__✓*__
__NEW !__ _Primary injector duty cycle (%)_		|__`PrimaryInjD`__		|<big>__✓*__	|<big>__✓__		|<big>__✓*__	|<big>__✓*__
__NEW !__ _0-100km/h Timer (Sec)_				|__`AccelTimer`__		|<big>__✓*__	|<big>__✓*__	|<big>__✓*__	|<big>__✓*__
_Engine Speed (rpm)_							|__`RPM`__				|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__		
_Absolute Intake Pressure (Kg/cm2)_				|__`Intakepress`__		|<big>__✓__		|				|				|<big>__✓*__
_Pressure Sensor Voltage (mv)_					|__`PressureV`__		|<big>__✓__		|				|				|<big>__✓*__
_Engine Load (N)_								|__`EngLoad`__			|				|<big>__✓__		|<big>__✓*__	|
_Mass Flow Sensor #1 (mv)_						|__`MAF1V`__			|				|<big>__✓__		|<big>__✓*__	|
_Mass Flow Sensor #2 (mv)_						|__`MAF2V`__			|				|<big>__✓*__	|<big>__✓*__	|
_Throttle Sensor Voltage (mv)_					|__`ThrottleV`__		|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Throttle Sensor #2 Voltage(mv)_				|__`ThrottleV_2`__		|				|				|				|<big>__✓*__
_Primary Injector Pulse Width (mSec)_          	|__`Primaryinp`__		|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Fuel correction_                              	|__`Fuelc`__			|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Leading Ignition Angle (deg)_                 	|__`Leadingign`__		|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Trailing Ignition Angle (deg)_                	|__`Trailingign`__		|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Fuel Temperature (deg.C)_                     	|__`Fueltemp`__			|<big>__✓__		|				|				|
_Metering Oil Pump Duty (%)_                    |__`Moilp`__			|<big>__✓__		|				|				|
_Boost Duty (TP.%)_                             |__`Boosttp`__			|<big>__✓__		|				|				|				|<font color="red">These are</font>
_Boost Duty (Wg,%)_                             |__`Boostwg`__			|<big>__✓__		|				|				|				|<font color="red">to be combined</font>
_Boost Pressure (PSI)_							|__`BoostPres`__		|				|<big>__✓*__	|<big>__✓*__	|<big>__✓*__	|<font color="red">in a future</font>
_Boost Duty (%)_								|__`BoostDuty`__		|				|<big>__✓*__	|<big>__✓*__	|<big>__✓*__	|<font color="red">release.</font>
_Water Temperature (deg. C)_                    |__`Watertemp`__		|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Intake Air Temperature (deg C)_                |__`Intaketemp`__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__	|<big>__✓*__
_Knocking Level_                                |__`Knock`__			|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Battery Voltage (V)_                           |__`BatteryV`__			|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_Vehicle Speed (Km/h)_                          |__`Speed`__			|<big>__✓__		|<big>__✓__		|<big>__✓*__	|<big>__✓*__
_ISCV duty (%)_                                 |__`Iscvduty`__			|<big>__✓__		|				|				|<big>__✓*__
_Mass Air Flow sensor activity ratio (%)_		|__`MAFactivity`__		|				|<big>__✓__		|<big>__✓*__	|		
_O2 Sensor Voltage (mv)_                       	|__`O2volt`__			|<big>__✓__		|<big>__✓*__	|<big>__✓*__	|<big>__✓*__
_O2 Sensor #2 Voltage (mV)_						|__`O2volt_2`__			|				|<big>__✓*__	|<big>__✓*__	|		
_Secondary Injector Pulse Width (mSec)_        	|__`Secinjpulse`__		|<big>__✓__		|				|				|
_Suction In Air Temperature (mV)_				|__`SuctionAirTemp`__	|				|				|				|<big>__✓*__
_Analog Auxiliary Input #1_                     |__`AUX1`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Analog Auxiliary Input #2_                     |__`AUX2`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Analog Auxiliary Input #3_                     |__`AUX3`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Analog Auxiliary Input #4_                   	|__`AUX4`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Analog Auxiliary Input #5_                    	|__`AUX5`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Analog Auxiliary Input #6_                  	|__`AUX6`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Analog Auxiliary Input #7_                    	|__`AUX7`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Analog Auxiliary Input #8_                   	|__`AUX8`__				|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Result of Analog Equation #1_                 	|__`Analog1`__			|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Result of Analog Equation #2_                 	|__`Analog2`__			|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Result of Analog Equation #3_                 	|__`Analog3`__			|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
_Result of Analog Equation #4_                 	|__`Analog4`__			|<big>__✓__		|<big>__✓__		|<big>__✓__		|<big>__✓__
<big>__*__</big> Implemented but untested


Analog Auxiliary Input Relationship Equations
----------------------------
It may not be convenient to view __`AUX1`__, __`AUX2`__, etc, in a gauge as is. Therefore we have implemented a simple linear equation parser for the `raspexi.cfg` file. The linear equations can be represented in the form _'y=m*x+b'_. Here, the result _'y'_ is stored in either __`Analog1`__, __`Analog2`__, __`Analog3`__ or __`Analog4`__ depending on the definition in `raspexi.cfg`, _'m'_ is a multiplier, _'x'_ is an auxiliary input (__`AUX1`__, __`AUX2`__, etc) and _'b'_ is an adder.

Two examples are as follows:

* For _'y'_ = __`Analog1`__, _'m'_ = `1.4`, _'x'_ = __`AUX1 - AUX2`__ and _'b'_ = `9.0`. The resulting line in `raspexi.cfg` could be `analog_eq1 = 1.4 * (AUX1 - AUX2) + 9.0`.

* For _'y'_ = __`Analog2`__, _'m'_ = `0.5`, _'x'_ = __`AUX3`__ and _'b'_ = `2.5`. The resulting line in `raspexi.cfg` could be `analog_eq2 = 0.5*AUX3+2.5`.

*Note: The multiplication, subtraction and addition cannot be changed and spaces in the equation are ignored.*

Custom Gauges and Dashboards
----------------------------
You can create your very own custom gauges and dashboards using __*MegaTunix*__ which you can download [__compiled here__](http://sourceforge.net/projects/megatunix/) or the [__source here__](https://github.com/djandruczyk/MegaTunix).

To create your own __Gauges__ you can use the __`MtxGaugeDesigner`__.<br>
To create your own __Dashboards__ (which can include custom gauges) you can use the __`MtxDashDesigner`__.

Once you have saved the custom Gauges and Dashboards in the XML file format you will need to make sure that the XML field element `<datasource>` contains the appropriate _'Datasource Name'_ from the table above for each `<gauge>` in the dashboard XML file.


Development
-----------
1. Download or clone the original forked MegaTunix source code using:
  ```
  $ git clone https://github.com/MarkusIppy/MegaTunix
  ```
2. Run the following to download __MegaTunix__ and install its dependencies (it may take a while to download and install these (>10mins)):
   ```
   $ cd ~/
   $ sudo apt-get install pkg-config libcurl4-openssl-dev libtool libtool-bin intltool libgtkgl2.0-dev libgtkglext1-dev g++ gcc flex bison glade libglade2-dev make git-core gdb automake1.9
   $ git clone https://github.com/MarkusIppy/MegaTunix MegaTunix
   ```
   A good step by step installation guide for MegaTunix can be found <a href="http://www.msextra.com/forums/viewtopic.php?t=23548">__here__</a>.
  (See [`README.md`](https://github.com/djandruczyk/MegaTunix/blob/master/README.md) in MegaTunix for more detail)

3. Download or clone __Raspexi__ source code with:

  ```
  $ cd ~/MegaTunix
  $ sudo git clone https://github.com/MarkusIppy/raspexi-viewer-linux raspexi
  ```

4. Add the exact line `raspexi/Makefile` to MegaTunix AutoConf configuration file (`configure.ac`)
  or apply the following patch (only needed if you use the original megatunix source instead of our fork ) 

  ```
  diff --git a/configure.ac b/configure.ac
  index e868b1b..9bec24f 100644
  --- a/configure.ac
  +++ b/configure.ac
  @@ -311,6 +311,7 @@ MegaTunix32_dbg.iss
   MegaTunix64_dbg.iss
   Doxyfile
   WIN_NOTES.txt
   raspexi/Makefile
   ])
   AC_OUTPUT
  ```

5. This step is only needed if you whish to install MegaTunix to use the gauge and dash designer on older rasbian distros .
  Edit the file MegaTunix/src/main.c  and comment out line 80 -86 by adding "//" in front of each line. This disables the    OpenGl error messages when launching MegaTunix
  ```
    /* Check if OpenGL is supported. */                                         
    if (gdk_gl_query() == FALSE) {
        g_print("OpenGL not supported\n");
        return 0;
    }
    else
        gl_ability = TRUE;
  ```
6. Execute `autogen.sh` to generate build configuration.

  ```
  $ sudo ./autogen.sh CPPFLAGS="-UDATA_DIR -DDATA_DIR=\\\"./\\\""
  ```
    
  Command line option `CPPFLAGS="-UDATA_DIR -DDATA_DIR=\\\"./\\\""`
  is need to override data directory (ex. `/Dashboards`, `/Gauges`, ...)
  of MegaTunix to working directory.
  
   If you would like to install MegaTunix so that you can create your own dashboards and gauges on the Raspberry Pi then run `sudo make install` and `sudo ldconfig`.
  
7. Build Raspexi

  ```
  $ cd ~/MegaTunix/raspexi
  $ sudo chmod +x compile.sh
  $ sudo ./compile.sh
  ```
  
8. Install associated fonts from Raspexi's GaugeFonts directory
  
  ```
  $ cd ~/MegaTunix/raspexi
  $ sudo chmod +x install_fonts.sh
  $ sudo ./install_fonts.sh
  ```
    
9. To build a binary package for deployment:
  ```
  $ sudo ./package.sh
  ```
  The binary package `raspexi-yyyymmdd.tar.gz` will then be created.


10. To use GoPro use one of the methods below to establish a Adhoc WIFI between GoPro and Raspberry Pi

  There are various ways of configuring your WIFI :
You can configure simply via [__GUI__](https://www.raspberrypi.org/documentation/configuration/wireless/), [__CLI__](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md) or
[__WICD CURSES__](http://www.raspyfi.com/wi-fi-on-raspberry-pi-a-simple-guide/)


11. Automatically start raspexi at boot without starting X Desktop (only works on linx distros which uses systemd eg Jessie)
 
First make shure your pi is set to boot to Console (you can set this in raspberry pi configuration) 

Now we need to get get the pi to boot with a blank screen instead of all the messages.
change console=tty1 to console=tty3 to redirect all the text to a different console 
change loglevel to 3 and hide the raspberry logo by adding logo.nologo
last but not least add the option -quiet .

```
$ sudo nano /boot/cmdline.txt
```
Here a example of my cmdline.txt

```
dwc_otg.lpm_enable=0 console=serial0,115200 console=tty3 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline fsck.repair=yes loglevel=3 rootwait logo.nologo -quiet 
```

Now we install Matchbox ( window manager ),unclutter to hide the mouse pointer and x11 xserver-utils
```
$ sudo apt-get install matchbox
$ sudo apt-get install x11-xserver-utils
$ sudo apt-get install unclutter
```

Create a file called startraspexi

```
$ sudo nano /home/pi/startraspexi
```
Insert the following text into startraspexi

```
#!/bin/sh
xset -dpms     # disable DPMS (Energy Star) features.
xset s off       # disable screen saver
xset s noblank # don't blank the video device
unclutter -root -idle 0.1 &
matchbox-window-manager &
/home/pi/raspexi/run.sh
```
Make startraspexi executable

```
sudo chmod +x /home/pi/startraspexi
```

Create a sercive to launch the startraspexi script at boot 

```
$ sudo nano /etc/systemd/system/raspexi.service

```

Insert the following text into raspexi.service

```
[Unit]

Description=Raspexi

[Service]

Type=simple

ExecStart=/usr/bin/xinit /home/pi/startraspexi

[Install]

WantedBy=multi-user.target

```

Test if the script works:
```
$ sudo systemctl start /etc/systemd/system/raspexi.service
```
If Raspexi is launching , quit it by pressing 'q' on the keyboard:

Now enable the script 

```
$ sudo systemctl enable /etc/systemd/system/raspexi.service
```

Reboot your pi and you should see Raspexi starting at boot 
 
```
$ sudo reboot
```


12.Launch a custom video at boot with OMXPlayer (only works on linx distros which uses systemd eg Jessie)

```
$ sudo nano /etc/systemd/system/bootsplash.service
```

Copy the following content into the file and save it ( this example assumes you have a video called "Raspexi.mp4" in the folder: "/home/pi/"  :

```
[Unit]
Description=BootSplash
DefaultDependencies=no
After=local-fs.target
Before=basic.target

[Service]
Type=simple
ExecStart=/usr/bin/omxplayer -b /home/pi/Raspexi.mp4

[Install]
WantedBy=getty.target

```


Test if the script works:
```
$ sudo systemctl start /etc/systemd/system/bootsplash.service
```
If the video is playing , then enable the script:
```
$ sudo systemctl enable /etc/systemd/system/bootsplash.service
```
Reboot your pi and you should see your video at boot 
```
$ sudo reboot
```

History
-------
Version	|Revision	|Date (d/m/y)	|Notes
:------:|:----------:|:-------------:|------
__TBA__ |	_TBA_	|TBA				|<ul><li>Added GoPro 2,3 and 3+ support  (Suryian)</li><li>Added _Primary injector duty cycle_ & _0-100km/h Timer_ datasources (JacobD)</li><li>Fixed issue where some dashboards had faint pixels/outlines around gauges (JacobD)</li><li>Minimised issue where screen would flash white when toggling between dashboards (JacobD)</li><li>Added gauges & dashboards</li></ul>
__v1.3__ |	_R7_		|10/04/2015		|<ul><li>Added _Vehicle Acceleration_, _Acceleration in G-force_, _Force exerted in Newtons_ & _Current gear number_ datasources (JacobD)</li><li>Added speed correction - calculated from a simple multiplier or from tyre sizes from the config file (JacobD)</li></ul>
__v1.2__ |	_R6_		|15/03/2015		|<ul><li>Added support for Nissan, Subaru and Toyota (by JacobD)</li><li>Added CSV log file error handling (JacobD)</li><li>Added XML dashboard file incorrect '_datasource_' error handling (JacobD)</li><li>Added linear equations to define the auxiliary relationships from the config file (JacobD)</li><li>Added _Instantaneous vehicle power_ datasource for power gauges (JacobD)</li></ul>
__v1.1__ |	_R5_		|07/07/2014		|<ul><li>Implementation of auxiliary inputs AUX1-AUX8 (SonicRaT)</li></ul>
__v1.0__ |	_R4_		|07/05/2014		|<ul><li>Revising and refactoring for public release (Markus Ippy)</li></ul>
__v0.3__ |	_R3_		|18/04/2014 	|<ul><li>Implement multiple dash board (up to 4), can be switch by key 1/2/3/4</li><li>Full screen on start</li><li>Data save to CSV file</li></ul>
__v0.2__ |	_R2_		|05/04/2014		|<ul><li>Implement PowerFC RS-232 protocol (based on fclogger.py)</li><li>Add configuration file (raspexi.cfg)</li><li>Fix issue Gauges data location</li></ul>
__v0.1__ |	_R1_		|31/03/2014		|<ul><li>Initial release</li></ul>
