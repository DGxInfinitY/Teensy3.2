# Teensy TinyBASICPlus
This is an interpereter for the Teensy 3.2 that is fully based off the Arduino TinyBasicPlus that can be found here:
https://github.com/BleuLlama/TinyBasicPlus
This has been fully ported to run on the Teensy 3.2 utilizing all the availble memory and eeprom. This makes it
possible to add many libraries and create a fully integrated operating system for the Teensy 3.2. This is 
my first attempt at porting an interperter so sorry if I made any mistakes and lets get going on porting the 
needed libraries for the graphics stack as well as input(keyboard or button/key matrix of some sort.)

# Plans for the future
Well as of now I have a simple project: I want to make a standalone computer out of this. But I seem to have a little problem with the needed libraries, I just can't get this to work with the tvOut(Arduino) library. The PS/2keyboard library has no problems but the tvOut seems to not have any Teensy support at all, I am still looking into it but an easier solution is to use one of the other supported MCU's as a GPU and have it use the Serial port as an input. This can be achieved using an ATMEGA328P(Arduino) Chip in combination with the Teensy as well, If I were able to sort out the serial connection between them I could make a single board computer using the Teensy 3.2 and a ATMEGA328P. I am just exploring my options as of now. If you have a better idea or if you can help with porting the tvOut library than email me at ddg2goodwin@gmail.com , I am simply just not skilled enough to even attempt porting it. The next "v1.2" release will hopefully have some extra commands(they won't be standard BASIC commands.) Or more work for the SBC project. 

# Release
##### Version 1.1 has been released!

Just minor bug fixes and what not as well as the official name in the boot up text. It now also has an LED boot up sequence just in case you didn't think it was working. You can turn this LED off by typing DWRITE 13, LOW and that will turn it off if you like.

# Connection Instructions
The serial port that you need to connect to it can be found out using the Arduino IDE, then you need to put this serial port into your favorite Serial Terminal. The buad rate is: 115200.

# Full list of supported statements and functions
## System

BYE - exits Basic, soft reboot on Arduino                                                                                 
END - stops execution from the program, also "STOP"                                                                       
MEM - displays memory usage statistics                                                                                  
NEW - clears the current program                                                                                          
RUN - executes the current program 

## File IO/SD Card                                                                                                          
FILES - lists the files on the SD card

LOAD filename.bas - loads a file from the SD card

CHAIN filename.bas - equivalent of: new, load filename.bas, run

SAVE filename.bas - saves the current program to the SD card, overwriting

## EEProm - nonvolatile on-chip storage

EFORMAT - clears the EEProm memory

ELOAD - load the program in from EEProm

ESAVE - save the current program to the EEProm

ELIST - print out the contents of EEProm

ECHAIN - load the program from EEProm and run it

## IO, Documentation

PEEK( address ) - set a value in memory (unimplemented)

POKE - get a value in memory (unimplemented)

PRINT expression - print out the expression, also "?"

REM stuff - remark/comment, also "'"

## Expressions, Math

A=V, LET A=V - assign value to a variable

+, -, *, / - Math

<,<=,=,<>,!=,>=,> - Comparisons

ABS( expression ) - returns the absolute value of the expression

RSEED( v ) - sets the random seed to v

RND( m ) - returns a random number from 0 to m

## Control

IF expression statement - perform statement if expression is true

FOR variable = start TO end - start for block

FOR variable = start TO end STEP value - start for block with step

NEXT - end of for block

GOTO linenumber - continue execution at this line number

GOSUB linenumber - call a subroutine at this line number

RETURN - return from a subroutine

## Pin IO

DELAY timems- wait (in milliseconds)

DWRITE pin,value - set pin with a value (HIGH,HI,LOW,LO)

AWRITE pin,value - set pin with analog value (pwm) 0..255

DREAD( pin ) - get the value of the pin

AREAD( analogPin ) - get the value of the analog pin

## Sound - Piezo wired up to A14(DAC) and ground

TONE freq,timems - play "freq" for "timems" milleseconds (1000 = 1 second)

TONEW freq,timems - same as above, but also waits for it to finish

NOTONE - stop playback of all playing tones

NOTE: TONE commands are by default disabled

## Example programs

Here are a few example programs to get you started...

### Blink

Hook up an LED between pin 13 and ground

10 FOR A=0 TO 10

20 DWRITE 13, HIGH

30 DELAY 250

40 DWRITE 13, LOW

50 DELAY 250

60 NEXT A

### Fade

Hook up an LED between pin 23 and ground

10 FOR A=0 TO 10

20 FOR B=0 TO 255

30 AWRITE 23, B

40 DELAY 10

50 NEXT B

60 FOR B=255 TO 0 STEP -1

70 AWRITE 23, B

80 DELAY 10

90 NEXT B

100 NEXT A

### LED KNOB

Hook up a potentiometeter between analog 0(Digital pin 14) and ground, led from digital 23 and ground. If knob is at 0, it stops

10 A = AREAD( 14 )

20 PRINT A

30 B = A / 4

40 AWRITE 23, B

50 IF A == 0 GOTO 100

60 GOTO 10

100 PRINT "Done."

### AREAD example
Hook up a wire to pin 14(A0) and plug it into various pins such as pin 13(LED pin) and see the voltage change.

10 A = AREAD( 14 )

20 PRINT A

30 GOTO 10

NOTE: To exit an infinity program you need to press CTRL+C. That will end the program.

### ECHAIN example

Write a small program, store it in EEPROM. We'll show that variables don't get erased when chaining happens

EFORMAT

10 A = A + 2

20 PRINT A

30 PRINT "From eeprom!"

40 IF A = 12 GOTO 100

50 PRINT "Shouldn't be here."

60 END

100 PRINT "Hi!"

Then store it in EEProm

ESAVE

Now, create a new program in main memory and run it

NEW

10 A = 10

20 PRINT A

30 PRINT "From RAM!"

40 ECHAIN

List both, and run

ELIST

LIST

RUN
