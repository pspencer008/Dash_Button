# The base of this is from here https://www.nathankowald.com/blog/2017/05/dash-button-with-raspberry-pi/

## Install the required dependencies:

### Update your software
sudo apt-get update && sudo apt-get upgrade

### Install pydhcplib
sudo apt-get install python-pip
sudo pip install pydhcplib

### Replace the script’s MAC address with yours

Replace "50:f4:de:f1:3b:a0" with the MAC address you found in step 1
It needs to be lowercase, with a colon after every two characters
dashbuttons.register("50:f4:de:f1:3b:a0", do_something)

### Make your Python script executable

sudo chmod +x /home/pi/dashbutton.py

### Test the script
Your Python script should now be able to detect your Amazon Dash button presses. Test the Python script by running:

sudo python /home/pi/dashbutton.py
Press your button and you should see the message, “button has been pressed”.

### Update the do_something function to make it do something useful


### Set the Python script to run on startup

Edit /etc/rc.local

sudo vim /etc/rc.local

Add the following to the end of /etc/rc.local (before exit 0

# Wait for an Internet connection
# If you connect using an Ethernet cable, change 'wlan0' to 'eth0'
while ! /sbin/ifconfig wlan0 | grep -q 'inet addr:[0-9]'; do
    sleep 3
done

# Network connection now exists: run Dash listener
# Change /home/pi/dashbutton.py to the path you copied the script to
# Send errors to rclocal.log <-- Useful for debugging sudo /usr/bin/python /home/pi/dashbutton.py 2>&1 /home/pi/rclocal.log &

# Make sure exit 0 is the last line of rclocal
exit 0


### Reboot and test your script

sudo shutdown -r now

You should have the dashbutton.py process running in the background, listening for presses.
To verify your Python script is running type:

ps wafux | grep dash

You should see something like:

root      2490  0.0  0.6   3756  2296 ?        S    May21   0:00 sudo /usr/bin/python /home/pi/dashbutton.py /home/pi/rclocal.log
root      2499  0.0  1.8   8904  6924 ?        S    May21   0:00  \_ /usr/bin/python /home/pi/dashbutton.py /home/pi/rclocal.log

This means the Python script is running and listening for your presses.