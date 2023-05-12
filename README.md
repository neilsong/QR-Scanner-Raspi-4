# QR scanner Raspberry Pi
![output image]( https://qengineering.eu/images/QR.webp )
## QR and barcode scanner for the Raspberry Pi 4 (64 bit OS). <br/>
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)<br/><br/>
Special made for a bare Raspberry Pi see : [Q-engineering computer vision](https://qengineering.eu/computer-vision-with-raspberry-pi-and-alternatives.html).
Specific modification of QEngineering's QR Scanner

------------

## Dependencies.
To run the application, you have to:
- A raspberry Pi 4 with a 64-bit operating system. It may have a only have Bullseye (Debian 11) operating system. <br/>
- ZBar installed.
- OpenCV installed. [Install OpenCV 4.5](https://qengineering.eu/install-opencv-4.5-on-raspberry-64-os.html) <br/>
- A working webcam.
- Gstreamer fully installed.

------------

## Installing ZBar.
You have to install ZBar first. It is a perfect library for scanning QR and barcodes. Much better and faster than the comparable OpenCV module. 
```
$ sudo apt install autopoint build-essential libv4l-dev libtool
$ git clone -b 0.23.92 https://github.com/mchehab/zbar.git
$ cd zbar
$ autoreconf -vfi
$ ./configure
```
![output image]( https://qengineering.eu/images/QR_build.webp )
```
$ make -j4
$ sudo make install
$ sudo ldconfig
```
Note that ZBar tries to work with /dev/video0, which the Bullseye operating system doesn't support (yet).<br/>
Since we're only using the decoding part of ZBar and not the ability to capture images, it won't affect our project.<br/>

## Installing Gstreamer fully
```
# install a missing dependency
$ sudo apt-get install libx264-dev libjpeg-dev
# install the remaining plugins
$ sudo apt-get install libgstreamer1.0-dev \
     libgstreamer-plugins-base1.0-dev \
     libgstreamer-plugins-bad1.0-dev \
     gstreamer1.0-plugins-ugly \
     gstreamer1.0-tools \
     gstreamer1.0-gl \
     gstreamer1.0-gtk3
# if you have Qt5 install this plugin
$ sudo apt-get install gstreamer1.0-qt5
```

### Scanning QR and/or barcodes
In the code you can configure the codes ZBar is trying to decode.
You can enable all possible codes by the single line
```
scanner.set_config(zbar::ZBAR_NONE, zbar::ZBAR_CFG_ENABLE, 1);
```
Or, if you want to scan a specific code, uncheck everything and then enable the one you want 
```
scanner.set_config(zbar::ZBAR_NONE, zbar::ZBAR_CFG_ENABLE, 0);
scanner.set_config(zbar::ZBAR_QRCODE, zbar::ZBAR_CFG_ENABLE, 1);
```
More info at [ZBar](http://zbar.sourceforge.net/api/zbar_8h.html#f7818ad6458f9f40362eecda97acdcb0).

------------

## Installing the app.

- Make sure you have OpenCV up and running on your system.<br/>
- Choose the folder with your operating system (Buster or BUllseye, 32 or 64 bit).<br/>
- Download the files.<br/>
- You can either build the app with Code::Blocks (`$ sudo apt-get install codeblocks`) or use CMake.<br/>
### Code::Blocks
Load the project file `QR.cbp` and run the app with F9.<br/>
For more info on how to work with the Code::Blocks IDE see our [tutorial](https://qengineering.eu/opencv-c-examples-on-raspberry-pi.html).<br/>
### CMake
```
mkdir build
cd build
cmake ..
make
```
![output image]( https://qengineering.eu/images/QRpi_CMake.png )<br/>
After the build you find the QRpi app in the buiild folder.<br/>
.<br/>
├── build<br/>
│   ├── CMakeCache.txt<br/>
│   ├── CMakeFiles<br/>
│   ├── cmake_install.cmake<br/>
│   ├── Makefile<br/>
│   └── **_QRpi_**<br/>
├── CMakeLists.txt<br/>
├── main.cpp<br/>
└── QRpi.cbp<br/><br/>

------------

## Final remarks.

- If using USB webcam, make sure to keep 16:9 aspect ratio when sampling Gstreamer. It is cropped to the biggest square for ZBar later.

![output image]( https://qengineering.eu/images/QRsucces.png )

