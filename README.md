# PCB-Printer-Tiva-Firmware
Firmware for TI TIVA control of laser exposing PCB printer

This code is designed to run on a TIVA TM4C123 processor, and is designed to work with the launchpad pinout.

The key features are as follows...

TIVA USB interface presents as a virtual COM port. This means it does not require any additional USB-serial converters such as the FTDI etc... The VCOM is native. This also means that the baud rate and other settings are completely unnecessary since they com port is never converted back into a physical COM port. It also means it runs at a USB rate of 12MBits/second. Obviously real throughput is lower, but is significantly higher than cnverting back to a physical UART.

There is a command processor which accepts text commands to configure things like printing resolution, source bitmap resolution, image polarity etc...

The serial interface also looks for bitmap images sent to the COM port. This means that in order to actually start printing all you need to do is 'copy' the source bitmap to the COM port.

You can flip the image and render from the opposite edge of the platform. This makes exposing double sided boards much simpler since as long as you have parallel edges and a perpendicular leading edge then positional accuracy will always be excellent. You will need to tell PCB printer what the spacing between edges is though, but that should be fixed.

You can setup all your specific configuration in a text file then simply 'copy' the text file to the COM port, then 'copy' your PCB bitmap file to the COM port.

No additional computer side software is necessary to drive the PCB printer. No converting the G-Code, nothing, nada, just copy the bitmap image to the COM port :)

The mechanical stepping resolution, the laser motion resolution and the source bitmap image can all be completely and independently different. This should make porting the code to mechanical systems with various drive mechanisms simple. The laser motion resolution will need to be tuned to your specific laser since it will depend on how large the laser dot is. The source bitmap resolution can be whatever you want, although matching the motion resolution will give the best results since there will be no scaling artifacts (these will be minimal anyhow, but hey...). One reason you may need to change the sourece bitmap resolution is your PCB tools. Some PCB tools will only generate bitmaps of fixed sizes (I know, come on... is this 1980???). If you use my recomended Gerber to BMP tools though it is not an issue.

VERY basic acceleration is implemented and is really such that the head is up to speed before starting to actually print.

I am having issues wither backlash repeatability at the moment so there is an option to turn on uni-direction printing. Backlash correction is kinda implemented, but I have not fine tuned it yet.

Support for the common 240x320 SPI LCD with a touch screen is also included to display configuration information and progress image display. There are many functions for buttons, images, frames, fonts, drawing etc... but I will clean them up and document them later.

Currently the touch screen is not actually used since I have not had time to do anything useful with it yet. This will come once my mechanical platform is robust. I have also included a rudimentary menu framework which is very easy to call functions with parameters and sub menus, but again, there is nothing behind it yet and I will clean it up substantially when I get a chance.

To generate a suitable bitmap you can either export a bitmap from your PCB tool, or you can convert your Gerber files to BMP. I recommend using Gerbv, which is a free and open source tool (http://gerbv.geda-project.org/).

Bitmap files supported are 32 bit, 24 bit and 1 bit monochrome. For larger PCBs it is recommended to use 1 bit monochrome. For 32 & 24 bit images the green channel is used to determine laser state, with green valuse < 128 corresponding to 'set' pixels. The image polarity can be inverted depending on requirements for the specific image source and for specific photo sensitivity polarity.

The terminal interface is also configurable for things like echon on/off and carriage return/new line formatting.

EEPROM storage is kinda there but again, I have not bothered yet. The basics are included and will get cleaned up later.

Other misc options are enable/disable the laser, turn on/off the laser (used to help focus), home the carriage, move the carriage, enable/disable home before print, enable/disable moving the head out of the way after printing, mirror the image, flip the image and invert the image.

Hopefully I have split things up so that you can either help clean this branch up or, so that you can grab what might be beneficial to you for your own platform and/or project.
