** Review comments for version

commit af33165416c78bff5535539db3183336a3adedb6
    Changed LIU to IDT82V2081.

OPEN:

* Repository / File Formats
	- rename .brd/.sch to sensible name, integrate in old
	   http://cgit.osmocom.org/osmo-e1-xcvr/ repository.
* Board

	- USB_VBUS_SENS not routed to microcontroller!
	 - GPS antenna input has no transient voltage suppressor, possible ESD
	damage (unless the
	GNS module has one inside? Did you find any documentation on that in
	the module data sheet? Recommended: 0603ESDA-TR
	  => will check




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
	- LIU move signal from LOS to #INT 
	  -> Interrupt can be generated on LOS anyway.
	 - diescrete resistors instead of U$2 resistor array is probably lower
	cost in SMT, as it's a standard 10k resistor reel and not one more
	"cut-tape" reel only for that part
		-> introduced quad resistor packs 10k for various pullups
	 - for the DAC, it might make sense to align with what sylvain is using
	in his FPGA based design (and what sysmocom has used in other designs):
	MAX5215 (14bit). Could be upgraded to 16bit MAX5217 if ever needed.
	 - UART has not transient voltage suppressor, possible ESD damage
	(recommended: IP4234CZ6)
	 - JTAG connector doesn't appear to have standard 2x10 ARM-JTAG pin-out.
	If there's space I'd prefer a 20pin ARM-JTAG.
	 - SV3 serial pin-out could be aligned with FTDI 1x5 (or 1x6?) usb
	serial cables. I think it makes sense to align with pin-out of industry
	standard cables that people might already have
	- LEDs near frontpanel
	 - does SPI+I2C header (SV6) have UEXT compatible pin-out? If we use a
	 2x5 header, it might make senese to use the olimex-standard UEXT format
	 as there are plenty of other boards/devices with that pin-out
	 - !RESET has no pull-up, design is possibly susceptible to picking up
	RF/noise as reset trigger
	 - UART has no pull-up resistor on RxD input, i.e. floating input unless
	UART is connected
	- add pullup on LIU #RST

REFUSED:
	- interface to old LIU only board (SPI + DATA on pinheader)
	 - might make sense to add a precision voltage reference for the DAC,
	e.g. MAX6163ESA?
		regulation seems to be good enough with the crappy DAC
		in the SAM4S, so I think having the "good" +UB supply that
		has constant load will be sufficient

	 - GPS receiver rx/tx/pps doen't have series termination (27R or
	the like), possibly resulting in switching tranients degrading GPS
	performance
	  => not needed in my oppinion
	 - 1k series resistors for LEDs seems quite high resistance for 3.3V. I
	would usually expect 330..680 Ohms there?
	  => with modern LEDs it's really bright enough, even with 1k (or even 5k)

	 - If inverted-logic signals would use !SIGNAL instead of #SIGNAL, EAGLE
	would over-stroke their names
	  => (vogelchr) Nice, didn't know this, but will probably not change.
