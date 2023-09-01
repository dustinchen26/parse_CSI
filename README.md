# CSI P1 Payload and CSI PDU Byte Parser
online calculate: https://dustinchen26.github.io/parse_CSI

## Ref Spec
```
Spec 38.212 Table 6.3.1.1.2-7: Mapping order of CSI fields of one CSI report, pmi-FormatIndicator=widebandPMI and cqi-FormatIndicator=widebandCQI
```

## Example
```
(1) Qcom [0xB896]CSI P1 Payload Parser
RI Bit: 2
Zero Padding Bit: 1
PMI WB X1 Bit: 3
PMI WB X2 Bit: 1
WB CQI Bit: 4
CSI P1 Payload: 491

Calculate
RI = 3
Zero Padding = 0
PMI WB X1 = 5
PMI WB X2 = 1
WB CQI = 12

(2) Wireshark_fapi [RX_CSI.ind]CSI PDU Byte Parser
RI Bit: 2
Zero Padding Bit: 1
PMI WB X1 Bit: 3
PMI WB X2 Bit: 1
WB CQI Bit: 4
CSI PDU #0( Byte 0 Byte 1): 0xd780

Calculate
RI = 3
Zero Padding = 0
PMI WB X1 = 5
PMI WB X2 = 1
WB CQI = 12
=================================================================================================================
【Example】
【QXDM_log】
ex:
CSI P1 Payload =491(10進位) => 000111101011(2進位)
反著讀(The most right is the first bit) => 110101111000
拆解(bit 2 1 3 1 4) => 11 0 101 1 1100 0
2進位轉成10進位 =>
RI= 11 => 3
Padding= 0 => 0
PMI X1= 101 => 5
PMI X2= 1 => 1
CQI= 1100 => 12

// (frame, slot)=(459,19), RI Bit: 2 / Zero Padding Bit: 1 / PMI WB X1 Bit: 3 / PMI WB X2 Bit: 1 / WB CQI Bit: 4
// parse Result => RI = 3 / Zero Padding = 0 / PMI WB X1 = 5 / PMI WB X2 = 1 / WB CQI = 12
00:59:43.637	[0xB8A7]	NR5G MAC CSF Report
      Timestamp {
         Slot = 19
         Numerology = 30kHz
         Frame = 459
      }
      Num CSF Reports = 1
      Reports[0] {
         Carrier ID = 0
         Resource Carrier ID = 0
         Report ID = 0
         Report Type =   PERIODIC
         Report Quantity Bitmask = CRI | RI | PMI | CQI
         Late CSF = 0
         Faked CSF = 0
         Num CSI P1 Bits = 11
         CSI {
            Metrics {
               CRI = 0
               RI = 3
               WB CQI = 12
               PMI WB X1 =        5
               PMI WB X2 =        1
               LI = 0
               Num SB = 0
            }
            Bit Width {
               CRI = 0
               RI = 2
               Zero Padding = 1
               WB CQI = 4
               PMI WB X1 =    3
               PMI WB X2 = 1
               LI = 0
               PMI SB X2 = 0
            }

// (frame, slot)=(459,19), CSI P1 Payload: 491
00:59:43.717	[0xB896]	NR5G MAC UCI Payload Information
   Records[1] {
      System Time {
         Slot = 19
         Numerology = 30kHz
         Frame = 459
      }
      Num Channels = 1
      Channel Info[0] {
         Channel = PUCCH_FMT2
         Carrier ID = 0
         Start Symbol = 0
         Num Symbols = 2
         Num HARQ Bits = 0
         Num SR Bits = 0
         Num CSF P1 Bits = 11
         Num CSF P2 Bits = 0
         CSI P1 Payload =        491
      }

【Wireshark_fapi [RX_CSI.ind]】
ex:
CSI PDU #0( Byte 0 Byte 1): 0xd780 => 轉成2進位 1101011110000000
拆解(bit 2 1 3 1 4) => 11 0 101 1 1100 00000
2進位轉成10進位:
RI= 11 => 3
Padding= 0 => 0
PMI X1= 101 => 5
PMI X2= 1 => 1
CQI= 1100 => 12

No.	Time	Source	Destination	Protocol	Info
105034	1970-01-01 09:03:49.788480	10.88.120.11	10.88.120.22	NR_INFO	[RX_CSI.ind]							
L1PA Message: RX_CSI.ind
    Message Type: RX_CSI.ind
    Length of Vendor-specific message body: 0
    Length of message body: 36
    SFN: 459
    SF: 9
    CCI: 0
    Number of Numerologies: 1
    Numerology: 1
        Slot index: 1
        Number of CSI Users: 1
        CSI #0:
            Handle: 0
            RNTI: 221
            Length: 4
            Data Offset: 24
            CSI part 1 CRC: 2 (CRC not present)
            CSI part 2 CRC: 0 (CRC pass)
            Number of reported CSI part 1 bits: 11
            Number of reported CSI part 2 bits: 0
            Timing Advance Tc: -2147483648 Tc (0)
            PUCCH RSSI: 102(-76.5dBFS)
            Effective SNR: 189(30.5dB)
            Reserved 2 bytes
    CSI PDU #0:
        Byte 0: 0xd7
        Byte 1: 0x80
        Reserved 2 bytes

```
