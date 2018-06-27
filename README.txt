** Review comments for version

commit af33165416c78bff5535539db3183336a3adedb6
    Changed LIU to IDT82V2081.

OPEN:

* Repository / File Formats
	- rename .brd/.sch to sensible name, integrate in old
	   http://cgit.osmocom.org/osmo-e1-xcvr/ repository.
* Board
	- LEDs near frontpanel
	- R1 120 Ohms (LIU Rx term)
	- R2, R8 9 Ohms (LIU Tx in series)
	- USB_VBUS_SENS not routed to microcontroller!
	- LIU move signal from LOS to #INT 
	  -> Interrupt can be generated on LOS anyway.
	- add pullup on LIU #RST

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

REFUSED:
	- interface to old LIU only board (SPI + DATA on pinheader)
