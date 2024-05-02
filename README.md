Mods for manager firmware V3.10. They will probably break compatibility with the FoxESS cloud. The function/variable names refer to the included Ghidra database.

Change "energy total" field unit to 10 Wh (the actual resolution is 20 Wh) from the default 100 Wh:
```
in "update_live_data_for_wifi_tx":
	    0802b9ce d1 f8 44 16     ldr.w      r1,[r1,#0x644]=>commEnergyTotalHundredthskWh
	    0802b9d2 0a 22           movs       r2,#0xa
	(-) 0802b9d4 b1 fb f2 f1     udiv       r1,r1,r2
	    0802b9d8 50 4a           ldr        r2,[PTR_DAT_0802bb1c]
	    0802b9da 91 60           str        r1,[r2,#0x8]=>energyTotalTenthskWh
	change to:
	    0802b9ce d1 f8 44 16     ldr.w      r1,[r1,#0x644]=>commEnergyTotalHundredthskWh
	    0802b9d2 0a 22           movs       r2,#0xa
	(+) 0802b9d4 af f3 00 80     nop.w
	    0802b9d8 50 4a           ldr        r2,[PTR_DAT_0802bb1c]
	    0802b9da 91 60           str        r1,[r2,#0x8]=>energyTotalTenthskWh
```

Always send energy info (no heartbeat or firmware versions packet):
```
	in "wifi_tx_send_one":
	    080348ee 00 88           ldrh       r0,[r0,#0x0]=>wifiTxNextPacketType
	(-) 080348f0 20 b1           cbz        r0,LAB_080348fc
	    080348f2 01 28           cmp        r0,#0x1
	(-) 080348f4 41 f0 34 84     bne.w      LAB_08036160
	    080348f8 01 f0 7a b9     b.w        LAB_08035bf0
	change to:
	    080348ee 00 88           ldrh       r0,[r0,#0x0]=>wifiTxNextPacketType
	(+) 080348f0 00 bf           nop
	    080348f2 01 28           cmp        r0,#0x1
	(+) 080348f4 af f3 00 80     nop.w
	    080348f8 01 f0 7a b9     b.w        LAB_08035bf0
```

Change packet sending interval from the default 30 seconds: maybe around 0x08027bbc, timer variable at 0x2000d20c? untested
