** Review comments for version

	commit e3206db8d85a40d4a1522e97d409fd64b1716cde
	Author: Christian Vogel <vogelchr@vogel.cx>
	Date:   Sun Jun 24 21:14:47 2018 +0200

* Repository / File Formats
	- EAGLE 7 compatibility
	- rename .brd/.sch to sensible name, integrate in old
	   http://cgit.osmocom.org/osmo-e1-xcvr/ repository.

* Board
	- ERASE jumper
	- make JTAGSEL pullup DNP (JTAGSEL, TEST, ERASE should float)
	- LEDs near frontpanel
	- interface to old LIU only board (SPI + DATA on pinheader)
	- pullups on i2c
	- possibly switch to 82V2081 LUI, as a few people have successfully
	  experimented with it, has access to nice stats over SPI
	  https://www.idt.com/products/interface-connectivity/telecom-interface-products/t1j1e1-interface-products/82v2081-1-channel-t1-j1-e1-short-haullong-haul-liu


