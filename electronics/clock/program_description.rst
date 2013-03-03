Program Description
====================

Here are some notes on the clock firmware:

Timer begins at 0xc4 and interrupts at overflow. TMR0 and Global Interrupts are set. Timer0 is increment on leading edge of TOCKI. The WDT is cleared at the end of the main loop.

0x0 -> TRISC All bits of Port C are used as output.
0x0 -> TRISB Port B is currently Unused.
b'10000' -> TRISA The TMR0 bit must be an input. Otherwise PORTA is set to output and currently unused.

Output:
In each case, bit 7 is the hour/not minute flag and bit 6 is the binary/not 7-seg flag. With binary, the remaining bits are the binary representation of the time, either minutes or hours. With the 7-seg, bit 5 is unused and the low nibble is the hex address of the segment to be turned on. I use dual 7-seg displays, the 8-segs(including decimal) are addresses 0-7 and the left are 8-15.



Reserved memory locations
---------------------------

#. endOfNamedSpots (20h) This is where I keep track of the last reserved memory spot. Just in case it comes in handy some day. INDF use maybe?

#. output (21h) This is the value to be sent to the the output port

#. counter (22h) This is for counting the number of times I move through the main loop.

#. tmp (23h) Temporary memory slot

#. myStatus (24h) My Status byte (see above)

#. saveW (25h) I use this to save the W register value when I have an interrupt

#. minutes (26h) Minute memory location

#. hours (27h) Hour memory location

#. currSeg (28h) I use this when I iterate through the 7-seg segments

#. valueToLatch (29h) This contains the current value sent to the Hex-16 latch

#. totalMinutesLOW (30h) Total minutes since start of timer. why not? might come in handy later

#. totalMinutesMIDDLE (31h) Middle of a 3byte counter of total minutes

#. totalMinutesHIGH (32h) End of total minutes segment. increment LOW, if carryover increment middle, if carryover increment high. if carryover... whoa!

#. setTimeTmp (33h) Not yet implemented to be used for setting time

My Status Register
--------------------

* Bit 0: Hours/Not Minute Displayed

* Bit 1: Binary/Not 7-seg Displayed

* Bit 2: Right/Not Left 7-Seg Displayed







