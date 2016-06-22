# Teensy TinyBASICPlus
This is an interpereter for the Teensy 3.2 that is fully based off the Arduino TinyBasicPlus that can be found here:
https://github.com/BleuLlama/TinyBasicPlus
This has been fully ported to run on the Teensy 3.2 utilizing all the availble memory and eeprom. This makes it
possible to add many libraries and create a fully integrated operating system for the Teensy 3.2. This is 
my first attempt at porting an interperter so sorry if I made any mistakes and lets get going on porting the 
needed libraries for the graphics stack as well as input(keyboard or button/key matrix of some sort.)

# Plans for the future
Well as of now I have a simple project: I want to make a standalone computer out of this. But I seem to have a little problem with the needed libraries, I just can't get this to work with the tvOut(Arduino) library. The PS/2keyboard library has no problems but the tvOut seems to not have any Teensy support at all, I am still looking into it but an easyer solution is to use one of the other supported MCU's as a GPU and have it use the Serial port as a input. This can be achieved using an ATMEGA328P-PU(PN) Chip in combination with the Teensy as well, If I wanted to make a single board computer using the Teensy 3.2 and a ATMEGA328P That could happen. I am just exploring my options as of now. If you have a better idea than please email me at ddg2goodwin@gmail.com that will help me out greatly, Also if you can help with porting the tvOut library than email me as well, I am simply just not skilled enough to even attempt porting it. The next "v.1.1" release will hopefully have some extra commands(they won't be standard BASIC commands.) Or more work for the SBC project. 
