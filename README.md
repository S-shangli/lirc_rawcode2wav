# lirc_rawcode2wav
This script convert to wav file from the rawcode of mode2 command(lirc).
It made for CHIP. but it can work any env. maybe.

# settings
|item|descriptions|
|---|---|
|RATE|output file sampling rate[Hz]|
|BIT|output file bit depth|
|SOX_CMD|sox command path|


# usage - make wav file
execute with two arg that is input_file_name and output_file_name

    [yourmachine]% ./rawcode2wav.sh ./inputfile.txt ./outputfile.wav

# usage - IR transmit
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

# usage example
    # install tools
    [yourmachine]% sudo apt-get install lirc bc sox
    
    # get RAW_CODES
    [yourmachine]% sudo mode2 -H audio_alsa -d default | tee inputfile.txt
    space 765
    pulse 127
    space 214
    pulse 80
    space 1208273
    pulse 2624
    space 2658
    <snip>
    pulse 852
    space 1895
    pulse 831
    space 809
    pulse 855
    ^C                <--- Ctrl-C to exit
    
    # convert to wav
    [yourmachine]% ./rawcode2wav.sh ./inputfile.txt outputfile.wav
    /usr/bin/sox WARN sox: `outputfile.wav' output clipped 46 samples; decrease volume?
    
    # IR transmit
    [yourmachine]% aplay outputfile.wav
    Playing WAVE 'outputfile.wav' : Signed 24 bit Little Endian in 3bytes, Rate 192000 Hz, Stereo

sox said anything, it's ok. It's using a sine wave with max amplitude.

# recommendation
* Edit a RAW_CODES to only a sequence of IR code, before convert.
it's appear between leader-code and long space.
* Edit a RAW_CODES to acutual format of NEC-format, AEHA-format or SONY-format.

