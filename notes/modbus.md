# Inverter modbus holding register map (from cloud client)

## Notes
*\* 10* in Remarks means the value is multiplied by 10, so a register value of 123 means 12.3

The remote server always reads a full block with *0x03 Read Holding Registers* and writes back a full block with *0x10 Preset Multiple Registers*, even if only one value was changed. This is indeed the only kind of access supported by the inverter's firmware - trying to read part of a block will fail.

Except when the text refers to a single register, then PresetSingleRegister (6) should be used to set it. But still ReadHoldingRegisters (3) to read.

## ConfigurationInfo (20 registers at 0xC418):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC418|Language|u16|0=English, 1=German, 2=Polish|
|0xC419|safety country|u16|0=AS4777_AU, 1=AS4777_NZ, 2=G98_1, 3=G99_1, 4=Netherlands, 5=CEI_021, 6=VDE0126, 7=ARN4105, 8=Brasil_220, 9=Brasil_240, 10=IEC61727, 11=Philippines, 12=NRS097_2_1, 13=Vietnam, 14=Poland, 15=Portugal, 16=PPDS, 17=UNE_206, 18=RD1699, 19=Belgium, 20=VFR2014, 21=UTE_C15_712_1, 22=Singapore, 23=Indonesia, 24=Malaysia, 25=Cambodia, 26=PEA, 27=MEA, 28=SriLanka, 29=Pakistan, 30=Ireland, 31=Denmark, 32=Slovakia, 33=Austria, 34=Switzerland, 35=Slovenia, 36=Hungary, 37=Serbia, 38=Croatia, 39=Turkey, 40=Cyprus, 41=Bulgaria, 42=Romania, 43=Greece, 44=Latvia, 45=Lithuania, 46=Estonia, 47=Sweden, 48=Norway, 49=Finland, 50=Argentina, 51=Chile, 51=Japan, 52=Mexico, 53=USA, 54=Canada, 55=China, 57=China-1, 58=Local, 59=SaudiArabia|
|0xC41A|ModbusSlaveId|u16|readonly in FoxCloud UI|
|0xC41B|Factorymode|u16|0=Invalid, 1=NotFactoryMode, 2=FactoryMode|
|0xC41C|PVConfig|u16|0=Invalid, 1=Single, 2=InParallel|
|0xC41D|DRMEnable|bit||
|0xC41E|MuteEnable|bit||
|0xC41F|StartSelftest|bit||
|0xC420|SaftyModeLock|bit||
|0xC421|DIONOFF|bit||
|?|EnableEps|?|?
|0xC424|OperationMode|bit||
|0xC42B|TBD|s16||


## StartParameters (12 registers at 0xC670):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC670|ConnectTime|u16||
|0xC671|LoadSpeed|u16|\* 10|
|0xC672|ReconnectTime|u16||
|0xC673|ReloadSpeed|u16|\* 10|
|0xC674|VgridBackHigh|u16|\* 10|
|0xC675|VgridBackLow|u16|\* 10|
|0xC676|FgridBackHigh|u16|\* 100|
|0xC677|FgridBackLow|u16|\* 100|
|0xC678|GridVoltageReconnectHighThreshold|u16|\* 10|
|0xC679|GridVoltageReconnectLowThreshold|u16|\* 10|
|0xC67A|GridFrequencyReconnectHighThreshold?|u16|\* 100|
|0xC67B|GridFrequencyReconnectLowThreshold|u16|\* 100|

?: listed as GirdVoltageReconnectLowThreshold

## GridVoltageParameters (14 registers at 0xC6D4):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC6D4|(bitfield)|u16|(OVP1Enable << 0) \| (OVP2Enable << 1) \| (OVP3Enable << 2) \| (UVP1Enable << 3) \| (UVP2Enable << 4) \| (UVP3Enable << 5) \| (OVPMovEnable << 6)|
|0xC6D5|Vmax1Limit|u16|\* 10|
|0xC6D6|Vmax1ProtectTime|u16|\* 100|
|0xC6D7|Vmax2Limit|u16|\* 10|
|0xC6D8|Vmax2ProtectTime|u16|\* 100|
|0xC6D9|Vmax3Limit|u16|\* 10|
|0xC6DA|Vmax3ProtectTime|u16|\* 100|
|0xC6DB|Vmin1Limit|u16|\* 10|
|0xC6DC|Vmin1ProtectTime|u16|\* 100|
|0xC6DD|Vmin2Limit|u16|\* 10|
|0xC6DE|Vmin2ProtectTime|u16|\* 100|
|0xC6DF|Vmin3Limit|u16|\* 10|
|0xC6E0|Vmin3ProtectTime|u16|\* 100|
|0xC6E1|Vgrid10minPro|u16|\* 10|


## GridFreqParameters (14 registers at 0xC79C):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC79C|(bitfield)|u16|(OFP1Enable << 0) \| (OFP2Enable << 1) \| (UFP1Enable? << 2) \| (UFP2Enable? << 3) \| (ROCOF << 4) \| (Italy宽窄频 << 5)|
|0xC79D|Fmax1Limit|u16|\* 100|
|0xC79E|Fmax1ProtectTime|u16|\* 100|
|0xC79F|Fmax2Limit|u16|\* 100|
|0xC7A0|Fmax2ProtectTime|u16|\* 100|
|0xC7A1|Fmin1Limit|u16|\* 100|
|0xC7A2|Fmin1ProtectTime|u16|\* 100|
|0xC7A3|Fmin2Limit|u16|\* 100|
|0xC7A4|Fmin2ProtectTime|u16|\* 100|
|0xC7A5|ROCOF|u16|\* 10|
|0xC7A6|GridFrequencyProtectLevel3Highthreshold|u16|\* 100|
|0xC7A7|GridFrequencyProtectLevel3HighTripTime|u16|\* 100|
|0xC7A8|GridFrequencyLevel3LowThreshold|u16|\* 100|
|0xC7A9|GridFrequencyLevel3LowTripTime|u16|\* 100|

?: UFP guessed due to incorrect labels

## PowerFreqParameters (8 registers at 0xC864):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC864|(bitfield)|u16|(FreDeratingEnable << 0) \| (PowerFollowFreq << 1)|
|0xC865|FreqPoint|u16|\* 100|
|0xC866|FreqSpeed|u16|\* 10|
|0xC867|FbackUp|u16|\* 100|
|0xC868|FbackDown|u16|\* 100|
|0xC869|WaitTimeBack|u16||
|0xC86A|FreqbackSpeed|u16|\* 10|
|0xC86B|WaittimeOverFreq|u16|\* 100|

## ReactiveConfig (27 registers at 0xC92C):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC92C|(bitfield)|u16|(FixedCosphiAhead << 1) \| (FixedCosphiHysteresis << 2) \| (PFLineMode << 3) \| (FixedQvar << 4) \| (Qu mode << 5) \| (VoltageLockEnable << 6) \| (PowerLockEnable << 7)|
|0xC92D|PFmode|u16|1=FixedCosphiAhead, 2=FixedCosphiHysteresis, 3=PFLineMode, 4=FixedQvar, 5=Qu mode)|
|0xC92E|Pfcosphi|u16|\* 10|
|0xC92F|PFQvar|s16|\* 10|
|0xC930|Pfcosphi1|u16|\* 100|
|0xC931|Pfpowerpoint1|u16||
|0xC932|Pfcosphi2|u16|\* 100|
|0xC933|Pfpowerpoint2|u16||
|0xC934|Pfcosphi3|u16|\* 100|
|0xC935|Pfpowerpoint3|u16||
|0xC936|Pfcosphi4|u16|\* 100|
|0xC937|Pfpowerpoint4|u16||
|0xC938|PflockinV|u16|\* 10|
|0xC939|PflockoutV|u16|\* 10|
|0xC93A|VU1|u16|\* 10|
|0xC93B|QU1|s16|\* 10|
|0xC93C|VU2|u16|\* 10|
|0xC93D|QU2|s16|\* 10|
|0xC93E|VU3|u16|\* 10|
|0xC93F|QU3|s16|\* 10|
|0xC940|VU4|u16|\* 10|
|0xC941|QU4|s16|\* 10|
|0xC942|QulockinP|u16||
|0xC943|QulockoutP|u16||
|0xC944|Qmax|u16|\* 100|
|0xC945|Waittime|u16|\* 100|
|0xC946|FixedQTimeConstant|u16|\* 100|


## FaultRideThrough (21 registers at 0xC990):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC990|(bitfield)|u16|(Low/UnderVoltageRideThrough << 0) \| (Low/UnderVoltageRideThrough << 1)|
|0xC991|LT1|u16|\* 100|
|0xC992|LV1|u16|\* 10|
|0xC993|LT2|u16|\* 100|
|0xC994|LV2|u16|\* 10|
|0xC995|LT3|u16|\* 100|
|0xC996|LV3|u16|\* 10|
|0xC997|LT4|u16|\* 100|
|0xC998|LV4|u16|\* 10|
|0xC999|LT5|u16|\* 100|
|0xC99A|LV5|u16|\* 10|
|0xC99B|HT1|u16|\* 100|
|0xC99C|HV1|u16|\* 10|
|0xC99D|HT2|u16|\* 100|
|0xC99E|HV2|u16|\* 10|
|0xC99F|HT3|u16|\* 100|
|0xC9A0|HV3|u16|\* 10|
|0xC9A1|HT4|u16|\* 100|
|0xC9A2|HV4|u16|\* 10|
|0xC9A3|HT5|u16|\* 100|
|0xC9A4|HV5|u16|\* 10|

## DCIConfig (6 registers at 0xC9F4):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xC9F4|(bitfield)|u16|(DCI1Enable << 0) \| (DCI2Enable << 1) \| (DCITestEnable << 2)|
|0xC9F5|Dcilimit1|u16|\* 1000|
|0xC9F6|Dcilimit1time|u16|\* 100|
|0xC9F7|Dcilimit2|u16|\* 1000|
|0xC9F8|Dcilimit2time|u16|\* 100|
|0xC9F9|Dcitest|u16||

## ActivePowerConfig (5 registers at 0xCA58):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xCA58|(bitfield)|u16|(RemoteDeratingEnable << 0) \| (RemoteONOFFEnable << 1) \| (ExportLimitEnable << 2) \| (PowerDecreaseRateEnable << 3)|
|0xCA59|ActivePowerLimit|u16||
|0xCA5A|RemoteONorOFF|u16|0=ON, 1=OFF|
|0xCA5B|ExportPower|u16||
|0xCA5C|PowerDecreaseRate|u16|\* 100|

## ACPowerDownConfig (5 registers at 0xCABC):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xCABC|ACHignPowerDownEnable|bit||
|0xCABD|Startpoint|u16|\* 10|
|0xCABE|Speed|u16|\* 10|
|0xCABF|PUModeEntryDelay|u16|\* 100|
|0xCAC0|Backspeed|u16|\* 10|

## MeterConfig (3 registers at 0xCBE8):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xCBE8|MeterFunction|u16|0=Disable, 1=CT, 2=SingleMeter, 3=DoubleMeter|
|0xCBE9|MeterAddress1|u16||
|0xCBEA|MeterAddress2|u16||

## SystemTime (4 registers at 0xCB84):
|Address|Name|Type|Remarks|
|-|-|-|-|
|0xCB84|year|u16|
|0xCB85|monthday|u16|(month << 8) \| day|
|0xCB86|hourminute|u16|(hour << 8) \| minute|
|0xCB87|second|u16|(second << 8)|

## Operation:
CheckUserPassword: write single register 0xC5A8 (u16)

CheckInstallerPassword: write single register 0xC5A9 (u16)

CheckAdministatorPassword: write single register 0xC5AA (u16)

ResetEToday: write 1 to single register 0xC5AB (u16)

ResetETotal: write 1 to single register 0xC5AC (u16)

ResetEventList: write 1 to single register 0xC5AD (u16)

WriteEtoday: write single register 0xC5AF (u16) * 10

ClearEpsOverLoad: write 1 to single register 0xC5B2 (u16)

DRM: write value << 8, value=0 or 1 to single register 0xC5B3 (u16)

RunDRM: write 1 << 8 to single register 0xC5B3 (u16)

ATETesting: write 1 to single register 0xC5B4 (u16)

STOP: write 1 to single register 0xC5B5 (u16)

Ground Fault Monitoring: write (0=ON, 1=OFF) to single register 0xC5BD (u16)


## RemoteControl (1 register at 0xC5AE):
RemoteControl: write (0=ON, 1=OFF) to single register 0xC5AE (u16)

## Other (2 registers at 0xC5B0):
0xC5B0: (ETotal * 10) << 16 (u16)

0xC5B1: (ETotal * 10) & 0xFFFF (u16)

## Unknown
0x2774 (24 registers)
