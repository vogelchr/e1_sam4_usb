** Review comments for version

commit af33165416c78bff5535539db3183336a3adedb6
    Changed LIU to IDT82V2081.

OPEN:

* Repository / File Formats
	- rename .brd/.sch to sensible name, integrate in old
	   http://cgit.osmocom.org/osmo-e1-xcvr/ repository.
* Board
	- LEDs near frontpanel
	- USB_VBUS_SENS not routed to microcontroller!
	- LIU move signal from LOS to #INT 
	  -> Interrupt can be generated on LOS anyway.
	- add pullup on LIU #RST

	(from LaF0rge:)
	 - UART has not transient voltage suppressor, possible ESD damage
	(recommended: IP4234CZ6)

	  => ok

	 - UART has no pull-up resistor on RxD input, i.e. floating input unless
	UART is connected

	  => ok, but I think has internal pullup in SAM4S, at least can be turned on

	 - GPS antenna input has no transient voltage suppressor, possible ESD
	damage (unless the
	GNS module has one inside? Did you find any documentation on that in
	the module data sheet? Recommended: 0603ESDA-TR

	  => will check

	 - !RESET has no pull-up, design is possibly susceptible to picking up
	RF/noise as reset trigger

	  => has internal pullup, but will be easy to add

	 - 1k series resistors for LEDs seems quite high resistance for 3.3V. I
	would usually expect 330..680 Ohms there?

	  => with modern LEDs it's really bright enough, even with 1k (or even 5k)

	 - JTAG connector doesn't appear to have standard 2x10 ARM-JTAG pin-out.
	If there's space I'd prefer a 20pin ARM-JTAG.

	  => it's compatible to the "standard" mini pinout (but on 0.1" grid),
	     20pin will be a little tight where it's now.

	 - GPS receiver rx/tx/pps doen't have series termination (27R or
	the like), possibly resulting in switching tranients degrading GPS
	performance

	  => not needed in my oppinion

	 - does SPI+I2C header (SV6) have UEXT compatible pin-out? If we use a
	 2x5 header, it might make senese to use the olimex-standard UEXT format
	 as there are plenty of other boards/devices with that pin-out

	  => makes sense

	 - SV3 serial pin-out could be aligned with FTDI 1x5 (or 1x6?) usb
	serial cables. I think it makes sense to align with pin-out of industry
	standard cables that people might already have

	  => makes sense

	 - for the DAC, it might make sense to align with what sylvain is using
	in his FPGA based design (and what sysmocom has used in other designs):
	MAX5215 (14bit). Could be upgraded to 16bit MAX5217 if ever needed.

	  => ok (vogelchr)

	 - might make sense to add a precision voltage reference for the DAC,
	e.g. MAX6163ESA?

	 - Cosmetic

	 - If inverted-logic signals would use !SIGNAL instead of #SIGNAL, EAGLE
	would over-stroke their names

	  => (vogelchr) Nice, didn't know this, but will probably not change.

	 - diescrete resistors instead of U$2 resistor array is probably lower
	cost in SMT, as it's a standard 10k resistor reel and not one more
	"cut-tape" reel only for that part

	   => (vogelchr) I think I'll add 10k quad resistor arrays for all
	                 the random pullup/pulldowns, will cut down on
			 individual resistors, and add only one component type
			 to the BOM


DONE after 20180624:
	- EAGLE 7 compatibility
		(exported _v7 files)
	- make JTAGSEL pullup DNP (JTAGSEL, TEST, ERASE should float)
	- ERASE jumper
	- pullups on i2c
	- pullups on SPI \CSn

	- possibly switch to 82V2081 LUI, as a few people have successfully
	  experimented with it, has access to nice stats over SPI
	  https://www.idt.com/products/interface-connectivity/telecom-interface-products/t1j1e1-interface-products/82v2081-1-channel-t1-j1-e1-short-haullong-haul-liu

DONE after 20180625:
	 - USB has no transient voltage suppressor, possible ESD damage
	 (recommended: IP4234CZ6)
	- R1 120 Ohms (LIU Rx term)
	- R2, R8 9 Ohms (LIU Tx in series)

REFUSED:
	- interface to old LIU only board (SPI + DATA on pinheader)
