# lirc_rawcode2wav
This script convert to wav file from the rawcode of mode2 command(lirc).
It made for CHIP. but it can work any env. maybe.

# settings
|item|descriptions|
|---|---|
|RATE|output file sampling rate[Hz]|
|BIT|output file bit depth|
|SOX_CMD|sox command path|


# usage
execute with two arg that is input_file_name and output_file_name

    [yourmachine]% ./rawcode2wav.sh ./inputfile.txt ./outputfile.wav

## usage IR transmit
play a wav file.

    [yourmachine]% aplay ./outputfile.wav
    Playing WAVE 'outputfile.wav' : Signed 24 bit Little Endian in 3bytes, Rate 192000 Hz, Stereo

# software requirements
* sox
* bash
* egrep / fgrep / sed
* bc

Note: this script generate a very long command line. it is restricted by ARG_MAX.

    [yourmachine]% getconf ARG_MAX
    2097152

# hardware requirement
* C.H.I.P. or any env?
* IR-LEDs (2 LEDs required)

# hardware connection
       CHIP
    +--------+
    |        |
    |     HP L----------------------------------+
    |        |                                  |
    |     HP R-----||------*---[ IR LED |>| ]---*
    |        |      C1     |                    |
    +--------+             +---[ |<| IR LED ]---+
     
    + : corner/cross-over
    * : connect
    HP L/R : audio output Left/Right
    C1 : 1uF or less

## hardware connection for mode2 (optional)
            CHIP          IR receiver
         +--------+       +---------+
         |        |       |         |
         |   VCC-5V-------VCC  OUTPUT---+
         |        |       |         |   |
         |      GND---*---GROUND    |   |
    +----MIC IN   |   |   |         |   |
    |    +--------+   |   +---------+   |
    |                 |                [R1]
    |                 |                 |
    |                 |                 *----+
    |                 |                 |    |
    |                 |                [R2]  |
    |                 |                 |    |
    |                 +-----------------+    |
    |                                        |
    +----------------------------------[C1]--+
     
    + : corner/cross-over
    * : connect
    R1 : 100k Ohm
    R2 : 560 Ohm
    C1 : 1 uF or less

# reffer
https://bbs.nextthing.co/t/installing-lirc-on-c-h-i-p/2449/5
