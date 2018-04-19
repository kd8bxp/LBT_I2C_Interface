# Little Buddy Talker I2C Interface.

This is an expansion project for the LBT, which is an SPI device.

The project came about because of a limitation of both some microcontrollers,  
and the LBT.  
The idea here is to use a 2nd microcontroller in this case an Arduino Nano connected to the LBT via SPI, the Nano is then connected to your main controller via I2C.  
The nano runs a part of the Word100 Library.  
https://github.com/kd8bxp/Word100  
The library is NOT required for this code and what is needed is included in the sketch directory.  
There are pictures in the picture directory. And a video of this working on a MAKERBuino, and UNO can be found here.  
https://youtu.be/NtPCaOJfA08  

The Little Buddy Talker was made by Patrick Thomas Mitchell  
http://www.engineeringshock.com/the-little-buddy-talker-project-page.html  

## Installation

Run the LBT_I2C_Slave_code_updated on the nano, connect the LBT to the nano as you normally would.  
PIN D1 to nano D11  
PIN SC to nano D13  
PIN CS to nano D10  
  
The I2C address is 0x08. You'll connect your second (main) microcontroller to SDA and SCL pins.(A4 and A5 of the nano).  You can power the nano using VIN or via the usb port (DON'T USE VIN if you use the USB port). Either way you'll also want to connect a ground to each microcontroller.  

LBT_I2C_sender_updated is some demo code to show how this works.  
I plan on turning this code into a full library, but it should give an example of how it works now.  

## Versions

Version 1.0.1 Apr 15, 2018 - Initial release of the code

## Usage

See the example/demo code lbt_i2c_sender_updated  

## Limitations 

Not everything works the way I wanted.  
1) setDelay() doesn't really work, you have to control the delay on the master controller side. This is a delay between words, 700 is the default and works the best but for some short words setting the delay shorter does work.  
2) getDelay() works - but since setDelay() doesn't really do anything is kind of pointless. And may only be good for if you forgot what the delay is(??).  
3) sayNumber() doesn't work at all - I have an idea as to why, but the easy fix will be to add that function to the local (master) library.  
4) setAMPM() works! getAMPM() works!  
5) sayHours() appears to work! - Which is strange to me since sayNumber() doesn't work, and effectly it's the same code (???????????) can't tell I am confused here.  
6) getSpeaking() the idea here was to see if the LBT device was speaking - this doesn't work at all, I'm really not sure how to fix it, or if it can be fixed.  
7) sayMinutes() doesn't work, but saying AM/PM does (?) again I'm not sure why this doesn't work, and sayHours() does work. For the most part sayNumbers(), sayHours(), and sayMinutes() all use more or less the same code, so (??????)  

## I2C Registers

When sending information to the LBT I2C interface, we send two numbers, the 1st is the command register, the 2nd will be dependant on what the command is.  
0x00 Status set the status register number - which is used to get information back from the I2C slave device. IE: 0x00 followed by 0x01 will send back if the AMPM flag is set.  
0x01 Say followed by HEX vaule of a word.  
0x02 sayNumber followed by a int number (NOT WORKING). * Most likely will be removed if/when I write a real library for this.  
0x03 sayHours followed by a number between 0 and 22.  
0x04 sayMinutes followed by a number (NOT WORKING)  
0x05 setAMPM folowed by a bool (TRUE/FALSE) default is TRUE or say AM/PM.  
0x06 setDelay followed by a int (Works but doesn't really do anything.)  

Status Registers  
There are 3 status registers  
First you need to set which register you want status from by using 0x00.  
0x01 will return a bool of AMPM (1 is true which is say AM/PM)
0x02 will return if the device is Speaking (DOESN'T WORK) It was a good idea that I'm not sure if it will ever work or not.  
0x03 will return the delay set time.  

*See the demo code on how to use the status registers.  

## Future Plans

1) Fix the above issues.  
2) Turn this into a full library and include it with the Word100 Library.  
3) Expand it to use the Big Buddy Talker, which isn't out yet, but on it's way.  
https://www.kickstarter.com/projects/172204344/the-big-buddy-talker-talk-with-your-arduino-and-ra/description  

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request

## Support Me

If you find this or any of my projects useful or enjoyable please support me.  
Anything I do get goes to buy more parts and make more/better projects.  
https://www.patreon.com/kd8bxp  
https://ko-fi.com/lfmiller  

## Other Projects

https://www.youtube.com/channel/UCP6Vh4hfyJF288MTaRAF36w  
https://kd8bxp.blogspot.com/  


## Credits

Copyright (C) 2018 LeRoy Miller  

## License

This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses>
