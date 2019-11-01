# Remeha Avanta V1 P3 Boiler connectivity using ESP8266
Connect ESP8266 directly to a Remeha Avanta V1 P3 CV/Boiler to read data using PHP.

The PHP code uses an Adafruit Huzzah ESP8266 to connect to and read data from a Remeha Avanta V1 P3 boiler. 
The Huzzah ESP8266 was chosen as it is a 5VDC device and does not require any level shifter or additional circuits to deal with the higher voltage. 

# What do you need
1. Adafruit Huzzah ESP8266 flashed with the ESP-Link firmware/software	
	(https://learn.adafruit.com/adafruit-huzzah-esp8266-breakout/overview)
2. Domoticz
	(https://www.domoticz.com/)
3. Webserver 
	(See packages, you can install it on your Domoticz RPi)
4. Packages
	(sudo apt install apache2 php7.3 libapache2-mod-php7.3 php7.3-cli php7.3-fpm php7.3-json php7.3-zip php7.3-mbstring php7.3-curl php7.3-xml php7.3-json)

# How to attach the Adafruit Huzzah ESP8266 to the Avanta V1 P3
Before you start, you will need a external 5v power supply with the Avanta since it can't provide enough power.
Using the VDC (pin4) to power your Adafruit will boot loop your boiler since it can't provide enough power.

Connect to the Remeha X13 connector using a 4P4C (RJ10) connector with the following pinouts:

Remeha >>>>>>>>>> ESP8266
1. Pin1 (GND)........> GND
2. Pin2 (RX).........> TX
3. Pin3 (TX).........> RX
4. Pin4 (VDC)........> Do not use (prevents boot loops of the boiler)

I'm not responsible for any damage in any way.

# How to send the data to Domoticz

1. Add a Dummy under the Hardware tab
2. Create Virtual Sensor at the Hardware tab (easiest is to use the Domoticz Devices naming of remehe.ini)
3. Change the IDX numbers in remeha.ini according to the virtual sensor IDX's
4. Run "php7.3 /var/www/html/remeha/remeha-counters.php" and "php7.3 /var/www/html/remeha/remeha-samples.php" on your webserver (or create a crontab to automate)
5. Domoticz should now be updated with the data

# How it works
Currently this is a 'read only' script and provides the following functionality:

1. Connects ESP to Avanta (set Baudrate of ESP-Link to 9600 8N1)
2. Sends Hex to Avanta and reads the responses (for "Sample Data", "Counter Data" and "Parameters")
3. Maps response to more 'logical' variables
4. Translates various 'bits' to provide correct messages
5. Writes information received to Domoticz server using cURL
6. The remeha-xxxxx.php scripts can be run from within a browser, while the remeha.php script is best run as a daemon or in the background

# Troubleshooting
Q: 	My Boiler is not providing hot water anymore, and shows an error
A: 	This mostly happens when someone is using the VDC (pin4) of the boiler to power the Adafruit. 
	Remove the VDC from the Adafruit. Now press and hold R on your Boiler, it will reset and reboot.

Q:	I have no idea whats wrong!
A:	Create an issue, i might be able to help

# Credits
juanelc - Creating and testing the scripts for Avanta V1 P3