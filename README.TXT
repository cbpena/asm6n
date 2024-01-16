--------------------------------------------------------------
ASM6N (v1.7.1)
A 6502 assembler by loopy (loopy at mm.st)
With modifications by never-obsolete, freem, nicklausw, and Sour
--------------------------------------------------------------

ASM6N is a fork of ASM6 1.6, with some features of ASM6F.

This goes over the changes that have been made. See the
original README for further details on usage.

--------------------------------------------------------------
Additions
--------------------------------------------------------------
	* Changed BIN directive to work like HEX, but for binary
	* Added ! operator to force ABS/X/Y instead of ZP/X/Y
	* Added RAM,ENDRAM, WRAM,ENDWRAM, SRAM,ENDSRAM directives
	* Added RS alias for DSB directive
	* Added WORDB, DWB, DCWB, DC.WB directives (define word, big-endian)
	* Added BANK directive and ? operator to retrieve a label's bank number
	* Added PRINT directive
	* Added support for exporting Mesen (.mlb) label files [based on asm6f code]
	* Added support for exporting all RAM lables to a lua file
	* Added support for stable illegal opcodes [based on asm6f code]

--------------------------------------------------------------
Command line
--------------------------------------------------------------

Usage:

        asm6 [-options] sourcefile [outputfile] [listfile]

Options:

        -?         Show some help
        -l         Create listing
        -L         Create verbose listing (expand REPT, MACRO)
        -d<name>:  Define a symbol and make it equal to 1
	-q         Quiet mode (suppress all output unless there's an error)
	-m         generate *.mlb file for use with Mesen
	-M         generate *.mlb file for use with Mesen, ingorning local labels
	-r         generate *.lua file with all ram/wram/sram labels
        Default output is <sourcefile>.bin
        Default listing is <sourcefile>.lst

--------------------------------------------------------------
Numbers and expressions
--------------------------------------------------------------

The ! operator can be used to force absolute addressing.

	lda !Label
	lda !Label, X
	lda !Label, Y

--------------------------------------------------------------
Assembler directives
--------------------------------------------------------------

BIN (use has been changed, INCBIN remains unchanged)

	Compact way of laying out a table of binary values.  Only raw binary values
	are allowed, no expressions.  Spaces can be used to separate numbers.	
		
		BIN 1111000000001111		; equivalent to DB %11110000, %00001111



RAM / ENDRAM, WRAM / ENDWRAM, SRAM / ENDSRAM

	Same function and use as ENUM / ENDE. Labels defined in these blocks
	will be output with the -r option, ENUM will not.



RS

	Alias for DSB directive.



DWB (also WORDB, DCWB, DC.WB)

	Emit word(s) in big-endian format.
		
		DWB $23C0			; equivalent to DB $23, $C0



BANK

	Set the global bank number. Each label defined after it will be assigned
	this value as its bank number. Can be retrieved using the ? operator.
		
		BANK 0
		Label_0:
		.
		.
		.
		BANK 4
		Label_1:
		.
		.
		BANK 7
		Label_2:	lda #?Label_0			; equivalent to lda #0
				lda #?Label_1			; equivalent to lda #4
				lda #?Label_2			; equivalent to lda #7



PRINT
	Display a message without stopping assembly.
		
		PRINT "This is a message!"



