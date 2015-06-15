This stuff is in progress.
https://github.com/JChristensen/mighty-1284p/tree/v1.6.3

bugs/issues noticed:
==========
PORT_NDX_TO_PCMSK was casting 0 as uint8_t instead of a pointer
this caused digitalPinToPCMSK to use pointer cast.
The pointer should also be a volatile but when 0 is returned it doesn't much
matter.
But there is code that uses the return value without checking it.
If so, then PINA gets stomped on if the pin value is invalid.

==========
pcint/pcmsk macros were all hosed up in SB variant file.
digitalPintoPCMSK()
wasn't indexing and returned pointer to table rather than pointer to register.
Corrected them and converted them over to the new inline ternary as well.

==========
PORT_TO_XXX macros should be using PA,PB,PB,PD defines rather than 0-3
(fixed in SB variant)

==========
SoftwareSerial does not validate the pin numbers.

==========
1.6.3 IDE leaves turds down in /tmp
It creates multiple .tmp directories but it doesn't remove the directory for
the untitled sketch that it opens when you start it up.

==========
Most Team Arduino Variants have digitalPinToInterrupt()
The "standard"/328, leonardo, variants
converts from a digital pin # to a INTx number.
The ethernet variants does not have this macro
The mega variant looks like it handles INT0, and INT1 but then has a
conversion for INT5,INT4,INT3,INT2 that only work on the 1281/2564 vs 1280/2580
#define digitalPinToInterrupt(p) ((p) == 2 ? 0 : ((p) == 3 ? 1 : ((p) >= 18 && (p) <= 21 ? 23 - (p) : NOT_AN_INTERRUPT)))
This would return, 0, 1, 5, 4, 3, 2

The 1284 has INT0, INT1, and INT2
(updated for all variants)

=======
The mega variant does not map all the possible PCINT pins.
They assumed that the only function of this was for SoftwareSerial, but it
could be used for all kinds of other stuff.

=======
avr_develoers picture/diagram is wrong.
INT0 and INT1 are not on dip physical pins 1 and 2 but on pins 16 and 17
(fixed)
