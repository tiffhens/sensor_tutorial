# How to Connect Analog Sensors to Your Raspberry Pi - A Tutorial
  The Raspberry Pi is a microcomputer that can be used to control digitial inputs 
  and outputs. This tutorial explains how to connect an analog sensor to your Raspberry Pi 
  (henceforth known as Raspi). This is a two-step process, because out of the box, 
  the Raspi only reads digital signals, so a converter must be added to the Raspi 
  in order to understand analog sensors and signals. Don’t worry, we can do this. 
  We have the technology.

## Materials List
* Raspberry Pi 2
* ADS1015
* potentiometer, light sensor
* Breadboard(s) and jumper wires

## You need this shit before you can do other shit
  This tutorial assumes that you are curious about the Raspberry Pi and have one, 
  already have done a few things to prepare, and you have the materials listed above. 

1. [Connect][1] to your Raspberry Pi. We are using SSH.  
2. Load the OS: Make sure it’s running [raspbian jesse][2]
3. Configure [I2C][3] on your raspi
4. [Download the data sheet][4] for reference.

[1]: https://www.raspberrypi.org/documentation/remote-access/ "Connect to your Raspi"
[2]: https://www.raspberrypi.org/downloads/raspbian/ "Raspbian Jessie"
[3]: http://example.org/ "Title"
[4]: https://cdn-shop.adafruit.com/datasheets/ads1015.pdf "Data Sheet"

## Assembly/Wiring Instructions

  Pay attention to the orientation of the pins that connect your breadboard to your Raspi as to not break your materials. 

  Connect the ADS1015 Board to the Raspberry Pi as follows, using a breadboard
  and jumper wires in order to give yourself more room.
  
  Connect the ADC to the Pi as follows: 
  * ADS1x15 VDD to Raspberry Pi 3.3V 
  * ADS1x15 GND to Raspberry Pi GND 
  * ADS1x15 SCL to Raspberry Pi SCL 
  * ADS1x15 SDA to Raspberry Pi SDA

![Wiring Photo][1]
[1]:https://c1.staticflickr.com/8/7770/29638761944_b968dc30c2_b.jpg


## Libraries - What You need to Install and Why

#### Note: during this step your Raspberry Pi must be connected to the internet

Now that you’ve finished wiring, you’re ready to install the Adafruit ADS1x15 Python Library to use the ADC.
It is available here on [Github](https://github.com/adafruit/Adafruit_Python_ADS1x15)

In the following sequence, we install python tools, clone the github repo, and change the directory. 

Open a terminal on the Raspberry Pi and run the following commands:

```
sudo apt-get update
```
press enter 

```
sudo apt-get install build-essential python-dev python-smbus git
```

press enter 

```
cd ~
```
press enter 

```
git clone https://github.com/adafruit/Adafruit_Python_ADS1x15.git
```
press enter 

```
cd Adafruit_Python_ADS1x15
```

press enter 

```
sudo ipython3 setup.py install
```

When you’ve finished and successfully installed this library, you’ll see something similar to this:

![Library Screenshot][2]
[2]:https://c5.staticflickr.com/9/8130/29972694740_fb14823849_b.jpg

#### Next, install this Package manager for python libraries.
  Pip functions similarly to the package manager Aptitude, however it is specifically for Python. 
  Pip checks to see if we are missing anything we might need to use these applications. 

To install from the [Python package index](http://adafru.it/lfJ) connect to a terminal on the
Raspberry Pi and execute the following commands:

```
sudo apt-get update
sudo apt-get install build-essential python-dev python-smbus python-pip
sudo pip install adafruit-ads1x15
```

## Learning how to use the library -- Testing 

  Now that we’ve installed the libraries necessary to use the converter, let's walk through some example 
  code to learn about how they work. These examples are available in the examples folder if you downloaded 
  and installed the library from source. 

1. Change to that folder by running in Terminal on the Pi:

```
cd ~/Adafruit_Python_ADS1x15/examples
```

Then, we'll start by looking at the simpletest.py example which is a basic example of reading and displaying the ADC channel values.  

 Run the following command to open the file in the nano text editor:

```
nano simpletest.py
```

Next, scroll down to the following block of code near the top:

```
# Create an ADS1115 ADC (16-bit) instance.
adc = Adafruit_ADS1x15.ADS1115()
 
# Or create an ADS1015 ADC (12-bit) instance.
#adc = Adafruit_ADS1x15.ADS1015()
 
# Note you can change the I2C address from its default (0x48), and/or the I2C
# bus by passing in these optional parameters:
#adc = Adafruit_ADS1x15.ADS1015(address=0x49, bus=1)
```

This code configures the example to use either an ADS1115 chip, or the ADS1015 chip.  The top uncommented line is choosing to use the ADS1115 chip by creating an ADS1115 object.  Below it is a commented line that instead creates an ADS1015 object to use the ADS1015 chip.  

Since we are using the ADS1015 chip, uncomment the line that includes ADS1015 and comment out the line including “adc = Adafruit_ADS1x15.ADS1115()” 

Your code should now look like this:

```
# Create an ADS1115 ADC (16-bit) instance.
# adc = Adafruit_ADS1x15.ADS1115()
 
# Or create an ADS1015 ADC (12-bit) instance.
adc = Adafruit_ADS1x15.ADS1015()
 
# Note you can change the I2C address from its default (0x48), and/or the I2C
# bus by passing in these optional parameters:
#adc = Adafruit_ADS1x15.ADS1015(address=0x49, bus=1)
```

Now save the file by pressing Ctrl-o, enter, then Ctrl-x to quit. 


#### Run simpletest.py to see if everything works.

There are several interpreters that can read Python. We use iPython3. If you haven't installed it,
you can find and install it [here](https://ipython.org/install.html).

Use the iPython3 interpreter to run the simpletest example:

```
sudo ipython3 simpletest.py
```

If everything is wired correctly, the example will print out a table with all of the ADC channels and their 
values.  Every half second a new row will be printed with the latest channel values.  

You will see output that looks something like this:

![Column Screenshot][3]
[3]:https://c8.staticflickr.com/8/7721/29639249863_b05e2b79df_b.jpg

## Actually doing shit - Connecting an analog sensor
  Once we know that everything is hooked up correctly, we can start adding analog sensor input.
  You'll see on the converter letters that read A0 through A4. These are your analog inputs. You can
  add up to 4 analog sensors. We will start with a potentiometer, aka knob. The potentiometer has three 
  signals - ground, power, and signal, and this tutorial assumes that you have attached/soldered wires
  or jumper cables.

#### Potentiometer Wiring

  To connect the knob, first disconnect power from your Raspi, just to be safe. Plug in again once
  everything is connected. Do this every time you add more wires.
  
  Share the power and ground from your ADC, and connect the yellow signal wire into A0, 
  as shown:
  
  ![](https://c7.staticflickr.com/6/5749/29972694830_21a348dab8_b.jpg)
  
  Once connected, use your terminal to first go back to the Adafruit directory:
  ```
  cd ~/Adafruit_Python_ADS1x15/examples
  ```
  Then open simpletest.py once again, and notice that when you turn the knob, The first column's values changes. You can guess that when you connect other sensors, the values will change in those columns.
  

  
## Level Up, bro. Add more Sensors

  Why not just stop at one? We'll show you how to add one more sensor, a photocell, aka light sensor.
  
  To connect the photocell, this tutorial assumes that you have spliced the ground wire to include a signal
  cable. To understand why and how to do this, visit this [other tutorial](https://www.raspberrypi.org/learning/physical-computing-with-python/ldr/).
  
  Connect your photocell/light sensor using the same method as the knob above. Share power and ground, and insert
  the signal cable into A1, as shown.
  
  ![Light Sensor Wiring Photo][5]
  [5]:https://c5.staticflickr.com/8/7576/29972694700_ace1d67e8c_b.jpg
  
  Once you've done this, go back to Terminal and run simpletest.py once again. When you cover up the light 
  sensor, you'll see the values in the second column vary.
  
  Congrats! You have connected your Raspi to an digital-to-analog converter, installed all the libraries
  necessary to make it work, and connected not one, but TWO sensors. You should feel proud.
  
  ...now go have a drink. Have one for me, too.
