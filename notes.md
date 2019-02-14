# Notes

## Datasheet links:
  - [Supermicro power supply dimensions](
    https://store.supermicro.com/media/wysiwyg/productspecs/PWS-1K66P-1R/PWS-1k66P-1R_quick_spec.pdf)
  - [Compatible? PCB edge connector: Amphenol 10035388-102LF](
    https://www.mouser.com/datasheet/2/18/10035388-1360968.pdf)
  - [4 pin disk drive power connector (TE 641737-1)](
    https://www.mouser.com/datasheet/2/418/NG_CS_82181_SOFTSHELL_STANDARD_DENSITY_0508-1262029.pdf)
  - [4 pin fan connector (Molex 47054-1000)](
    https://www.mouser.com/datasheet/2/276/0470531000_PCB_HEADERS-146351.pdf)
  - [PWM fan specification](
    http://handsontec.com/pdf_files/4_Wire_PWM_Spec.pdf)

## connector layout
Looking from front of case, 7+7 signal pins (top + bottom), then 9+9 pins
of +12V (top + bottom), and finally 9+9 pins of ground (top + bottom)

## Fan connector:
There is what appears to be an 8 pin JST connector on a vertically mounted
daughter board near the left edge of the card edge connector with a cable
that connects to the twin fans. Disconnecting the cable causes the power
supply to error on startup. Probing it with a multimeter shows the
following: (numbering from the left/bottom pin of the fan connector on the pcb)

1. ~200 Hz square wave (tachometer? 2 counts per cycle?)
2. ~150 Hz square wave (tachometer? 2 counts per cycle?)
3. +3 Vpp square wave ~30% duty cycle 50 kHz (PWM control?)
4. +3 Vpp square wave ~30% duty cycle 50 kHz (PWM control?)
5. 0V (ground?)
6. 0V (ground?)
7. +11.75V (power vs. power sense?)
8. +12V (power?)

Hypothesis: this is likely to be a variation of the standard 4 pin fan
connector, in which case it should be possible to simulate a fan by connecting
two NPN transistors with the collectors connected to pins 1 and 2, the emitters
connected to ground/pins 5 and 6, and the gate connected to a ~200 Hz square
wave. I would still need to provide sufficient airflow to keep the power supply
cool at full load, of course (or, if that proves to be too noisy, use the PWM
outputs from pin 3 & 4 to be able to ramp up the fan speed when needed).
