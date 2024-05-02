FAT32 formatted flash drive

The first attempt may fail with "Copy Error 01", in this case just replug the flash drive and try again.

The extension must be exactly `hex` or `bin`, lower case. The bin files are NOT straight flash images, neither are they flash images but except the bootloader (flash firmware images). The bin files consist of checksummed 21-byte blocks.

The accepted hex files are the same ones you'd get from the FoxESS Cloud service, they are normal Intel hex files of the flash firmware starting at 0x08005000. To convert a binary manager firmware image to such a hex:

```
objcopy -I binary -O ihex --change-addresses 0x08005000 firmware.bin firmware.hex
```

Create the directory and file structure corresponding to the target chip:

The characters marked VV.VV represent the version number in the filename, it is checked against the currently installed one and installing a lower one is not allowed. It can also be set to "VA.AA" to always allow installing the firmware.

The characters marked TT represent the inverter type, must be T2 for non-G3 T-series.

The character marked F represents the firmware type, M for master, S for slave and also M for manager.

Unmarked characters that are not part of the extension are not used for anything.

```
/Update/Master
	                TT   F      VV VV
	10-300-11017-21_T20K_Master_V3.48.bin

	TT   F      VV VV
	T20K_Master_V3.27.bin

/Update/Slave
	                TT   F     VV VV
	10-300-11018-01_T20K_Slave_V3.01.hex

	TT   F     VV VV
	T20K_Slave_V3.01.hex

/Update/Manager
	                TT   F       VV VV
	10-300-11039-01_T20K_Manager_V3.12.hex

	TT   F       VV VV
	T20K_Manager_V3.10.hex
```

The manager bootloader checks for the `AA 55 AA 55 AA 55 12 34 56 78` magic to be present at 0x0803f000, if not it will try to load the image from the SPI flash into the GD32's internal flash. When a manager upgrade is triggered the manager copies the update package into the SPI flash, erases the magic in the MCU flash and resets into the bootloader. The hex/foxbin parsing is done after the reset, if you put a bad file in say hello to a bootloop.
