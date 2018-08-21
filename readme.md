﻿ # Raw IR Data Recording and Viewing
 
# TAGS: IR, Thermal Imager, Lepton 3.5, PureThermal 2, Raspberry Pi, Linux, Ubuntu, Windows, Python, OpenCV, Matplotlib, VNC, Wireless Control

## Description

I wanted an infared, thermal imaging camera for my job that could be easily controlled remotely from a PC and record raw IR data. The Lepton 3.5 along with the purethermal 2 (PT2) board provided a lot of options integrated with a raspberry pi. Looking through Groupgets Github software GetThermal and purethermal1-uvc-capture, I was able to piece together two user interfaces, IR Data and IR Data Viewer, combining opencv and matplotlib with pyqt4.

## Table of Contents:

- IR Data Ablilities
- IR Data Viewer Abilities
- Revision Changes
- Downloading, Installing, and Running Software
- Important Notices
- UI Developement

## IR Data Abilities

The GitHub folder IR Data has the python script irdatavXX.X.py and a .ui file.

IR Data Can:
- View the IR Camera 9 Hz Stream
- Display Max and Min Temperatures
- Record Raw Data as HDF5 without overloading raspberry pi CPU
- Save to Specific Filenames and Directories
- Be Controlled Wirelessly using RealVNC

Screenshot of IR Data Software

![Alt text](/images/irDataStreaming.png?raw=true)

In order to control the IR Camera wirelessly, I was able to use a VNC Session between a PC computer (or a Smart Phone!) and the Raspberry Pi connected to the same network (WIFI). Follow the below link to learn how to control your raspberry pi wirelessly.

https://www.realvnc.com/en/raspberrypi/

If you purchase the raspberry pi touchscreen, you can also very easily have a handheld IR camera if you don't have access to a wirless network and can't just use your phone. The UI file would most likely need to be resized/adjusted using Qt Designer mentioned in UI Development at the bottom of this document in order to properly fit the UI on the 800x400 screen.

## IR Data Viewer Abilities

The folder IR Data Viewer has the python script irDataViewervXX.py and another .ui file. IR Data Viewer is a post processing script designed to take in the binary .HDF5 files and process them into a matplotlib figure for analysis.

IR Data Viewer Can:
- View .HDF5 Recorded Raw Data in matplotlib format
- Save .AVI Files of specific length
- Save PNG Images
- Generate TIFF Files for further processing in MATLAB or GNU Octave
- View Temperatures at any pixel on the frame
- Zoom in and analyze your data in depth
- Executable files are included for Linux and Windows Machines!

Screenshot of IR Data Viewer Software

![Alt text](/images/irDataViewerSelected.png?raw=true)

Special thanks to the developers of GetThermal and the Flir Community Forum who helped me achieve my goals in this project.

I purchased the Lepton 3.5 and PT2 from the below link:

https://groupgets.com/manufacturers/getlab/products/purethermal-2-flir-lepton-smart-i-o-module

## REVISION CHANGES

### IR Data

- v12 = Works, but crashes on pi
- v14 = Uses new .ui and doesn't crash on pi
- v15 = New save format
- v16 = Improvments all around

### IR Data Viewer

- v10 = Open and review .hdf5 files

## HOW TO GET THIS SOFTWARE RUNNING ON YOUR PI OR LINUX PC

### SYSTEM UPDATE/INSTALL PACKAGE COMMANDS

If you have not already, please connect your pi or linux computer to the internet and open up the terminal. Most of these commands download the python packages necessary to run IR Data on your linux os. Important Note: These scripts were written using Python 2.7, not Python 3.

Terminal Commands:

	sudo apt-get update
	sudo apt-get upgrade
	sudo apt-get dist-upgrade
	sudo apt-get install python-qt4
	sudo apt-get install python-opencv
	sudo apt-get install python-tifffile
	sudo apt-get install python-h5py
	sudo apt-get install python-psutil
	sudo apt-get install git
	sudo apt-get install libusb-1.0-0-dev
	sudo apt-get install libusb-1.0
	sudo apt-get install build-essential

### DOWNLOAD SOFTWARE FROM GITHUB

These terminal commands downloads the software from GitHub. They also install the libuvc package develped by Groupgets to use the PT2 board with Linux/MAC OS.

Terminal Commands:

	cd Documents
	git clone https://github.com/Kheirlb/purethermal1-uvc-capture.git
	cd purethermal1-uvc-capture
	cd python
	git clone https://github.com/groupgets/libuvc
	sudo apt-get install cmake
	cd libuvc
	mkdir build
	cd build
	cmake ..
	make && sudo make install
	sudo ldconfig -v
	cd ../..

### RUNNING IR Data

Make sure the PT2 is plugged into your pi or other linux computer.

Terminal Commands:

	cd IR_Data
	sudo python irdatavXX.X.py

### RUNNNING IR Data Viewer - Post Processing Script

Terminal Commands:

	cd IR_Data_Viewer
	sudo python irDataViewervXX.py

Or As An Executable:

	irDataViewervXX

There is also a Windows executable for IR Data Viewer.

## IMPORTANT NOTICES

### For Raspberry Pi
Might have to run .py files as sudo (admin)

Your Raspberry Pi should have the latest version of Rasbian Strech with Desktop installed on the SD Card. Follow this link  https://www.raspberrypi.org/downloads/raspbian/ and install the ZIP image onto your SD Card using your favorite SD Card writer (My favorite is Etcher found at https://etcher.io/. It does not require you unzip the file).

### For IR Data
You can notice in the first Screenshot under IR Data Abilities the pixelation of the IR images; this is because it is being resized from 120X160 to 480x640 using Qt's resize function only, the cv2 resize function is commented out. IR Data is configured to run on a raspberry pi 3b+ at around 45% CPU usage (cv2.resize takes up too much resources on the pi causing the software to crash). If you are running on a stronger computer, simply uncomment the cv2.resize function from irDatav16.5.py script.

Unfortunately, Perform FFC and changing Gain Modes features on IR Data are still unavailable. If anyone wants to help me develop these features, please reach out to me or take it on yourself.

### Old FYI for Previous Versions

- v12:
	- All Raw Data is Save to Same Directory as .PY
	- Use .m script and Octave GNU to process .tiff raw data files.

## UI Development

If you would like to change the user interface and do more development of your own, I used Qt Designer.

Install Qt Designer:

	sudo apt-get install qt4-designer

Or Qt Creator:

	sudo apt-get install qtcreator

### Helpful Links

- https://pythonspot.com/qt4-file-dialog/
- https://stackoverflow.com/questions/4286036/how-to-have-a-directory-dialog-in-pyqt
