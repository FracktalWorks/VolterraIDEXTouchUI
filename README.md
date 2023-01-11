# IdexTouchUI
> This respository contains the step-by-step guide to configure the touch UI of Idex machine ,including files used and links 
---

## OCTOprint TOUCHUI configuration 
* Refer this documentation for the intial setup of the Touch ui in RaspberryPI octoprint server [Julia-Touch-UI-Documentation](https://github.com/FracktalWorks/Julia-Touch-UI-Documentation)

## Configuring the ``` Waveshare 5 inch display ``` to the Raspbery PI octoprint server
* Connect the waveshare 5 inch display to the raspberry pi HDMI port
> **Note :**
if you are using the raspberry pi 4 then use the ```HDMI to micro HDMI converter``` as by default you don't have any HDMI port in it.

* Follow this blog to do rotation of the display from landscape to portrait  [Rotating the Raspberry Pi display ](https://howchoo.com/pi/raspberry-pi-display-rotation)

> **Note :**
This rotation of the display only works for the ```Raspberry Pi displays``` ,there are the extensions for the other displays **check out the official documentation of the respective displays** for configuration of the displays to configure with Raspberry pi.

## Connecting the ```Raspberry pi octoprint server``` wirelessly with the help of ```bitwise ssh``` :  

- Download bitwise ssh from here **[Bitwise ssh - client](https://www.bitvise.com/ssh-client-download)**

- Connect to the ip address of the idex machine and use default port : 22 and login ID & passwords.

- Now open the ```New terminal console```.


## RASPBERRY PI/ Wave share SCREEN ROTATION:
### 1. Edit boot/config.txt
- From your computer, open Terminal (Mac) or Command Prompt (Windows) and connect to your Raspberry Pi via SSH.

- Then, run the following command to edit the file:

 ```sudo nano /boot/config.txt```

### 2. Rotate the Raspberry Pi display

 insert one of the following values :
 
 ``` 
display_rotate=0 # Normal (landscape)
display_rotate=1 # 90 degrees (portrait, upside down) <--- this is the we used for this touch ui.
display_rotate=2 # 180 degrees (landscape)
display_rotate=3 # 270 degrees (portrait, upside down)
display_rotate=0x10000 # horizontal flip
display_rotate=0x20000 # vertical flip
 
 ```

### 3. Rotate the Raspberry Pi touchscreen

If you're using a touchscreen, you'll also need to adjust touchscreen rotation. Otherwise, your taps will all be on the wrong part of the screen! This will adjust both display and touchscreen rotation.
 > Note :
 *This won't work for the wave share , as in officaial raspberry pi documentation you might be getting only this reference including many blog's*
Insert one of the following values at the end of the file:

```
lcd_rotate=0 # default (normal landscape)
lcd_rotate=1 # 90 degrees (portrait, upside down)
lcd_rotate=2 # 180 degrees (landscape)
lcd_rotate=3 # 270 degrees (portrait, upside down) 
```

###instead use this below method for fruitful result. ```(as we are using waveshare 5inch  display & may vary for even other waveshare versions)```

1. install libinput
```sudo apt-get install xserver-xorg-input-libinput```

2. Create the xorg.conf.d directory under /etc/X11/ (if the directory already exists, proceed directly to step 3).
```sudo mkdir /etc/X11/xorg.conf.d```

3.Copy the 40-libinput-conf file to the directory you created just now.
```sudo cp /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/```

4.Edit this file.
```sudo nano /etc/X11/xorg.conf.d/40-libinput.conf```

```
# Match on all types of devices but joysticks
#
# If you want to configure your devices, do not copy this file.
# Instead, use a config snippet that contains something like this:
#
# Section "InputClass"
#   Identifier "something or other"
#   MatchDriver "libinput"
#
#   MatchIsTouchpad "on"
#   ... other Match directives ...
#   Option "someoption" "value"
# EndSection
#
# This applies the option any libinput device also matched by the other
# directives. See the xorg.conf(5) man page for more info on
# matching devices.

Section "InputClass"
        Identifier "libinput pointer catchall"
        MatchIsPointer "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection

Section "InputClass"
        Identifier "libinput keyboard catchall"
        MatchIsKeyboard "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
 EndSection
 
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsKeyboard "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection 

 Section "InputClass"
        Identifier "libinput touchscreen catchall"
        MatchIsKeyboard "on"
        Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection

Section "InputClass"
        Identifier "libinput tablet catchall"
        MatchIsKeyboard "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
 EndSection
 ```

Find the part of the touchscreen, add the following statement inside, and then save the file.

```Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"```  (Because we want portrait orientation )

After completing these steps. The LCD could rotate 90 degrees both display and touch.

> Note:

90 degree: ```Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"```

180 degree: ```Option "CalibrationMatrix" "-1 0 1 0 -1 1 0 0 1"```

270 degree: ```Option "CalibrationMatrix" "0 -1 1 1 0 0 0 0 1"```

the press ```ctrl + x``` and then press save (y/n)? ```Y``` , the press enter file will be saved .

 ### 4. Restart your Pi

```sudo reboot```

# Making the TouchUI for the machine 

As the orientation of the display to the machine is portrait mode (i.e, 90 vertically) , I have used the [Volterrra Touch UI](https://github.com/FracktalWorks/Volterra400Dual2022TouchUI) as a reference and clone the repository.

-  Open the **MainGUI.ui** file in the cloned repository using **Qt Designer**.

# Installing the Qt designer in windows 

Text based tutuorial : https://www.techwithtim.net/tutorials/pyqt5-tutorial/basic-gui-application/ Setting up with

1) you can use the official *[Qt designer](https://build-system.fman.io/qt-designer-download)* site for downloading the .exe setup file and install it

2) Or you can you the command line installation as suggested below

- Open **command prompt** and paste the below command

```pip install PyQt5Designer```

```pip install Pyqt5-tools```

```pip install pyuic5```

now search for the desinger.exe 

**[Tutorial of Qt designer](https://www.youtube.com/watch?v=Vde5SH8e1OQ)**

-  Make the required changes in the file using  Qt designer software (Open ```.ui``` files with qt desinger only ).


-  Then try to compile the .ui file to .py file using the below steps mentioned

**Installation of pycharm**  Pycharm: https://pythonpyqt.com/how-to-install-pyqt5-in-pycharm/


# Converting the Qt designer .ui file to .py file using pycharm external tools

Lets learn how to..,

[Convert Qt Designer ui file to Python file using  Pycharm External Tools](https://youtu.be/1CZ2RZ6ffRo )
______________________________________



1) Setup External tools in pycharm.

- Click on the ```File > settings``` option on the top left and click on the ```tools > external tools``` click on ```Create tool / Add icon +``` 

2) Convert .ui file from pyqt5 to .py (python) file without any codes and one time setup.

Arguments : ```$FileName$ -o $FileNameWithoutExtension$.py```

Working Directory : ```$FileDir$```

- I am not totally mentioning the full procedure here as **I have attched the youtube link for the referece** so that you can have the full clarity while he explains the step-by-step process for it .

now type use the ommand  ```pyuic5.exe .\mainGUI.ui -o .\mainGUI.py```  or ```pyuic5 .\mainGUI.ui -o .\mainGUI.py```in terminal of pycharm.

## Making changes in the main.py 

This is the main python file that acts a backend for the touch UI as it process the parameters and elements of the mainGUI and process them
* Remove/ replace/ add the required widgets , elements that are added in the GUI/.ui file which is being converted into the .py file so that there won't be any errors while execution 

## Making & configuring the octoprint plugin for the Idex twin dragon :

* Check this documentation for geting started wth octoprint plugins & configuring the setup.py , _init_.py and the basic setup of the plugin : https://docs.octoprint.org/en/master/plugins/gettingstarted.html

* Check this documentaion for the development of the plugins in octoprint : https://docs.octoprint.org/en/master/plugins/index.html 

* Check with ```setup.py```, ```versioneer.py```, ```_init_.py```, ```MANIFESTATION.in``` , ```setup.cfg``` etc., other files what so ever are applicable for the project.

# Making the Github Commit & uploading the files of the project repository

* check the files without errors ,put a description of the changes and make the Git pull & make the commit to your changes in the main branch 

* for the plugin make a new release of the plugin and put a description of the changes and set a release.

# Trouble shooting

## Make sure before You make the release of the plugin make the repository from private to public or else it won't install

## How to degrade the pip version in octoprint server 

https://community.octoprint.org/t/i-cannot-install-uninstall-update-plugins-update-octoprint-or-restore-a-backup-i-get-a-unicodeencodeerror-latin-1-codec-cant-encode-characters/42684


Refer this documentation for the degradation of the pip version of the octoprint : https://gist.github.com/foosel/4d2d34d7689a07b6d506d9508fc115f4

## You can see the output , test it and make changes according to the requirement.
