# TAIPAN (Hi-Res version) for APPLE II - DISASSEMBLY

## LINKS

### General

- [forum - reverse engineering Robotron 2084 for the Apple II](http://forum.6502.org/viewtopic.php?f=3&t=5517) mentions (online?) dissembler "White flame"?
- [group - 6502bench SourceGen disassembler updated](https://groups.google.com/g/comp.sys.apple2/c/SBBdH1ChMwQ) mentions the Mini-Assembler and Disambler

### Mini-Assembler/Disassembler

- [Disk - `MINIASSM.SHK`](https://mirrors.apple2.org.za/ftp.apple.asimov.net/images/programming/assembler/MINIASSM.SHK) from [mirrors - ftp apple2](https://mirrors.apple2.org.za/ftp.apple.asimov.net/images/programming/assembler/)
- [Manual - PDF](https://downloads.reactivemicro.com/Users/Grant_Stockley/Apple2WozMiniAssembler.pdf)

### Stack Exchange
[CALL -151 What did it do on the APPLE \]\[](https://stackoverflow.com/q/143374/4424636)
### Monitor
- [Monitor reference](http://www.easy68k.com/paulrsm/6502/MONREF.HTM)
- [Monitor source](http://www.easy68k.com/paulrsm/6502/MON.TXT)

[](https://www.masswerk.at/6502/disassembler.html)

### For ROM hooks
#### `$DB5C` (`OUTDO`), `$FDED` (`COUT`) and `$DD7B` (`FRMEVL`)
[Apple IIc and II e Assembly Language](http://www.apple-iigs.info/doc/fichiers/Apple%20IIc%20and%20IIe%20Assembly%20Language%20(Jules%20H.%20Gilder).pdf)

#### `$FCA8` (`WAIT`), `$E6FB` (`CONINT`), `$FD0C` (`RDKEY`) and `$DEBE` (`CHKCOM`)
[Inside the Apple II e](https://fabiensanglard.net/fd_proxy/prince_of_persia/Inside%20the%20Apple%20IIe.pdf)

#### `$E10C` (`AYINT`)
[Applesoft: Internals--Floating Point Math Package (3 of 3)](http://absurdengineering.org/library/MASTER%20Tech%20Info%20Library/Apple%20II%20Software/Programming/Applesoft%20Basic/ASoft%20Intrnl%20Rtnes%20Info/TIL00076%20-%20Applesoft%20-%20Internals--Floating%20Point%20Math%20Package%20(3%20of%203).pdf)

#### `$EAF9` (`MOVFM`) and `$EFAE` (`RND`)
[Applesoft: Internals--Floating Point Math Package (2 of 3)](http://absurdengineering.org/library/MASTER%20Tech%20Info%20Library/Apple%20II%20Software/Programming/Applesoft%20Basic/ASoft%20Intrnl%20Rtnes%20Info/TIL00075%20-%20Applesoft%20-%20Internals--Floating%20Point%20Math%20Package%20(2%20of%203).pdf)

Also related [Applesoft: Internals--Floating Point Math Package (1 of 3)](http://absurdengineering.org/library/MASTER%20Tech%20Info%20Library/Apple%20II%20Software/Programming/Applesoft%20Basic/ASoft%20Intrnl%20Rtnes%20Info/TIL00074%20-%20Applesoft%20-%20Internals--Floating%20Point%20Math%20Package%20(1%20of%203).pdf)

#### `$36` - `$39`

[Robert Munafo's Embryonic Peeks Pokes and Calls for the Apple II](https://www.mrob.com/pub/xapple2/ppc-rpm.html)

```none
   $36                 lo  Character output hook.
   $37                 hi  Normally points to 9EBD, the DOS output
                           handler (OPUTINCP).
  
   $38                 lo  Character input hook.
   $39                 hi  Normally points to 9E81, the DOS input
                           handler (INPTINCP).
```


## HOW TO DISASSEMBLE PRE-LOADED CODE
Instead of trying to find an AppleSoft compatible disassembler that can load non-destructively, once the code to be disassembled has been loaded, just use the built in monitor ([CALL -151](https://stackoverflow.com/q/143374/4424636)) to get the hex dump and then use an [online disassembler](https://www.masswerk.at/6502/disassembler.html) to get the assembler code.

Enter `3D0G` to return to BASIC.

Alternatively you can use the built-in disassembler, using the Monitor command `L` (listing).

If using the Virtual II emulator, then, prior to entering the Monitor, you could issue a `PR#1` in BASIC, to output the hex dumps (or disassembly listing) to the printer. The resulting printout can be saved as a PDF and then the text copied from that PDF.

## CODE

Note: The end of a subroutine will be denoted by `0x60` [RTS](https://www.masswerk.at/6502/6502_instruction_set.html#RTS)

## CALLS TO BE INVESTIGATED

- 2200 (`0x0898`) General input (put into `WK$` (partially?))
- 2224 (`0x08B0`) Ship sink
- 2368 (`0x0940`) Ship flash and damage animation???
- 2512 (`0x09D0`) Sound (four beeps)
- 2518 (`0x09D6`) Sound (three beeps)
- 2521 (`0x09D9`) Sound (two beeps)
- 2524 (`0x09DC`) Sound (one simple beep)
- 2560 (`0x0A00`) Selective input ("QBS"/"YN"/Etc., in other words, the contents of `CH$`)
- 2680 (`0x0A78`) Numeric input (put into `WK$` apparently, see line 150)

Note that there is a `CALL 6147` (`$1803`) in line 10000. This is not (yet) covered here.

### Additional "internal" code
There are four additional routines that are not called from BASIC, but from within the machine code itself:

 - (`0x0838`) Sound (the actual beeps)
   - `0x0838` (`CALL 2104`) - One beep (from `CALL 2524`)
   - `0x083D` (`CALL 2109`) - Two beep (from `CALL 2521`)
   - `0x0842` (`CALL 2114`) - Three beep (from `CALL 2518`)
   - `0x0847` (`CALL 2119`) - Four beep (from `CALL 2512`)
   - `0x084C` (`CALL 2124`) - Four beep (from `CALL 2512`)
 - (`0x0910`) Ship sink related
 - (`0x0B10`) Maths (uncalled)
 - (`0x09E0 `) UNKNWN2 (uncalled)


## The memory map

### Lowest byte first


- `0x0800` - `0x081F` - BASIC code (line 10) 
- `0x0820` - `0x0837` - Junk
- `0x0838` - `0x086F` - One beep
  - `0x083D` - `0x086F` - Two beep
  - `0x0842` - `0x086F` - Three beep
  - `0x0847` - `0x086F` - Four beep
  - `0x084C` - `0x086F` - Four beep
- `0x0870` - `0x0897` - Junk
- `0x0898` - `0x08AC` - General input
- `0x08AD` - `0x08AF` - Junk
- `0x08B0` - `0x090F` - Ship sink
- `0x0910` - `0x0938` - Ship sink related
- `0x0939` - `0x093F` - Junk
- `0x0940` - `0x09BC` - Ship flash
- `0x09BD` - `0x09CF` - Junk
- `0x09D0` - `0x09DF` - Sound
- `0x09E0` - `0x09F8` - UNKNWN2
- `0x09F9` - `0x09FF` - Junk
- `0x0A00` - `0x0A77` - Selective input
- `0x0A78` - `0x0B0C` - Numeric Input
- `0x0B0D` - `0x0B0F` - Junk
- `0x0B10` - `0x0B3C` - Uncalled maths
- `0x0B3D` - `0x0BFF` - Junk


### Highest byte first


- `0x0B3D` - `0x0BFF` - Junk
- `0x0B10` - `0x0B3C` - Uncalled maths
- `0x0B0D` - `0x0B0F` - Junk
- `0x0A78` - `0x0B0C` - Numeric Input
- `0x0A00` - `0x0A77` - Selective input
- `0x09F9` - `0x09FF` - Junk
- `0x09E0` - `0x09F8` - UNKNWN2
- `0x09D0` - `0x09DF` - Sound
- `0x09BD` - `0x09CF` - Junk
- `0x0940` - `0x09BC` - Ship flash
- `0x0939` - `0x093F` - Junk
- `0x0910` - `0x0938` - Ship sink related
- `0x08B0` - `0x090F` - Ship sink
- `0x08AD` - `0x08AF` - Junk
- `0x0898` - `0x08AC` - General input
- `0x0870` - `0x0897` - Junk
- `0x0838` - `0x086F` - One beep
  - `0x084C` - `0x086F` - Four beep
  - `0x0847` - `0x086F` - Four beep
  - `0x0842` - `0x086F` - Three beep
  - `0x083D` - `0x086F` - Two beep
- `0x0820` - `0x0837` - Junk
- `0x0800` - `0x081F` - BASIC code (line 10) 

Note that, due to the surrounding "junk" code, disassembly can range from `$0838` to `$0B0C` (or `$0B3C`, if you wish to include the unknown maths subroutine.


## The Code - by section

### GENERAL INPUT
`CALL 2200` (`0x0898`)
#### Raw
```none
A2002075FDA0028A
9169C8A9009169C8
A902916960000000
```
#### Hex
```none
0898: A2 00 20 75 FD A0 02 8A
08A0: 91 69 C8 A9 00 91 69 C8
08A8: A9 02 91 69 60 00 00 00
```
#### Assembler
```none
                            * = $0898
0898   A2 00                LDX #$00
089A   20 75 FD             JSR $FD75 (MON:NXTCHAR)
089D   A0 02                LDY #$02
089F   8A                   TXA
08A0   91 69                STA ($69),Y
08A2   C8                   INY
08A3   A9 00                LDA #$00
08A5   91 69                STA ($69),Y
08A7   C8                   INY
08A8   A9 02                LDA #$02
08AA   91 69                STA ($69),Y
08AC   60                   RTS
08AD   00                   BRK
08AE   00                   BRK
08AF   00                   BRK
                            .END
```


### SHIP SINK
`CALL 2224` (`0x08B0`)

Does not include `JSR` from `0x08B0` to `0x0910`.


#### Raw
```none
488A489848A92085
E6A9008D3C09AD39
098D3B09A200AD39
09201009A526853E
A527853FCE3909E8
AD3909201009A526
853CA527853DA000
B13C913EC8C007D0
F7E028D0D1AD3B09
8D3909A94920A8FC
EE3C09AD3C09C928
DOBA68A868AA6860
```
#### Hex
```none
08B0: 48 8A 48 98 48 A9 20 85
08B8: E6 A9 00 8D 3C 09 AD 39
08C0: 09 8D 3B 09 A2 00 AD 39
08C8: 09 20 10 09 A5 26 85 3E
08D0: A5 27 85 3F CE 39 09 E8
08D8: AD 39 09 20 10 09 A5 26
08E0: 85 3C A5 27 85 3D A0 00
08E8: B1 3C 91 3E C8 C0 07 D0
08F0: F7 E0 28 D0 D1 AD 3B 09
08F8: 8D 39 09 A9 49 20 A8 FC
0900: EE 3C 09 AD 3C 09 C9 28
0908: 0D BA 68 A8 68 AA 68 60
```
#### Assembler
```none
                            * = $08B0
08B0   48                   PHA
08B1   8A                   TXA
08B2   48                   PHA
08B3   98                   TYA
08B4   48                   PHA
08B5   A9 20                LDA #$20
08B7   85 E6                STA $E6
08B9   A9 00                LDA #$00
08BB   8D 3C 09             STA $093C
08BE   AD 39 09             LDA $0939
08C1   8D 3B 09             STA $093B
08C4   A2 00                LDX #$00
08C6   AD 39 09   L08C6     LDA $0939
08C9   20 10 09             JSR $0910
08CC   A5 26                LDA $26
08CE   85 3E                STA $3E
08D0   A5 27                LDA $27
08D2   85 3F                STA $3F
08D4   CE 39 09             DEC $0939
08D7   E8                   INX
08D8   AD 39 09             LDA $0939
08DB   20 10 09             JSR $0910
08DE   A5 26                LDA $26
08E0   85 3C                STA $3C
08E2   A5 27                LDA $27
08E4   85 3D                STA $3D
08E6   A0 00                LDY #$00
08E8   B1 3C      L08E8     LDA ($3C),Y
08EA   91 3E                STA ($3E),Y
08EC   C8                   INY
08ED   C0 07                CPY #$07
08EF   D0 F7                BNE L08E8
08F1   E0 28                CPX #$28
08F3   D0 D1                BNE L08C6
08F5   AD 3B 09             LDA $093B
08F8   8D 39 09             STA $0939
08FB   A9 49                LDA #$49
08FD   20 A8 FC             JSR $FCA8    (MON:WAIT)
0900   EE 3C 09             INC $093C
0903   AD 3C 09             LDA $093C
0906   C9 28                CMP #$28
0908   0D BA 68             ORA $68BA
090B   A8                   TAY
090C   68                   PLA
090D   AA                   TAX
090E   68                   PLA
090F   60                   RTS
                            .END

;auto-generated symbols and labels
 L08E8      $08E8
 L08C6      $08C6
```

### SHIP SINK RELATED???
`(CALL 2320)` (`0x0910`)

#### Raw
```none
4829C085264A4A05
2685266885270A0A
0A26270A26270A66
26A527291F05E685
27A5266D3A098526
60570857280100EC
```
#### Hex
```none
0910: 48 29 C0 85 26 4A 4A 05
0918: 26 85 26 68 85 27 0A 0A
0920: 0A 26 27 0A 26 27 0A 66
0928: 26 A5 27 29 1F 05 E6 85
0930: 27 A5 26 6D 3A 09 85 26
0938: 60 57 08 57 28 01 00 EC
```
#### Assembler
```none
                            * = $0910
0910   48                   PHA
0911   29 C0                AND #$C0
0913   85 26                STA $26
0915   4A                   LSR A
0916   4A                   LSR A
0917   05 26                ORA $26
0919   85 26                STA $26
091B   68                   PLA
091C   85 27                STA $27
091E   0A                   ASL A
091F   0A                   ASL A
0920   0A                   ASL A
0921   26 27                ROL $27
0923   0A                   ASL A
0924   26 27                ROL $27
0926   0A                   ASL A
0927   66 26                ROR $26
0929   A5 27                LDA $27
092B   29 1F                AND #$1F
092D   05 E6                ORA $E6
092F   85 27                STA $27
0931   A5 26                LDA $26
0933   6D 3A 09             ADC $093A
0936   85 26                STA $26
0938   60                   RTS
0939   57                   ???                ;%01010111 'W'
093A   08                   PHP
093B   57                   ???                ;%01010111 'W'
093C   28                   PLP
093D   01 00                ORA ($00,X)
093F   EC 00 00             CPX $0000
                            .END
```

### SHIP SINK and SHIP SINK RELATED
Combined:
- `CALL 2224` (`0x08B0`), and;
- (`CALL 2320`) (`0x0910`)

Includes `JSR` from `0x08B0` to `0x0910`.


#### Raw
```none
488A489848A92085
E6A9008D3C09AD39
098D3B09A200AD39
09201009A526853E
A527853FCE3909E8
AD3909201009A526
853CA527853DA000
B13C913EC8C007D0
F7E028D0D1AD3B09
8D3909A94920A8FC
EE3C09AD3C09C928
DOBA68A868AA6860

4829C085264A4A05
2685266885270A0A
0A26270A26270A66
26A527291F05E685
27A5266D3A098526
60570857280100EC
```
#### Hex
```none
08B0: 48 8A 48 98 48 A9 20 85
08B8: E6 A9 00 8D 3C 09 AD 39
08C0: 09 8D 3B 09 A2 00 AD 39
08C8: 09 20 10 09 A5 26 85 3E
08D0: A5 27 85 3F CE 39 09 E8
08D8: AD 39 09 20 10 09 A5 26
08E0: 85 3C A5 27 85 3D A0 00
08E8: B1 3C 91 3E C8 C0 07 D0
08F0: F7 E0 28 D0 D1 AD 3B 09
08F8: 8D 39 09 A9 49 20 A8 FC
0900: EE 3C 09 AD 3C 09 C9 28
0908: 0D BA 68 A8 68 AA 68 60
0910: 48 29 C0 85 26 4A 4A 05
0918: 26 85 26 68 85 27 0A 0A
0920: 0A 26 27 0A 26 27 0A 66
0928: 26 A5 27 29 1F 05 E6 85
0930: 27 A5 26 6D 3A 09 85 26
0938: 60 57 08 57 28 01 00 EC
```
#### Assembler
```none
                            * = $08B0
08B0   48                   PHA
08B1   8A                   TXA
08B2   48                   PHA
08B3   98                   TYA
08B4   48                   PHA
08B5   A9 20                LDA #$20
08B7   85 E6                STA $E6
08B9   A9 00                LDA #$00
08BB   8D 3C 09             STA $093C
08BE   AD 39 09             LDA $0939
08C1   8D 3B 09             STA $093B
08C4   A2 00                LDX #$00
08C6   AD 39 09   L08C6     LDA $0939
08C9   20 10 09             JSR L0910
08CC   A5 26                LDA $26
08CE   85 3E                STA $3E
08D0   A5 27                LDA $27
08D2   85 3F                STA $3F
08D4   CE 39 09             DEC $0939
08D7   E8                   INX
08D8   AD 39 09             LDA $0939
08DB   20 10 09             JSR L0910
08DE   A5 26                LDA $26
08E0   85 3C                STA $3C
08E2   A5 27                LDA $27
08E4   85 3D                STA $3D
08E6   A0 00                LDY #$00
08E8   B1 3C      L08E8     LDA ($3C),Y
08EA   91 3E                STA ($3E),Y
08EC   C8                   INY
08ED   C0 07                CPY #$07
08EF   D0 F7                BNE L08E8
08F1   E0 28                CPX #$28
08F3   D0 D1                BNE L08C6
08F5   AD 3B 09             LDA $093B
08F8   8D 39 09             STA $0939
08FB   A9 49                LDA #$49
08FD   20 A8 FC             JSR $FCA8
0900   EE 3C 09             INC $093C
0903   AD 3C 09             LDA $093C
0906   C9 28                CMP #$28
0908   0D BA 68             ORA $68BA
090B   A8                   TAY
090C   68                   PLA
090D   AA                   TAX
090E   68                   PLA
090F   60                   RTS
0910   48         L0910     PHA
0911   29 C0                AND #$C0
0913   85 26                STA $26
0915   4A                   LSR A
0916   4A                   LSR A
0917   05 26                ORA $26
0919   85 26                STA $26
091B   68                   PLA
091C   85 27                STA $27
091E   0A                   ASL A
091F   0A                   ASL A
0920   0A                   ASL A
0921   26 27                ROL $27
0923   0A                   ASL A
0924   26 27                ROL $27
0926   0A                   ASL A
0927   66 26                ROR $26
0929   A5 27                LDA $27
092B   29 1F                AND #$1F
092D   05 E6                ORA $E6
092F   85 27                STA $27
0931   A5 26                LDA $26
0933   6D 3A 09             ADC $093A
0936   85 26                STA $26
0938   60                   RTS
0939   57                   ???                ;%01010111 'W'
093A   08                   PHP
093B   57                   ???                ;%01010111 'W'
093C   28                   PLP
093D   01 00                ORA ($00,X)
093F   EC 00 00             CPX $0000
                            .END

;auto-generated symbols and labels
 L0910      $0910
 L08E8      $08E8
 L08C6      $08C6
```


### SHIP FLASH
`CALL 2368` (`0x0940`)

#### Raw
```none
488A489848A92085
E6A9008DC009ADBD
098DBF09A200ADBD
09209409A526853C
A527853DE8CEBD09
A000B13C49FF913C
C8C007D0F5E02AD0
DDA95020A8FCADBF
098DBD09EEC009AD
C009C908D0C668A8
68AA68604829C085
264A4A0526852668
85270A0A0A26270A
26270A6626A52729
1F05E68527A5266D
BE09852660570857
0801000000000000
0000000000000000
```
#### Hex
```none
0940: 48 8A 48 98 48 A9 20 85 
0948: E6 A9 00 8D C0 09 AD BD 
0950: 09 8D BF 09 A2 00 AD BD 
0958: 09 20 94 09 A5 26 85 3C 
0960: A5 27 85 3D E8 CE BD 09 
0968: A0 00 B1 3C 49 FF 91 3C 
0970: C8 C0 07 D0 F5 E0 2A D0 
0978: DD A9 50 20 A8 FC AD BF 
0980: 09 8D BD 09 EE C0 09 AD 
0988: C0 09 C9 08 D0 C6 68 A8
0990: 68 AA 68 60 48 29 C0 85 
0998: 26 4A 4A 05 26 85 26 68 
09A0: 85 27 0A 0A 0A 26 27 0A 
09A8: 26 27 0A 66 26 A5 27 29 
09B0: 1F 05 E6 85 27 A5 26 6D 
09B8: BE 09 85 26 60 57 10 57 
09C0: 08 01 00 00 00 00 00 00 
```
#### Assembler
```none
                            * = $0940
0940   48                   PHA
0941   8A                   TXA
0942   48                   PHA
0943   98                   TYA
0944   48                   PHA
0945   A9 20                LDA #$20
0947   85 E6                STA $E6
0949   A9 00                LDA #$00
094B   8D C0 09             STA $09C0
094E   AD BD 09             LDA $09BD
0951   8D BF 09             STA $09BF
0954   A2 00      L0954     LDX #$00
0956   AD BD 09   L0956     LDA $09BD
0959   20 94 09             JSR L0994
095C   A5 26                LDA $26
095E   85 3C                STA $3C
0960   A5 27                LDA $27
0962   85 3D                STA $3D
0964   E8                   INX
0965   CE BD 09             DEC $09BD
0968   A0 00                LDY #$00
096A   B1 3C      L096A     LDA ($3C),Y
096C   49 FF                EOR #$FF
096E   91 3C                STA ($3C),Y
0970   C8                   INY
0971   C0 07                CPY #$07
0973   D0 F5                BNE L096A
0975   E0 2A                CPX #$2A
0977   D0 DD                BNE L0956
0979   A9 50                LDA #$50
097B   20 A8 FC             JSR $FCA8   (MON:WAIT)
097E   AD BF 09             LDA $09BF
0981   8D BD 09             STA $09BD
0984   EE C0 09             INC $09C0
0987   AD C0 09             LDA $09C0
098A   C9 08                CMP #$08
098C   D0 C6                BNE L0954
098E   68                   PLA
098F   A8                   TAY
0990   68                   PLA
0991   AA                   TAX
0992   68                   PLA
0993   60                   RTS
0994   48         L0994     PHA
0995   29 C0                AND #$C0
0997   85 26                STA $26
0999   4A                   LSR A
099A   4A                   LSR A
099B   05 26                ORA $26
099D   85 26                STA $26
099F   68                   PLA
09A0   85 27                STA $27
09A2   0A                   ASL A
09A3   0A                   ASL A
09A4   0A                   ASL A
09A5   26 27                ROL $27
09A7   0A                   ASL A
09A8   26 27                ROL $27
09AA   0A                   ASL A
09AB   66 26                ROR $26
09AD   A5 27                LDA $27
09AF   29 1F                AND #$1F
09B1   05 E6                ORA $E6
09B3   85 27                STA $27
09B5   A5 26                LDA $26
09B7   6D BE 09             ADC $09BE
09BA   85 26                STA $26
09BC   60                   RTS
09BD   57                   ???                ;%01010111 'W'
09BE   10 57                BPL $0A17
09C0   08                   PHP
09C1   01 00                ORA ($00,X)
09C3   00                   BRK
09C4   00                   BRK
09C5   00                   BRK
09C6   00                   BRK
09C7   00                   BRK
                            .END

;auto-generated symbols and labels
 L0994      $0994
 L096A      $096A
 L0956      $0956
 L0954      $0954
```


### FOUR BEEP
`CALL 2512` (`0x09D0`)

#### Raw
```none
4C4C084C47084C42
084C3D084C380860
```
#### Hex
```none
09D0: 4C 4C 08 4C 47 08 4C 42
09D8: 08 4C 3D 08 4C 38 08 60
```
#### Assembler
```none
                            * = $09D0
09D0   4C 4C 08             JMP $084C
09D3   4C 47 08             JMP $0847
09D6   4C 42 08             JMP $0842
09D9   4C 3D 80             JMP $083D
09DC   4C 38 08             JMP $0838
09DF   60                   RTS
                            .END
```

### THREE BEEP
`CALL 2518` (`0x09D6`)

#### Raw
```none
4C42
084C3D084C380860
```
#### Hex
```none
09D6:                   4C 42
09D8: 08 4C 3D 08 4C 38 08 60
```
#### Assembler
```none
                            * = $09D6
09D6   4C 42 08             JMP $0842
09D9   4C 3D 08             JMP $083D
09DC   4C 38 08             JMP $0838
09DF   60                   RTS
                            .END
```

### TWO BEEP
`CALL 2521` (`0x09D9`)

#### Raw
```none
4C3D084C380860
```
#### Hex
```none
09D9:    4C 3D 08 4C 38 08 60
```
#### Assembler
```none
                            * = $09D9
09D9   4C 3D 08             JMP $083D
09DC   4C 38 08             JMP $0838
09DF   60                   RTS
                            .END
```


### SINGLE BEEP
`CALL 2524` (`0x09DC`)

#### Raw
```none
4C380860
```
#### Hex
```none
09DC:             4C 38 08 60
```
#### Assembler
```none
                            * = $09DC
09DC   4C 38 08             JMP $0838
09DF   60                   RTS
                            .END
```

### UNKNWN2
`2528` (`0x09E0`) (could be nothing)

I have assigned this routine the label, `UNKNWN2`.

#### Raw
```none
207BDD20FBE68A48
20BEDE207BDD20FB
E668205CDBCAD0FA
60000004070034
```
#### Hex
```none
09E0: 20 7B DD 20 FB E6 8A 48
09E8: 20 BE DE 20 7B DD 20 FB
09F0: E6 68 20 5C DB CA D0 FA
09F8: 60 00 00 04 07 00 34
```
#### Assembler
```none
                            * = $09E0
09E0   20 7B DD             JSR $DD7B (ROM:FRMEVL)
09E3   20 FB E6             JSR $E6FB (ROM:CONINT)
09E6   8A                   TXA
09E7   48                   PHA
09E8   20 BE DE             JSR $DEBE (ROM:CHKCOM)
09EB   20 7B DD             JSR $DD7B (ROM:FRMEVL)
09EE   20 FB E6             JSR $E6FB (ROM:CONINT)
09F1   68                   PLA
09F2   20 5C DB   L09F2     JSR $DB5C (ROM:OUTDO)
09F5   CA                   DEX
09F6   D0 FA                BNE L09F2
09F8   60                   RTS
09F9   00                   BRK
09FA   00                   BRK
09FB   04                   ???                ;%00000100
09FC   07                   ???                ;%00000111
09FD   00                   BRK
09FE   34                   ???                ;%00110100 '4'
                            .END

;auto-generated symbols and labels
 L09F2      $09F2
```

### SELECTIVE INPUT
`CALL 2560` (`0x0A00`)

#### Raw
```none
A018B1698DFE09A0
09B1698DFD0938C8
B169E9018583C8B1
69E9008584A42420
0CFDC98DD00D2CFE
09F017A0008CFC09
4C6B0A297F8DFF09
ACFD09D183F00988
D0F920DC094C100A
8CFC09105CDBA424
200CFDC98DF01448
A988205CDBA9A020

5CDBA988205CDB68
4C330AADFC09A011
916988A900916960
```
#### Hex
```none
0A00: A0 18 B1 69 8D FE 09 A0
0A08: 09 B1 69 8D FD 09 38 C8
0A10: B1 69 E9 01 85 83 C8 B1
0A18: 69 E9 00 85 84 A4 24 20
0A20: 0C FD C9 8D D0 0D 2C FE
0A28: 09 F0 17 A0 00 8C FC 09
0A30: 4C 6B 0A 29 7F 8D FF 09
0A38: AC FD 09 D1 83 F0 09 88
0A40: D0 F9 20 DC 09 4C 10 0A
0A48: 8C FC 09 10 5C DB A4 24
0A50: 20 0C FD C9 8D F0 14 48
0A58: A9 88 20 5C DB A9 A0 20
0A60: 5C DB A9 88 20 5C DB 68
0A68: 4C 33 0A AD FC 09 A0 11
0A70: 91 69 88 A9 00 91 69 60
```
#### Assembler
```none
                            * = $0A00
0A00   A0 18                LDY #$18
0A02   B1 69                LDA ($69),Y
0A04   8D FE 09             STA $09FE
0A07   A0 09                LDY #$09
0A09   B1 69                LDA ($69),Y
0A0B   8D FD 09             STA $09FD
0A0E   38                   SEC
0A0F   C8                   INY
0A10   B1 69      L0A10     LDA ($69),Y
0A12   E9 01                SBC #$01
0A14   85 83                STA $83
0A16   C8                   INY
0A17   B1 69                LDA ($69),Y
0A19   E9 00                SBC #$00
0A1B   85 84                STA $84
0A1D   A4 24                LDY $24
0A1F   20 0C FD             JSR $FD0C (MON:RDKEY)
0A22   C9 8D                CMP #$8D
0A24   D0 0D                BNE L0A33
0A26   2C FE 09             BIT $09FE
0A29   F0 17                BEQ L0A42
0A2B   A0 00                LDY #$00
0A2D   8C FC 09             STY $09FC
0A30   4C 6B 0A             JMP L0A6B
0A33   29 7F      L0A33     AND #$7F
0A35   8D FF 09             STA $09FF
0A38   AC FD 09             LDY $09FD
0A3B   D1 83      L0A3B     CMP ($83),Y
0A3D   F0 09                BEQ L0A48
0A3F   88                   DEY
0A40   D0 F9                BNE L0A3B
0A42   20 DC 09   L0A42     JSR $09DC (SINGLEBEEP)
0A45   4C 10 0A             JMP L0A10
0A48   8C FC 09   L0A48     STY $09FC
0A4B   10 5C                BPL $0AA9
0A4D   DB                   ???                ;%11011011
0A4E   A4 24                LDY $24
0A50   20 0C FD             JSR $FD0C (MON:RDKEY)
0A53   C9 8D                CMP #$8D
0A55   F0 14                BEQ L0A6B
0A57   48                   PHA
0A58   A9 88                LDA #$88
0A5A   20 5C DB             JSR $DB5C (ROM:OUTDO)
0A5D   A9 A0                LDA #$A0
0A5F   20 5C DB             JSR $DB5C (ROM:OUTDO)
0A62   A9 88                LDA #$88
0A64   20 5C DB             JSR $DB5C (ROM:OUTDO)
0A67   68                   PLA
0A68   4C 33 0A             JMP L0A33
0A6B   AD FC 09   L0A6B     LDA $09FC
0A6E   A0 11                LDY #$11
0A70   91 69                STA ($69),Y
0A72   88                   DEY
0A73   A9 00                LDA #$00
0A75   91 69                STA ($69),Y
0A77   60                   RTS
                            .END

;auto-generated symbols and labels
 L0A33      $0A33
 L0A42      $0A42
 L0A6B      $0A6B
 L0A48      $0A48
 L0A3B      $0A3B
 L0A10      $0A10
```

### NUMERIC INPUT
`CALL 2680` (`0x0A78`)

#### Raw
```none
A002B1698DFD0938
C8B169E9018583C8
B169E9008584A901
8dfC09A424200CFD
297F8DFF09C90DF0
5CADFC09C901D00A
ADFF09C941F0384C
D50AADFF09C908D0
1CACFC09A9209183
A988205CDBA9A020
5CDBA988205CDBCE
FC094C930AADFC09
C90AB01BADFF09C9
309014C93AB010AC
FC099183EEFC0909
80205CDB4C930A20
DC094C930AACFC09
C00AF008A9209183
C84C000B60000000
```
#### Hex
```none
0A78: A0 02 B1 69 8D FD 09 38
0A80: C8 B1 69 E9 01 85 83 C8
0A88: B1 69 E9 00 85 84 A9 01
0A90: 8D FC 09 A4 24 20 0C FD
0A98: 29 7F 8D FF 09 C9 0D F0
0AA0: 5C AD FC 09 C9 01 D0 0A
0AA8: AD FF 09 C9 41 F0 38 4C
0AB0: D5 0A AD FF 09 C9 08 D0
0AB8: 1C AC FC 09 A9 20 91 83
0AC0: 0A C0 A9 88 20 5C DB A9
0AC8: A0 20 5C DB A9 88 20 5C
0AD0: DB CE FC 09 4C 93 0A AD
0AD8: FC 09 C9 0A B0 1B AD FF
0AE0: 09 C9 30 90 14 C9 3A B0
0AE8: 10 AC FC 09 91 83 EE FC
0AF0: 09 09 80 20 5C DB 4C 93
0AF8: 0A 20 0A F8 DC 09 4C 93
0B00: 0A AC FC 09 C0 0A F0 08
0B08: A9 20 91 83 C8 4C 00 0B
0B10: 60 00 00 00
```
#### Assembler
```none
                            * = $0A78
0A78   A0 02                LDY #$02
0A7A   B1 69                LDA ($69),Y
0A7C   8D FD 09             STA $09FD
0A7F   38                   SEC
0A80   C8                   INY
0A81   B1 69                LDA ($69),Y
0A83   E9 01                SBC #$01
0A85   85 83                STA $83
0A87   C8                   INY
0A88   B1 69                LDA ($69),Y
0A8A   E9 00                SBC #$00
0A8C   85 84                STA $84
0A8E   A9 01                LDA #$01
0A90   8D FC 09             STA $09FC
0A93   A4 24      L0A93     LDY $24
0A95   20 0C FD             JSR $FD0C (MON:RDKEY)
0A98   29 7F                AND #$7F
0A9A   8D FF 09             STA $09FF
0A9D   C9 0D                CMP #$0D
0A9F   F0 5C                BEQ L0AFD
0AA1   AD FC 09             LDA $09FC
0AA4   C9 01                CMP #$01
0AA6   D0 0A                BNE L0AB2
0AA8   AD FF 09             LDA $09FF
0AAB   C9 41                CMP #$41
0AAD   F0 38                BEQ L0AE7
0AAF   4C D5 0A             JMP L0AD5
0AB2   AD FF 09   L0AB2     LDA $09FF
0AB5   C9 08                CMP #$08
0AB7   D0 1C                BNE L0AD5
0AB9   AC FC 09             LDY $09FC
0ABC   A9 20                LDA #$20
0ABE   91 83                STA ($83),Y
0AC0   A9 88                LDA #$88
0AC2   20 5C DB             JSR $DB5C (ROM:OUTDO)
0AC5   A9 A0                LDA #$A0
0AC7   20 5C DB             JSR $DB5C (ROM:OUTDO)
0ACA   A9 88                LDA #$88
0ACC   20 5C DB             JSR $DB5C (ROM:OUTDO)
0ACF   CE FC 09             DEC $09FC
0AD2   4C 93 0A             JMP L0A93
0AD5   AD FC 09   L0AD5     LDA $09FC
0AD8   C9 0A                CMP #$0A
0ADA   B0 1B                BCS L0AF7
0ADC   AD FF 09             LDA $09FF
0ADF   C9 30                CMP #$30
0AE1   90 14                BCC L0AF7
0AE3   C9 3A                CMP #$3A
0AE5   B0 10                BCS L0AF7
0AE7   AC FC 09   L0AE7     LDY $09FC
0AEA   91 83                STA ($83),Y
0AEC   EE FC 09             INC $09FC
0AEF   09 80                ORA #$80
0AF1   20 5C DB             JSR $DB5C (ROM:OUTDO)
0AF4   4C 93 0A             JMP L0A93
0AF7   20 DC 09   L0AF7     JSR $09DC (SINGLEBEEP)
0AFA   4C 93 0A             JMP L0A93
0AFD   AC FC 09   L0AFD     LDY $09FC
0B00   C0 0A      L0B00     CPY #$0A
0B02   F0 08                BEQ L0B0C
0B04   A9 20                LDA #$20
0B06   91 83                STA ($83),Y
0B08   C8                   INY
0B09   4C 00 0B             JMP L0B00
0B0C   60         L0B0C     RTS
0B0D   00                   BRK
0B0E   00                   BRK
0B0F   00                   BRK
                            .END

;auto-generated symbols and labels
 L0AFD      $0AFD
 L0AB2      $0AB2
 L0AE7      $0AE7
 L0AD5      $0AD5
 L0A93      $0A93
 L0AF7      $0AF7
 L0B0C      $0B0C
 L0B00      $0B00
```

## Additional internal code sections

Additional code that is called internally, from the assembly listing, rather than externally from BASIC.


### Sound routines

- `0838` (`CALL 2104`) - One beep (from `CALL 2524`)
- `083D` (`CALL 2109`) - Two beep (from `CALL 2521`)
- `0842` (`CALL 2114`) - Three beep (from `CALL 2518`)
- `0847` (`CALL 2119`) - Four beep (from `CALL 2512`)
- `084C` (`CALL 2124`) - Four beep (from `CALL 2512`)

#### Raw
```none
A2014C5108A2034C
5108A2094C5108A2
194C5108A21B4C51
08BD7008F019205F
08E8D0F54C6F0848
A0C0684820A8FCAD
30C088D0F5686060
FF0A000609080706
```
#### Hex
```none
0838: A2 01 4C 51 08 A2 03 4C
0840: 51 08 A2 09 4C 51 08 A2
0848: 19 4C 51 08 A2 1B 4C 51
0850: 08 BD 70 08 F0 19 20 5F
0858: 08 E8 D0 F5 4C 6F 08 48
0860: A0 C0 68 48 20 A8 FC AD
0868: 30 C0 88 D0 F5 68 60 60
```
#### Assembler
```none
                            * = $0838
0838   A2 01                LDX #$01
083A   4C 51 08             JMP L0851
083D   A2 03                LDX #$03
083F   4C 51 08             JMP L0851
0842   A2 09                LDX #$09
0844   4C 51 08             JMP L0851
0847   A2 19                LDX #$19
0849   4C 51 08             JMP L0851
084C   A2 1B                LDX #$1B
084E   4C 51 08             JMP L0851
0851   BD 70 08   L0851     LDA $0870,X
0854   F0 19                BEQ L086F
0856   20 5F 08             JSR L085F
0859   E8                   INX
085A   D0 F5                BNE L0851
085C   4C 6F 08             JMP L086F
085F   48         L085F     PHA
0860   A0 C0                LDY #$C0
0862   68         L0862     PLA
0863   48                   PHA
0864   20 A8 FC             JSR $FCA8 (MON:WAIT)
0867   AD 30 C0             LDA $C030
086A   88                   DEY
086B   D0 F5                BNE L0862
086D   68                   PLA
086E   60                   RTS
086F   60         L086F     RTS
                            .END

;auto-generated symbols and labels
 L0851      $0851
 L086F      $086F
 L085F      $085F
 L0862      $0862
```

Note that the *soft switch* for the speaker is at `$0C30`, and is accesssed in line `0x0867`.

### Ship sink related (`SHPSNK2`)

`(CALL 2320)` (`0x0910`)

See above.

### UNKNWN2

`2528` (`0x09E0`) (could be nothing)

See above.

### Unknown maths routine

`CALL 2680` (`0x0B10`)

Range: `0x0B10` - `0x0B3C`

At first glance, this routine doesn't seem to be called from anywhere, either externally, or from within the machine code. Is it an orphaned routine?

#### Raw

```none
200CE1A5A10AAABD
3D0B8D260BBD3E0B
8D270BA200BD830B
F008A424205CDBE8
D0F3A0E9A91320F9
EA20AEEF60830B84
```

#### Hex

```none
0B10: 20 0C E1 A5 A1 0A AA BD
0B18: 3D 0B 8D 26 0B BD 3E 0B
0B20: 8D 27 0B A2 00 BD 83 0B
0B28: F0 08 A4 24 20 5C DB E8
0B30: D0 F3 A0 E9 A9 13 20 F9
0B38: EA 20 AE EF 60 83 0B 84
```


#### Assembler


```none
0B10   20 0C E1             JSR $E10C  (MATH:AYINT)
0B13   A5 A1                LDA $A1
0B15   0A                   ASL A
0B16   AA                   TAX
0B17   BD 3D 0B             LDA $0B3D,X
0B1A   8D 26 0B             STA $0B26
0B1D   BD 3E 0B             LDA $0B3E,X
0B20   8D 27 0B             STA $0B27
0B23   A2 00                LDX #$00
0B25   BD 83 0B   L0B25     LDA $0B83,X
0B28   F0 08                BEQ L0B32
0B2A   A4 24                LDY $24
0B2C   20 5C DB             JSR $DB5C  (ROM:OUTDO)
0B2F   E8                   INX
0B30   D0 F3                BNE L0B25
0B32   A0 E9      L0B32     LDY #$E9
0B34   A9 13                LDA #$13
0B36   20 F9 EA             JSR $EAF9  (MATH:MOVFM)
0B39   20 AE EF             JSR $EFAE  (MATH:RND)
0B3C   60                   RTS
```

### FRSTCALL

`CALL 6147` (`0x1803`)

This is the first BASIC statement, a `CALL` to `6147`. Does (apparently) nothing when called from the BASIC prompt.

#### Labels

```none
COUT     = $FDED
DOSRHOOK = $03EA

FRSTCALL = $1803
START    = $1820
SECOND   = $180F
SECONDLP = $1814
```


#### Hex dump
```none
1800: 4C 0F 18 4C 20 18 0A 00
1808: 12 4C 25 1B 4C 25 1B 20
1810: 20 18 A0 00 B9 43 18 20
1818: ED FD C8 C0 1D 90 F5 60
1820: A9 75 85 38 A9 18 85 39
1828: A9 19 85 36 A9 19 85 37
1830: 20 EA 03 2C 52 C0 2C 57
1838: C0 20 55 1B 20 0E 1A 2C
1840: 50 C0 60 90 C8 C9 AD D2
```

#### Assembler


```none
 COUT     = $FDED
 DOSRHOOK = $03EA

                            * = $1800
1800   4C 0F 18             JMP SECOND
1803   4C 20 18   FRSTCALL  JMP START
1806   0A                   ASL A
1807   00                   BRK
1808   12                   ???                ;%00010010
1809   4C 25 1B             JMP $1B25
180C   4C 25 1B             JMP $1B25
180F   20 20 18   SECOND    JSR START
1812   A0 00                LDY #$00
1814   B9 43 18   SECONDLP  LDA $1843,Y
1817   20 ED FD             JSR COUT
181A   C8                   INY
181B   C0 1D                CPY #$1D
181D   90 F5                BCC SECONDLP
181F   60                   RTS
1820   A9 75      START     LDA #$75
1822   85 38                STA $38
1824   A9 18                LDA #$18
1826   85 39                STA $39
1828   A9 19                LDA #$19
182A   85 36                STA $36
182C   A9 19                LDA #$19
182E   85 37                STA $37
1830   20 EA 03             JSR DOSRHOOK
1833   2C 52 C0             BIT $C052
1836   2C 57 C0             BIT $C057
1839   20 55 1B             JSR $1B55
183C   20 0E 1A             JSR $1A0E
183F   2C 50 C0             BIT $C050
1842   60                   RTS
                            .END

;defined symbols used as labels
 FRSTCALL   $1803
 SECONDLP   $1814
 START      $1820
 SECOND     $180F
 COUT       $FDED
 DOSRHOOK   $03EA
```

### What are the locations `$36` to `$39`?

See here

```none
1820   A9 75      START     LDA #$75
1822   85 38                STA $38
1824   A9 18                LDA #$18
1826   85 39                STA $39
1828   A9 19                LDA #$19
182A   85 36                STA $36
182C   A9 19                LDA #$19
182E   85 37                STA $37
```

It can be seen that `$36` to `$39` (54-57) have values stashed inside them.

From [Bigger Apple II PEEK, POKE and CALL List](https://www.vintagecomputer.net/browse_thread.cfm?id=790),

> `PRINT PEEK(54)+256*PEEK(55)` Returns the address of the current output routine
> `PRINT PEEK(56)+256*PEEK(57)` Returns the address of the current <strike>output</strike> input routine

Typo: Presumably, the second line should say "input". 

From [Inside the Apple II e](https://fabiensanglard.net/fd_proxy/prince_of_persia/Inside%20the%20Apple%20IIe.pdf), page 183

> The symbolic name for locations `$38`/`$39` is `KSW` (for keyboard switch); `$38` by itself is called `KSWL` and `$39` is called `KSWH`.
> 
> ...
> 
> For those of you who just can't wait, the //e uses an output link called `CSW` (`$36`/`$37`) to point to the output sub- routine that is to take control whenever the standard output sub- routine, `COUT` (`$FDED`), is called.

Also, on page 250,

> The symbolic name for locations $36/$37 is CSW (for character switch); $36 by itself is called CSWL and $37 is called CSWH. CSW is commonly referred to as the "output link" or "output hook."

These are the "hooks" for the input and output. As an aside, is this all `RWTS` related (resetting the input controls, p138)? From page 176:

> The //e's disk operating system (DOS 3.3 or ProDOS) is integrated into the system by storing the address of its special input subrou- tine at KSWL and KSWH.



### Why `JSR DOS_REHOOK`?

Note that, at `$1830`, there is a `JSR` to `DOSRHOOK`. Normally (?), this is a `JMP`, see [Epson MX-80 Text Screen Dump](https://www.txbobsc.com/aal/1983/aal8303.html#a3). There is no `RTS` statement to return from..!


- At `$1830`, there is a `JSR $03EA`. However, shouldn't this be `JMP`, as there is no `RTS`, and `$03EA` is just a jump "vector"? 

Note that there *is* a call vector table at `$03D0` - `$3FF`, which encumpasses `$03EA`. From [Wikipedia - Apple DOS - Technical details](https://en.wikipedia.org/wiki/Apple_DOS#Technical_details),

> A call vector table in the region of `$03D0–03FF` allows programs to find DOS wherever it is loaded in the system memory. For example, if the DOS hooked into the BASIC CLI stops functioning, it can be reinitialized by calling location `$03D0` (`97610`) – hence the traditional `3D0G` ("3D0 go") command to return to BASIC from the System Monitor.



Examining the routine at `$03EA` more closely.

`$03EA` is covered on page 130, [Inside the Apple II e](https://fabiensanglard.net/fd_proxy/prince_of_persia/Inside%20the%20Apple%20IIe.pdf):

> `$3EA`-`$3EC`     A JMP instruction to the DOS 3.3 subroutine that causes it to accept new I/0 links and reconnect it- self. This subroutine must be called to properly in- stall new I/0 subroutines without affecting DOS 3.3 (see Chapters 6 and 7).



#### Disassembly of `$03EA`

`$03EA` is basically, just a jump vector, to `$A851` (i.e. `DOS`)

Using labels from [Micro Apple](https://vintageapple.org/apple_ii/pdf/Micro_Apple_1981.pdf), in particular the `DOS` label:

```none
DOS = $A851
YDOS = $AA5B
KBRD = $C000
STRB = $C010
ESCl = $FC2C
CEOP = $FC42
RKEY = $FD0C
OUTl = $FDF0
KEYN = $FD1B
MNTR = $FF65 
```
  
The hex dump:


```none
03E8: AA 60 4C 51 A8 EA EA 4C
03F0: 59 FA 66 D5 70 4C E0 09
03F8: 4C 65 FF 4C 65 FF 65 FF
0400: A0 A0 A0 A0 A0 A0 A0 A0
```

Disassembling gives

```none
03EA   4C 51 A8   SUB_03EA  JMP DOS
```

It is just a jump!

Conclusion: This is ether:
 - A bug and `JMP $03EA` should have been used instead of `JSR $03EA`, and there is a weird stack issue (a return address is never popped off - potential memory leak???), or 
 - I have a mess up hex dump and have made an error.

#### Further examination and exploration of code

Note that there is a `$60` (`RTS`) preceding, so, just for fun, let's examine some of the code before `$03EA`:

```none
03C0: 2D 2E 2F 30 31 32 FF FF
03C8: 33 34 35 36 37 38 FF 39
03D0: 4C BF 9D 4C 84 9D 4C FD
03D8: AA 4C B5 B7 AD 0F 9D AC
03E0: 0E 9D 60 AD C2 AA AC C1
03E8: AA 60 4C 51 A8 EA EA 4C
03F0: 59 FA 66 D5 70 4C E0 09
03F8: 4C 65 FF 4C 65 FF 65 FF
0400: A0 A0 A0 A0 A0 A0 A0 A0
```

So, from the disassembly (note that the `RWTS` and `RWTS_VEC` labels are discussed below)

```none
 DOS      = $A851
 MNTR     = $FF65
 RWTS     = $B7B5

                            * = $03C0
03C0   2D 2E 2F             AND $2F2E
03C3   30 31                BMI L03F6
03C5   32                   ???                ;%00110010 '2'
03C6   FF                   ???                ;%11111111
03C7   FF                   ???                ;%11111111
03C8   33                   ???                ;%00110011 '3'
03C9   34                   ???                ;%00110100 '4'
03CA   35 36                AND $36,X
03CC   37                   ???                ;%00110111 '7'
03CD   38                   SEC
03CE   FF                   ???                ;%11111111
03CF   39 4C BF             AND $BF4C,Y
03D2   9D 4C 84             STA $844C,X
03D5   9D 4C FD             STA $FD4C,X
03D8   AA                   TAX
03D9   4C B5 B7   RWTS_VEC  JMP RWTS
03DC   AD 0F 9D             LDA $9D0F
03DF   AC 0E 9D             LDY $9D0E
03E2   60                   RTS
03E3   AD C2 AA             LDA $AAC2
03E6   AC C1 AA             LDY $AAC1
03E9   60                   RTS
03EA   4C 51 A8   SUB_03EA  JMP DOS
03ED   EA                   NOP
03EE   EA                   NOP
03EF   4C 59 FA             JMP $FA59
03F2   66 D5                ROR $D5
03F4   70 4C                BVS $0442
03F6   E0 09      L03F6     CPX #$09
03F8   4C 65 FF             JMP MNTR
03FB   4C 65 FF             JMP MNTR
03FE   65 FF                ADC $FF
0400   A0 A0                LDY #$A0
0402   A0 A0                LDY #$A0
0404   A0 A0                LDY #$A0
0406   A0 A0                LDY #$A0
                            .END

;auto-generated symbols and labels
 L03F6      $03F6

;defined symbols used as labels
 DOS        $A851
 MNTR       $FF65
 SUB_03EA   $03EA
 RWTS_VEC   $03D9
 RWTS       $B7B5
```

##### Derivation of labels

It can be seen that (two `RTS` prior to the `JMP DOS` call) there is a preceding `JMP` to `$B7B5` at `$03D9`

`$03D9` (`RWTS_VEC`) is covered on page 137, Inside the Apple IIe:

> It turns out that all diskette I/0 operations performed by DOS 3.3 are executed by a single subroutine called RWTS (Read or Write a Track and Sector) that can be invoked by calling the RWTS page 3 vector at location $3D9 (see Table S-1). 

So,

```none
RWTS_VEC = $03D9
```

From [Robert Munafo's Embryonic Peeks Pokes and Calls for the Apple II](https://www.mrob.com/pub/xapple2/ppc-rpm.html)

```none
$B7B5  47029   -18507     DOS 3.3 RWTS code and its subroutines. This
    to     to              includes most of the physical disk I/O functionality
 $BFFF  49151   -16385     of DOS such as reading and writing blocks, and
                           formatting an entire disk.
```

So we now have a couple of additional labels that can be used in the disassembly:

```none
RWTS_VEC = $03D9
RWTS     = $B7B5
```

Adding `SUB_03EA` as a marker for the call from `$1830`, where there is a `JSR $03EA`, and incorporating the other labels:

```none
DOS = $A851
YDOS = $AA5B
KBRD = $C000
STRB = $C010
ESCl = $FC2C
CEOP = $FC42
RKEY = $FD0C
OUTl = $FDF0
KEYN = $FD1B
MNTR = $FF65 

SUB_03EA = $03EA

RWTS_VEC = $03D9
RWTS     = $B7B5
```


## The Code - The whole thing

The entire block of code from `0x0898` to `0x0B0F` (plus the preceeding `0x0838` to `0x086F`.

### Missing blocks

 - What about the missing block `0x0870` to `0x0897`? 
   - This is just junk, see below
 - And `0x09C8` - `0x09CF`?
   - This is just junk, see below


### Junk blocks
 
 - These seems to be blocks of nonsense bytes - and some of them *can* destroy a continuous listing of the whole block `0x0800` to `0x0BFF`, pages 8 to 11:
   - `0x0800` - `0x0837`
   - `0x0870` - `0x0897`
   - `0x08AD` - `0x08AF`
   - `0x0939` - `0x093F` 
   - `0x09BD` - `0x09CF`
   - `0x09F9` - `0x09FF`
   - `0x0B0D` - `0x0B0F`
   - `0x0B3D` - `0x0BFF`

In fact, according to [this answer](https://retrocomputing.stackexchange.com/a/28128/202) to [Looking for an accurate Apple II(e) character set - in particular, what is CHR$\133)?](https://retrocomputing.stackexchange.com/q/28127/202), one of the character sets (the "chubby" character set) is held at `$0B00`. The olde tyme font is at `$1600`.

However, at '$0B00` there is some code, at `$0A00` - `$0B0C`: `SELINP` and `NUMINP`, plus at `$0B10` - `$0B3C`, the unknown/uncalled maths routine.

The BASIC one-line code is held at `$0800`.

```none
10 POKE 103,1: POKE 104,64: POKE 16384,0: GOTO 10
```

This can be seen in the junk code block, `$0800` - `$0837` ..?

```none
0800: 00 21 08 0A 00 B9 31 30    .....P10
0808: 33 2C 31 3A B9 31 30 34    3,1:P104
0810: 2C 36 34 3A B9 31 36 33    ,64:P163
0818: 38 34 2C 30 3A AB 31 30    84,0:G10
0820: 00 00 00 00 00 00 00 00    ........
0828: 00 00 00 00 00 00 00 00 
0830: 00 00 00 00 00 00 00 00
```
where,

 - `$2C` = ","
 - `$3A` = ":"
 - `$B9` = "P" = POKE
 - `$AB` = "G" = GOTO

#### Junk candidate

One probable candidate (this destroys the listing - but only in the first line of the routine, there after the bytes sort themselves out):

   - `0x0939` - `0x093F` 
     - `093F   EC 48 8A             CPX $8A48`
     - should be
     - `0940   48                   PHA`
     - `0941   8A                   TXA`
   - Note that *sometimes*, admittedly rarely, the value of `$093F` is `$00` and not `$EC`, like so
     - `0938- 60 57 10 57 28 01 00 00` 
   - instead of (the more usual)
     - `0938- 60 57 10 57 28 01 00 EC`
   - The presence of `00` instead of `EC` *seems* to occur when the font is Olde tyme, after the break - however, not always. It was seen (once) on a Apple //e enhanced, UK machine
     - The break (Ctrl-C) can led to a third character set of not chubby character, nor Olde tyme characters, but characters that look like bits of ships. Unfortunately, the monitor output is unreadable when this third case occurs, due to the character set.
     - The selection of font is time dependant between (ESC and Ctrl-C) or (break after start up time)? 
       - Chubby - (after reboot) wait ESC long time after start up (10+ seconds?), immediate Ctrl-C
         - Used for the "Mega-micro computers" and "Avalanche Productions Inc." logos, and the "Press 'ESC' key to start" message, on the splash screen.
         - Similar to *Pinocchio* at [52:52](https://www.youtube.com/watch?v=9EDtnLRuFuU&t=3172) in [0096 Giving my Apple \]\[+ multiple personalities with ROMX+](https://www.youtube.com/watch?v=9EDtnLRuFuU)
       - Chubby Inverted - (after reboot) immediate ESC after start up, immediate Ctrl-C
       - Old Tyme - New boot of fresh machine (unable to reproduce)???
         - Similar to *Gothic* at [52:08](https://www.youtube.com/watch?v=9EDtnLRuFuU&t=3128) in [0096 Giving my Apple \]\[+ multiple personalities with ROMX+](https://www.youtube.com/watch?v=9EDtnLRuFuU)
       - Ships - Hit Ctrl-C just as the name your character dialog is started being drawn.

### Random, uncalled, code blocks

 - Random (maths) Code:
   - At decimal location: `2680`
   - Range: `0x0B10` - `0x0B3C`
 - UNKNWN2:
   - At decimal location: `2528` 
   - Range: `0x09E0` - `0x09F8`
 - Aren't there others?

### The entire code block

#### Labels

```none
; #### Additional labels ####
; Additional Labels, for routines *external* to Taipan's code base, for the [symbol table](https://www.masswerk.at/6502/disassembler.html) (i.e ROM calls, etc.):

 OUTDO    = $DB5C
 FRMEVL   = $DD7B
 WAIT     = $FCA8
 CONINT   = $E6FB
 CHKCOM   = $DEBE
 AYINT    = $E10C
 MOVFM    = $EAF9
 RND      = $EFAE
 RDKEY    = $FD0C

; ##### Optional labels #####
; Additional Labels, for routines *internal* to Taipan's code base, for the [symbol table](https://www.masswerk.at/6502/disassembler.html)

 BEEP1    = $0838     ; called by SOUNDx routine(s)
 BEEP2    = $083D     ; called by SOUNDx routine(s)
 BEEP3    = $0842     ; called by SOUNDx routine(s)
 BEEP4A   = $0847     ; called by SOUNDx routine(s)
 BEEP4B   = $084C     ; called by SOUNDx routine(s)
 GENINP   = $0898     ; General input
 SHPSNK   = $08B0     ; Ship sinking
 SHKSNK2  = $0910     ; Ship shinking (related)
 SHPFSH   = $0940     ; Ship flash
 SOUND4   = $09D0     ; Sound (four beeps)
 SPUND3   = $09D6     ; Sound (three beeps)
 SOUND2   = $09D9     ; Sound (two beeps)
 SOUND1   = $09DC     ; Sound (one beep)
 UNKNWN2  = $09E0     ; Unknown/uncalled???
 SELINP   = $0A00     ; Selective input
 NUMINP   = $0A78     ; Numeric input
 SHPMATH  = $0B10     ; Unknown/uncalled maths routine???

; ##### More optional labels #####
; Additional labels, for branches *within* routines

 SNDRTN   = $086F     ; Sound RTS
 GENRTN   = $08AC     ; General input RTS

 SHPSNKL1 = $08C4     ; Internal loop #1 - ship sink
 SHPSNKL2 = $08C6     ; Internal loop #2 - ship sink
 SHPSNKL3 = $08E8     ; Internal loop #3 - ship sink
 SHPFSHL1 = $0954     ; Internal loop #1 - ship flash
 SHPFSHL2 = $0956     ; Internal loop #2 - ship flash
 SHPFSHL3 = $096A     ; Internal loop #3 - ship flash
 SHPFSHED = $0994     ; Final part of - ship flash
 UNKNWNLP = $09F2     ; Internal loop - unknown/uncalled routine
 SELIP1   = $0A1D     ; Internal loop #1 - selective input
 SELIP2   = $0A33     ; Internal loop #2 - selective input
 SELIP3   = $0A3B     ; Internal loop #3 - selective input
 SELIP4   = $0A42     ; Internal loop #4 - selective input
 SELIP5   = $0A48     ; Internal loop #5 - selective input
 SELIP6   = $0A6B     ; Internal loop #6 - selective input
 NUMIP1   = $0A93     ; Internal loop #1 - numeric input
 NUMIP2   = $0AB2     ; Internal loop #2 - numeric input
 NUMIP3   = $0AD5     ; Internal loop #3 - numeric input
 NUMIP4   = $0AE7     ; Internal loop #4 - numeric input
 NUMIP5   = $0AF7     ; Internal loop #5 - numeric input
 NUMIP6   = $0AFD     ; Internal loop #6 - numeric input
 NUMIP7   = $0B00     ; Internal loop #7 - numeric input
 NUMRTN   = $0B0C     ; Numeric input RTS

 BEEPDO   = $0862     ; Make the actual sound
 BEEPJMP1 = $0851     ; Internal jump #1 - beep
 BEEPJMP2 = $085F     ; Internal jump #2 - beep

 SHPMATH1 = $0B25     ; Internal loop #1 - unknown math routine
 SHPMATH2 = $0B32     ; Internal loop #2 - unknown math routine

; Labels auto-generated by disassembler
; L0993    = $0993 ; Fake call from junk bytes (`0x093A`)
; L0A17    = $0A17 ; Fake call from junk bytes (`0x09BE`)
; Disassembling to just $0B3C, instead of $0BFF, truncates the following labels:
; L0BA5    = $0BA5 ; Fake call from junk bytes (`0x0B6C`)
; L0BC8    = $0BC8 ; Fake call from junk bytes (`0x0B6E`)
; L0BD6    = $0BD6 ; Fake call from junk bytes (`0x0B70`)
; L0B24    = $0B24 ; Fake call from junk bytes (`0x0B72`)
; L0B55    = $0B55 ; Fake call from junk bytes (`0x0B74`)
; L0BBE    = $0BBE ; Fake call from junk bytes (`0x0B76`)
; L0BCD    = $0BCD ; Fake call from junk bytes (`0x0BDC`)
```


#### Hex dump (non-contiguous)
```none
0838: A2 01 4C 51 08 A2 03 4C
0840: 51 08 A2 09 4C 51 08 A2
0848: 19 4C 51 08 A2 1B 4C 51
0850: 08 BD 70 08 F0 19 20 5F
0858: 08 E8 D0 F5 4C 6F 08 48
0860: A0 C0 68 48 20 A8 FC AD
0868: 30 C0 88 D0 F5 68 60 60

...

0898: A2 00 20 75 FD A0 02 8A
08A0: 91 69 C8 A9 00 91 69 C8
08A8: A9 02 91 69 60 00 00 00

08B0: 48 8A 48 98 48 A9 20 85
08B8: E6 A9 00 8D 3C 09 AD 39
08C0: 09 8D 3B 09 A2 00 AD 39
08C8: 09 20 10 09 A5 26 85 3E
08D0: A5 27 85 3F CE 39 09 E8
08D8: AD 39 09 20 10 09 A5 26
08E0: 85 3C A5 27 85 3D A0 00
08E8: B1 3C 91 3E C8 C0 07 D0
08F0: F7 E0 28 D0 D1 AD 3B 09
08F8: 8D 39 09 A9 49 20 A8 FC
0900: EE 3C 09 AD 3C 09 C9 28
0908: 0D BA 68 A8 68 AA 68 60
0910: 48 29 C0 85 26 4A 4A 05
0918: 26 85 26 68 85 27 0A 0A
0920: 0A 26 27 0A 26 27 0A 66
0928: 26 A5 27 29 1F 05 E6 85
0930: 27 A5 26 6D 3A 09 85 26
0938: 60 57 08 57 28 01 00 EC

0940: 48 8A 48 98 48 A9 20 85
0948: E6 A9 00 8D C0 09 AD BD
0950: 09 20 94 09 A5 26 85 3C
0958: A5 27 85 3D E8 CE BD 09
0960: A0 00 B1 3C 49 FF 91 3C
0968: C8 C0 07 D0 F5 E0 2A D0
0970: DD A9 50 20 A8 FC AD BF
0978: 09 8D BD 09 EE C0 09 AD
0980: C0 09 C9 08 D0 C6 68 A8
0988: 68 AA 68 60 48 29 C0 85
0990: 26 4A 4A 05 26 85 26 68
0998: 85 27 0A 0A 0A 26 27 0A
09A0: 26 27 0A 66 26 A5 27 29
09A8: 1F 05 E6 85 27 A5 26 6D
09B0: BE 09 85 26 60 57 08 57
09B8: 08 01 00 00 00 00 00 00
09C0: 00 00 00 00 00 00 00 00

...
09C8:
...

09D0: 4C 4C 08 4C 47 08 4C 42
09D8: 08 4C 3D 08 4C 38 08 60

09E0: 20 7B DD 20 FB E6 8A 48
09E8: 20 BE DE 20 7B DD 20 FB
09F0: E6 68 20 5C DB CA D0 FA
09F8: 60 00 00 04 07 00 34    IIe (UK)
09F8: 60 00 00 00 02 02 00 32 IIe Enhanced (UK)

0A00: A0 18 B1 69 8D FE 09 A0
0A08: 09 B1 69 8D FD 09 38 C8
0A10: B1 69 E9 01 85 83 C8 B1
0A18: 69 E9 00 85 84 A4 24 20
0A20: 0C FD C9 8D D0 0D 2C FE
0A28: 09 F0 17 A0 00 8C FC 09
0A30: 4C 6B 0A 29 7F 8D FF 09
0A38: AC FD 09 D1 83 F0 09 88
0A40: D0 F9 20 DC 09 4C 10 0A
0A48: 8C FC 09 10 5C DB A4 24
0A50: 20 0C FD C9 8D F0 14 48
0A58: A9 88 20 5C DB A9 A0 20
0A60: 5C DB A9 88 20 5C DB 68
0A68: 4C 33 0A AD FC 09 A0 11
0A70: 91 69 88 A9 00 91 69 60

0A78: A0 02 B1 69 8D FD 09 38
0A80: C8 B1 69 E9 01 85 83 C8
0A88: B1 69 E9 00 85 84 A9 01
0A90: 8D FC 09 A4 24 20 0C FD
0A98: 29 7F 8D FF 09 C9 0D F0
0AA0: 5C AD FC 09 C9 01 D0 0A
0AA8: AD FF 09 C9 41 F0 38 4C
0AB0: D5 0A AD FF 09 C9 08 D0
0AB8: 1C AC FC 09 A9 20 91 83
0AC0: 0A C0 A9 88 20 5C DB A9
0AC8: A0 20 5C DB A9 88 20 5C
0AD0: DB CE FC 09 4C 93 0A AD
0AD8: FC 09 C9 0A B0 1B AD FF
0AE0: 09 C9 30 90 14 C9 3A B0
0AE8: 10 AC FC 09 91 83 EE FC
0AF0: 09 09 80 20 5C DB 4C 93
0AF8: 0A 20 0A F8 DC 09 4C 93
0B00: 0A AC FC 09 C0 0A F0 08
0B08: A9 20 91 83 C8 4C 00 0B
0B10: 60 00 00 00
```

#### Contiguous hex dump from PR#1 monitor output (`0x0800` - `0x0BFF`)

```none
0800: 00 21 08 0A 00 B9 31 30 
0808: 33 2C 31 3A B9 31 30 34 
0810: 2C 36 34 3A B9 31 36 33 
0818: 38 34 2C 30 3A AB 31 30 
0820: 00 00 00 00 00 00 00 00 
0828: 00 00 00 00 00 00 00 00 
0830: 00 00 00 00 00 00 00 00 
0838: A2 01 4C 51 08 A2 03 4C 
0840: 51 08 A2 09 4C 51 08 A2 
0848: 19 4C 51 08 A2 1B 4C 51 
0850: 08 BD 70 08 F0 19 20 5F 
0858: 08 E8 D0 F5 4C 6F 08 48 
0860: A0 C0 68 48 20 A8 FC AD 
0868: 30 C0 88 D0 F5 68 60 60 
0870: FF 0A 00 06 09 08 07 06 
0878: 00 08 06 05 04 03 08 06 
0880: 05 04 03 08 06 05 04 03 
0888: 00 12 00 07 09 08 09 07 
0890: 07 09 08 09 07 00 00 00 
0898: A2 00 20 75 FD A0 02 8A 
08A0: 91 69 C8 A9 00 91 69 C8 
08A8: A9 02 91 69 60 00 00 00 
08B0: 48 8A 48 98 48 A9 20 85 
08B8: E6 A9 00 8D 3C 09 AD 39 
08C0: 09 8D 3B 09 A2 00 AD 39 
08C8: 09 20 10 09 A5 26 85 3E 
08D0: A5 27 85 3F CE 39 09 E8 
08D8: AD 39 09 20 10 09 A5 26 
08E0: 85 3C A5 27 85 3D A0 00 
08E8: B1 3C 91 3E C8 C0 07 D0 
08F0: F7 E0 28 D0 D1 AD 3B 09 
08F8: 8D 39 09 A9 56 20 A8 FC 
0900: EE 3C 09 AD 3C 09 C9 28 
0908: D0 BA 68 A8 68 AA 68 60 
0910: 48 29 C0 85 26 4A 4A 05 
0918: 26 85 26 68 85 27 0A 0A 
0920: 0A 26 27 0A 26 27 0A 66 
0928: 26 A5 27 29 1F 05 E6 85 
0930: 27 A5 26 6D 3A 09 85 26 
0938: 60 57 10 57 28 01 00 EC 
0940: 48 8A 48 98 48 A9 20 85 
0948: E6 A9 00 8D C0 09 AD BD 
0950: 09 8D BF 09 A2 00 AD BD 
0958: 09 20 94 09 A5 26 85 3C 
0960: A5 27 85 3D E8 CE BD 09 
0968: A0 00 B1 3C 49 FF 91 3C 
0970: C8 C0 07 D0 F5 E0 2A D0 
0978: DD A9 50 20 A8 FC AD BF 
0980: 09 8D BD 09 EE C0 09 AD 
0988: C0 09 C9 08 D0 C6 68 A8
0990: 68 AA 68 60 48 29 C0 85 
0998: 26 4A 4A 05 26 85 26 68 
09A0: 85 27 0A 0A 0A 26 27 0A 
09A8: 26 27 0A 66 26 A5 27 29 
09B0: 1F 05 E6 85 27 A5 26 6D 
09B8: BE 09 85 26 60 57 10 57 
09C0: 08 01 00 00 00 00 00 00 
09C8: 00 00 00 00 00 00 00 00 
09D0: 4C 4C 08 4C 47 08 4C 42 
09D8: 08 4C 3D 08 4C 38 08 60 
09E0: 20 7B DD 20 FB E6 8A 48 
09E8: 20 BE DE 20 7B DD 20 FB 
09F0: E6 68 20 5C DB CA D0 FA 
09F8: 60 00 00 00 02 02 00 32 
0A00: A0 18 B1 69 8D FE 09 A0 
0A08: 09 B1 69 8D FD 09 38 C8 
0A10: B1 69 E9 01 85 83 C8 B1 
0A18: 69 E9 00 85 84 A4 24 20 
0A20: 0C FD C9 8D D0 0D 2C FE 
0A28: 09 F0 17 A0 00 8C FC 09 
0A30: 4C 6B 0A 29 7F 8D FF 09 
0A38: AC FD 09 D1 83 F0 09 88 
0A40: D0 F9 20 DC 09 4C 1D 0A 
0A48: 8C FC 09 20 5C DB A4 24 
0A50: 20 0C FD C9 8D F0 14 48 
0A58: A9 88 20 5C DB A9 A0 20 
0A60: 5C DB A9 88 20 5C DB 68 
0A68: 4C 33 0A AD FC 09 A0 11 
0A70: 91 69 88 A9 00 91 69 60 
0A78: A0 02 B1 69 8D FD 09 38 
0A80: C8 B1 69 E9 01 85 83 C8 
0A88: B1 69 E9 00 85 84 A9 01 
0A90: 8D FC 09 A4 24 20 0C FD 
0A98: 29 7F 8D FF 09 C9 0D F0 
0AA0: 5C AD FC 09 C9 01 D0 0A 
0AA8: AD FF 09 C9 41 F0 38 4C 
0AB0: D5 0A AD FF 09 C9 08 D0 
0AB8: 1C AC FC 09 A9 20 91 83 
0AC0: A9 88 20 5C DB A9 A0 20 
0AC8: 5C DB A9 88 20 5C DB CE 
0AD0: FC 09 4C 93 0A AD FC 09 
0AD8: C9 0A B0 1B AD FF 09 C9 
0AE0: 30 90 14 C9 3A B0 10 AC 
0AE8: FC 09 91 83 EE FC 09 09 
0AF0: 80 20 5C DB 4C 93 0A 20 
0AF8: DC 09 4C 93 0A AC FC 09 
0B00: C0 0A F0 08 A9 20 91 83 
0B08: C8 4C 00 0B 60 00 00 00 
0B10: 20 0C E1 A5 A1 0A AA BD 
0B18: 3D 0B 8D 26 0B BD 3E 0B 
0B20: 8D 27 0B A2 00 BD 83 0B 
0B28: F0 08 A4 24 20 5C DB E8 
0B30: D0 F3 A0 E9 A9 13 20 F9 
0B38: EA 20 AE EF 60 83 0B 84 
0B40: 0B 8E 0B 99 0B A2 0B B5 
0B48: 0B C8 0B E4 0B 01 0C 12 
0B50: 0C 1F 0C 2D 0C 74 0C 94 
0B58: 0C D5 0C 59 0D C9 0D 54 
0B60: 0E 6C 0E 8B 0E C9 0E 43 
0B68: 0F D5 0F 13 10 37 10 58 
0B70: 10 64 10 B0 10 DF 10 46 
0B78: 11 69 11 8E 11 B6 11 D0 
0B80: 11 F2 11 00 C8 EF EE E7 
0B88: A0 CB EF EE E7 00 A0 D7 
0B90: E1 F2 E5 E8 EF F5 F3 E5 
0B98: 00 CC EF E3 E1 F4 E9 EF
0BA0: EE 00 C3 EF ED F0 F2 E1 
0BA8: E4 EF F2 A7 F3 A0 D2 E5 
0BB0: F0 EF F2 F4 00 A0 A0 C3 
0BB8: E1 F0 F4 E1 E9 EE A7 F3 
0BC0: A0 D2 E5 F0 EF F2 F4 00 
0BC8: C8 EF F7 A0 ED F5 E3 E8 
0BD0: A0 F7 E9 EC EC A0 F9 EF 
0BD8: F5 A0 E4 E5 F0 EF F3 E9 
0BE0: F4 BF A0 00 C8 EF F7 A0 
0BE8: ED F5 E3 E8 A0 F7 E9 EC 
0BF0: EC A0 F9 EF F5 A0 F7 E9 
0BF8: F4 E8 E4 F2 E1 F7 BF A0 
```


#### Assembly code (as per the hex dump)

Unfortunately, the hex dump, "as is", has a messed up start of the ship flash routine, `SHPFSH` (`0x0940`), due to the junk code (`$EC` instead of `$00`) at `0x093F`, preceding `SHPFSH`, which breaks the disassembly:

```none
093D   01 00                ORA ($00,X)
093F   EC 48 8A             CPX $8A48
0942   48                   PHA
0943   98                   TYA
0944   48                   PHA
```
should be

```none
093D   01 00                ORA ($00,X)
093F   00                   BRK
0940   48                   PHA
0941   8A                   TXA
0942   48                   PHA
0943   98                   TYA
0944   48                   PHA
```
We can delete the assembly listing below as it is incorrect and essentially the same as the corrected disassembly below (apart from the byte at `$093F`).



#### Assembly code (with `$093F` as `00`)

Fixes the messed up start of SHPFSH (`0x0940`). Otherwise junk code preceding SHPFSH breaks the disassembly.

```none
                            * = $0800
0800   00                   BRK
0801   21 08                AND ($08,X)
0803   0A                   ASL A
0804   00                   BRK
0805   B9 31 30             LDA $3031,Y
0808   33                   ???                ;%00110011 '3'
0809   2C 31 3A             BIT $3A31
080C   B9 31 30             LDA $3031,Y
080F   34                   ???                ;%00110100 '4'
0810   2C 36 34             BIT $3436
0813   3A                   ???                ;%00111010 ':'
0814   B9 31 36             LDA $3631,Y
0817   33                   ???                ;%00110011 '3'
0818   38                   SEC
0819   34                   ???                ;%00110100 '4'
081A   2C 30 3A             BIT $3A30
081D   AB                   ???                ;%10101011
081E   31 30                AND ($30),Y
0820   00                   BRK
0821   00                   BRK
0822   00                   BRK
0823   00                   BRK
0824   00                   BRK
0825   00                   BRK
0826   00                   BRK
0827   00                   BRK
0828   00                   BRK
0829   00                   BRK
082A   00                   BRK
082B   00                   BRK
082C   00                   BRK
082D   00                   BRK
082E   00                   BRK
082F   00                   BRK
0830   00                   BRK
0831   00                   BRK
0832   00                   BRK
0833   00                   BRK
0834   00                   BRK
0835   00                   BRK
0836   00                   BRK
0837   00                   BRK
0838   A2 01      L0838     LDX #$01
083A   4C 51 08             JMP L0851
083D   A2 03      L083D     LDX #$03
083F   4C 51 08             JMP L0851
0842   A2 09      L0842     LDX #$09
0844   4C 51 08             JMP L0851
0847   A2 19      L0847     LDX #$19
0849   4C 51 08             JMP L0851
084C   A2 1B      L084C     LDX #$1B
084E   4C 51 08             JMP L0851
0851   BD 70 08   L0851     LDA $0870,X
0854   F0 19                BEQ L086F
0856   20 5F 08             JSR L085F
0859   E8                   INX
085A   D0 F5                BNE L0851
085C   4C 6F 08             JMP L086F
085F   48         L085F     PHA
0860   A0 C0                LDY #$C0
0862   68         L0862     PLA
0863   48                   PHA
0864   20 A8 FC             JSR $FCA8
0867   AD 30 C0             LDA $C030
086A   88                   DEY
086B   D0 F5                BNE L0862
086D   68                   PLA
086E   60                   RTS
086F   60         L086F     RTS
0870   FF                   ???                ;%11111111
0871   0A                   ASL A
0872   00                   BRK
0873   06 09                ASL $09
0875   08                   PHP
0876   07                   ???                ;%00000111
0877   06 00                ASL $00
0879   08                   PHP
087A   06 05                ASL $05
087C   04                   ???                ;%00000100
087D   03                   ???                ;%00000011
087E   08                   PHP
087F   06 05                ASL $05
0881   04                   ???                ;%00000100
0882   03                   ???                ;%00000011
0883   08                   PHP
0884   06 05                ASL $05
0886   04                   ???                ;%00000100
0887   03                   ???                ;%00000011
0888   00                   BRK
0889   12                   ???                ;%00010010
088A   00                   BRK
088B   07                   ???                ;%00000111
088C   09 08                ORA #$08
088E   09 07                ORA #$07
0890   07                   ???                ;%00000111
0891   09 08                ORA #$08
0893   09 07                ORA #$07
0895   00                   BRK
0896   00                   BRK
0897   00                   BRK
0898   A2 00                LDX #$00
089A   20 75 FD             JSR $FD75
089D   A0 02                LDY #$02
089F   8A                   TXA
08A0   91 69                STA ($69),Y
08A2   C8                   INY
08A3   A9 00                LDA #$00
08A5   91 69                STA ($69),Y
08A7   C8                   INY
08A8   A9 02                LDA #$02
08AA   91 69                STA ($69),Y
08AC   60                   RTS
08AD   00                   BRK
08AE   00                   BRK
08AF   00                   BRK
08B0   48                   PHA
08B1   8A                   TXA
08B2   48                   PHA
08B3   98                   TYA
08B4   48                   PHA
08B5   A9 20                LDA #$20
08B7   85 E6                STA $E6
08B9   A9 00                LDA #$00
08BB   8D 3C 09             STA $093C
08BE   AD 39 09             LDA $0939
08C1   8D 3B 09             STA $093B
08C4   A2 00      L08C4     LDX #$00
08C6   AD 39 09   L08C6     LDA $0939
08C9   20 10 09             JSR L0910
08CC   A5 26                LDA $26
08CE   85 3E                STA $3E
08D0   A5 27                LDA $27
08D2   85 3F                STA $3F
08D4   CE 39 09             DEC $0939
08D7   E8                   INX
08D8   AD 39 09             LDA $0939
08DB   20 10 09             JSR L0910
08DE   A5 26                LDA $26
08E0   85 3C                STA $3C
08E2   A5 27                LDA $27
08E4   85 3D                STA $3D
08E6   A0 00                LDY #$00
08E8   B1 3C      L08E8     LDA ($3C),Y
08EA   91 3E                STA ($3E),Y
08EC   C8                   INY
08ED   C0 07                CPY #$07
08EF   D0 F7                BNE L08E8
08F1   E0 28                CPX #$28
08F3   D0 D1                BNE L08C6
08F5   AD 3B 09             LDA $093B
08F8   8D 39 09             STA $0939
08FB   A9 56                LDA #$56
08FD   20 A8 FC             JSR $FCA8
0900   EE 3C 09             INC $093C
0903   AD 3C 09             LDA $093C
0906   C9 28                CMP #$28
0908   D0 BA                BNE L08C4
090A   68                   PLA
090B   A8                   TAY
090C   68                   PLA
090D   AA                   TAX
090E   68                   PLA
090F   60                   RTS
0910   48         L0910     PHA
0911   29 C0                AND #$C0
0913   85 26                STA $26
0915   4A                   LSR A
0916   4A                   LSR A
0917   05 26                ORA $26
0919   85 26                STA $26
091B   68                   PLA
091C   85 27                STA $27
091E   0A                   ASL A
091F   0A                   ASL A
0920   0A                   ASL A
0921   26 27                ROL $27
0923   0A                   ASL A
0924   26 27                ROL $27
0926   0A                   ASL A
0927   66 26                ROR $26
0929   A5 27                LDA $27
092B   29 1F                AND #$1F
092D   05 E6                ORA $E6
092F   85 27                STA $27
0931   A5 26                LDA $26
0933   6D 3A 09             ADC $093A
0936   85 26                STA $26
0938   60                   RTS
0939   57                   ???                ;%01010111 'W'
093A   10 57                BPL L0993
093C   28                   PLP
093D   01 00                ORA ($00,X)
093F   00                   BRK
0940   48                   PHA
0941   8A                   TXA
0942   48                   PHA
0943   98                   TYA
0944   48                   PHA
0945   A9 20                LDA #$20
0947   85 E6                STA $E6
0949   A9 00                LDA #$00
094B   8D C0 09             STA $09C0
094E   AD BD 09             LDA $09BD
0951   8D BF 09             STA $09BF
0954   A2 00      L0954     LDX #$00
0956   AD BD 09   L0956     LDA $09BD
0959   20 94 09             JSR L0994
095C   A5 26                LDA $26
095E   85 3C                STA $3C
0960   A5 27                LDA $27
0962   85 3D                STA $3D
0964   E8                   INX
0965   CE BD 09             DEC $09BD
0968   A0 00                LDY #$00
096A   B1 3C      L096A     LDA ($3C),Y
096C   49 FF                EOR #$FF
096E   91 3C                STA ($3C),Y
0970   C8                   INY
0971   C0 07                CPY #$07
0973   D0 F5                BNE L096A
0975   E0 2A                CPX #$2A
0977   D0 DD                BNE L0956
0979   A9 50                LDA #$50
097B   20 A8 FC             JSR $FCA8
097E   AD BF 09             LDA $09BF
0981   8D BD 09             STA $09BD
0984   EE C0 09             INC $09C0
0987   AD C0 09             LDA $09C0
098A   C9 08                CMP #$08
098C   D0 C6                BNE L0954
098E   68                   PLA
098F   A8                   TAY
0990   68                   PLA
0991   AA                   TAX
0992   68                   PLA
0993   60         L0993     RTS
0994   48         L0994     PHA
0995   29 C0                AND #$C0
0997   85 26                STA $26
0999   4A                   LSR A
099A   4A                   LSR A
099B   05 26                ORA $26
099D   85 26                STA $26
099F   68                   PLA
09A0   85 27                STA $27
09A2   0A                   ASL A
09A3   0A                   ASL A
09A4   0A                   ASL A
09A5   26 27                ROL $27
09A7   0A                   ASL A
09A8   26 27                ROL $27
09AA   0A                   ASL A
09AB   66 26                ROR $26
09AD   A5 27                LDA $27
09AF   29 1F                AND #$1F
09B1   05 E6                ORA $E6
09B3   85 27                STA $27
09B5   A5 26                LDA $26
09B7   6D BE 09             ADC $09BE
09BA   85 26                STA $26
09BC   60                   RTS
09BD   57                   ???                ;%01010111 'W'
09BE   10 57                BPL L0A17
09C0   08                   PHP
09C1   01 00                ORA ($00,X)
09C3   00                   BRK
09C4   00                   BRK
09C5   00                   BRK
09C6   00                   BRK
09C7   00                   BRK
09C8   00                   BRK
09C9   00                   BRK
09CA   00                   BRK
09CB   00                   BRK
09CC   00                   BRK
09CD   00                   BRK
09CE   00                   BRK
09CF   00                   BRK
09D0   4C 4C 08             JMP L084C
09D3   4C 47 08             JMP L0847
09D6   4C 42 08             JMP L0842
09D9   4C 3D 08             JMP L083D
09DC   4C 38 08   L09DC     JMP L0838
09DF   60                   RTS
09E0   20 7B DD             JSR $DD7B
09E3   20 FB E6             JSR $E6FB
09E6   8A                   TXA
09E7   48                   PHA
09E8   20 BE DE             JSR $DEBE
09EB   20 7B DD             JSR $DD7B
09EE   20 FB E6             JSR $E6FB
09F1   68                   PLA
09F2   20 5C DB   L09F2     JSR $DB5C
09F5   CA                   DEX
09F6   D0 FA                BNE L09F2
09F8   60                   RTS
09F9   00                   BRK
09FA   00                   BRK
09FB   00                   BRK
09FC   02                   ???                ;%00000010
09FD   02                   ???                ;%00000010
09FE   00                   BRK
09FF   32                   ???                ;%00110010 '2'
0A00   A0 18                LDY #$18
0A02   B1 69                LDA ($69),Y
0A04   8D FE 09             STA $09FE
0A07   A0 09                LDY #$09
0A09   B1 69                LDA ($69),Y
0A0B   8D FD 09             STA $09FD
0A0E   38                   SEC
0A0F   C8                   INY
0A10   B1 69                LDA ($69),Y
0A12   E9 01                SBC #$01
0A14   85 83                STA $83
0A16   C8                   INY
0A17   B1 69      L0A17     LDA ($69),Y
0A19   E9 00                SBC #$00
0A1B   85 84                STA $84
0A1D   A4 24      L0A1D     LDY $24
0A1F   20 0C FD             JSR $FD0C
0A22   C9 8D                CMP #$8D
0A24   D0 0D                BNE L0A33
0A26   2C FE 09             BIT $09FE
0A29   F0 17                BEQ L0A42
0A2B   A0 00                LDY #$00
0A2D   8C FC 09             STY $09FC
0A30   4C 6B 0A             JMP L0A6B
0A33   29 7F      L0A33     AND #$7F
0A35   8D FF 09             STA $09FF
0A38   AC FD 09             LDY $09FD
0A3B   D1 83      L0A3B     CMP ($83),Y
0A3D   F0 09                BEQ L0A48
0A3F   88                   DEY
0A40   D0 F9                BNE L0A3B
0A42   20 DC 09   L0A42     JSR L09DC
0A45   4C 1D 0A             JMP L0A1D
0A48   8C FC 09   L0A48     STY $09FC
0A4B   20 5C DB             JSR $DB5C
0A4E   A4 24                LDY $24
0A50   20 0C FD             JSR $FD0C
0A53   C9 8D                CMP #$8D
0A55   F0 14                BEQ L0A6B
0A57   48                   PHA
0A58   A9 88                LDA #$88
0A5A   20 5C DB             JSR $DB5C
0A5D   A9 A0                LDA #$A0
0A5F   20 5C DB             JSR $DB5C
0A62   A9 88                LDA #$88
0A64   20 5C DB             JSR $DB5C
0A67   68                   PLA
0A68   4C 33 0A             JMP L0A33
0A6B   AD FC 09   L0A6B     LDA $09FC
0A6E   A0 11                LDY #$11
0A70   91 69                STA ($69),Y
0A72   88                   DEY
0A73   A9 00                LDA #$00
0A75   91 69                STA ($69),Y
0A77   60                   RTS
0A78   A0 02                LDY #$02
0A7A   B1 69                LDA ($69),Y
0A7C   8D FD 09             STA $09FD
0A7F   38                   SEC
0A80   C8                   INY
0A81   B1 69                LDA ($69),Y
0A83   E9 01                SBC #$01
0A85   85 83                STA $83
0A87   C8                   INY
0A88   B1 69                LDA ($69),Y
0A8A   E9 00                SBC #$00
0A8C   85 84                STA $84
0A8E   A9 01                LDA #$01
0A90   8D FC 09             STA $09FC
0A93   A4 24      L0A93     LDY $24
0A95   20 0C FD             JSR $FD0C
0A98   29 7F                AND #$7F
0A9A   8D FF 09             STA $09FF
0A9D   C9 0D                CMP #$0D
0A9F   F0 5C                BEQ L0AFD
0AA1   AD FC 09             LDA $09FC
0AA4   C9 01                CMP #$01
0AA6   D0 0A                BNE L0AB2
0AA8   AD FF 09             LDA $09FF
0AAB   C9 41                CMP #$41
0AAD   F0 38                BEQ L0AE7
0AAF   4C D5 0A             JMP L0AD5
0AB2   AD FF 09   L0AB2     LDA $09FF
0AB5   C9 08                CMP #$08
0AB7   D0 1C                BNE L0AD5
0AB9   AC FC 09             LDY $09FC
0ABC   A9 20                LDA #$20
0ABE   91 83                STA ($83),Y
0AC0   A9 88                LDA #$88
0AC2   20 5C DB             JSR $DB5C
0AC5   A9 A0                LDA #$A0
0AC7   20 5C DB             JSR $DB5C
0ACA   A9 88                LDA #$88
0ACC   20 5C DB             JSR $DB5C
0ACF   CE FC 09             DEC $09FC
0AD2   4C 93 0A             JMP L0A93
0AD5   AD FC 09   L0AD5     LDA $09FC
0AD8   C9 0A                CMP #$0A
0ADA   B0 1B                BCS L0AF7
0ADC   AD FF 09             LDA $09FF
0ADF   C9 30                CMP #$30
0AE1   90 14                BCC L0AF7
0AE3   C9 3A                CMP #$3A
0AE5   B0 10                BCS L0AF7
0AE7   AC FC 09   L0AE7     LDY $09FC
0AEA   91 83                STA ($83),Y
0AEC   EE FC 09             INC $09FC
0AEF   09 80                ORA #$80
0AF1   20 5C DB             JSR $DB5C
0AF4   4C 93 0A             JMP L0A93
0AF7   20 DC 09   L0AF7     JSR L09DC
0AFA   4C 93 0A             JMP L0A93
0AFD   AC FC 09   L0AFD     LDY $09FC
0B00   C0 0A      L0B00     CPY #$0A
0B02   F0 08                BEQ L0B0C
0B04   A9 20                LDA #$20
0B06   91 83                STA ($83),Y
0B08   C8                   INY
0B09   4C 00 0B             JMP L0B00
0B0C   60         L0B0C     RTS
0B0D   00                   BRK
0B0E   00                   BRK
0B0F   00                   BRK
0B10   20 0C E1             JSR $E10C
0B13   A5 A1                LDA $A1
0B15   0A                   ASL A
0B16   AA                   TAX
0B17   BD 3D 0B             LDA $0B3D,X
0B1A   8D 26 0B             STA $0B26
0B1D   BD 3E 0B             LDA $0B3E,X
0B20   8D 27 0B             STA $0B27
0B23   A2 00                LDX #$00
0B25   BD 83 0B   L0B25     LDA $0B83,X
0B28   F0 08                BEQ L0B32
0B2A   A4 24                LDY $24
0B2C   20 5C DB             JSR $DB5C
0B2F   E8                   INX
0B30   D0 F3                BNE L0B25
0B32   A0 E9      L0B32     LDY #$E9
0B34   A9 13                LDA #$13
0B36   20 F9 EA             JSR $EAF9
0B39   20 AE EF             JSR $EFAE
0B3C   60                   RTS
0B3D   83                   ???                ;%10000011
0B3E   0B                   ???                ;%00001011
0B3F   84 0B                STY $0B
0B41   8E 0B 99             STX $990B
0B44   0B                   ???                ;%00001011
0B45   A2 0B                LDX #$0B
0B47   B5 0B                LDA $0B,X
0B49   C8                   INY
0B4A   0B                   ???                ;%00001011
0B4B   E4 0B                CPX $0B
0B4D   01 0C                ORA ($0C,X)
0B4F   12                   ???                ;%00010010
0B50   0C                   ???                ;%00001100
0B51   1F                   ???                ;%00011111
0B52   0C                   ???                ;%00001100
0B53   2D 0C 74             AND $740C
0B56   0C                   ???                ;%00001100
0B57   94 0C                STY $0C,X
0B59   D5 0C                CMP $0C,X
0B5B   59 0D C9             EOR $C90D,Y
0B5E   0D 54 0E             ORA $0E54
0B61   6C 0E 8B             JMP ($8B0E)
0B64   0E C9 0E             ASL $0EC9
0B67   43                   ???                ;%01000011 'C'
0B68   0F                   ???                ;%00001111
0B69   D5 0F                CMP $0F,X
0B6B   13                   ???                ;%00010011
0B6C   10 37                BPL L0BA5
0B6E   10 58                BPL L0BC8
0B70   10 64                BPL L0BD6
0B72   10 B0                BPL L0B24
0B74   10 DF                BPL L0B55
0B76   10 46                BPL L0BBE
0B78   11 69                ORA ($69),Y
0B7A   11 8E                ORA ($8E),Y
0B7C   11 B6                ORA ($B6),Y
0B7E   11 D0                ORA ($D0),Y
0B80   11 F2                ORA ($F2),Y
0B82   11 00                ORA ($00),Y
0B84   C8                   INY
0B85   EF                   ???                ;%11101111
0B86   EE E7 A0             INC $A0E7
0B89   CB                   ???                ;%11001011
0B8A   EF                   ???                ;%11101111
0B8B   EE E7 00             INC $00E7
0B8E   A0 D7                LDY #$D7
0B90   E1 F2                SBC ($F2,X)
0B92   E5 E8                SBC $E8
0B94   EF                   ???                ;%11101111
0B95   F5 F3                SBC $F3,X
0B97   E5 00                SBC $00
0B99   CC EF E3             CPY $E3EF
0B9C   E1 F4                SBC ($F4,X)
0B9E   E9 EF                SBC #$EF
0BA0   EE 00 C3             INC $C300
0BA3   EF                   ???                ;%11101111
0BA4   ED F0 F2             SBC $F2F0
0BA7   E1 E4                SBC ($E4,X)
0BA9   EF                   ???                ;%11101111
0BAA   F2                   ???                ;%11110010
0BAB   A7                   ???                ;%10100111
0BAC   F3                   ???                ;%11110011
0BAD   A0 D2                LDY #$D2
0BAF   E5 F0                SBC $F0
0BB1   EF                   ???                ;%11101111
0BB2   F2                   ???                ;%11110010
0BB3   F4                   ???                ;%11110100
0BB4   00                   BRK
0BB5   A0 A0                LDY #$A0
0BB7   C3                   ???                ;%11000011
0BB8   E1 F0                SBC ($F0,X)
0BBA   F4                   ???                ;%11110100
0BBB   E1 E9                SBC ($E9,X)
0BBD   EE A7 F3             INC $F3A7
0BC0   A0 D2                LDY #$D2
0BC2   E5 F0                SBC $F0
0BC4   EF                   ???                ;%11101111
0BC5   F2                   ???                ;%11110010
0BC6   F4                   ???                ;%11110100
0BC7   00                   BRK
0BC8   C8         L0BC8     INY
0BC9   EF                   ???                ;%11101111
0BCA   F7                   ???                ;%11110111
0BCB   A0 ED                LDY #$ED
0BCD   F5 E3      L0BCD     SBC $E3,X
0BCF   E8                   INX
0BD0   A0 F7                LDY #$F7
0BD2   E9 EC                SBC #$EC
0BD4   EC A0 F9             CPX $F9A0
0BD7   EF                   ???                ;%11101111
0BD8   F5 A0                SBC $A0,X
0BDA   E4 E5                CPX $E5
0BDC   F0 EF                BEQ L0BCD
0BDE   F3                   ???                ;%11110011
0BDF   E9 F4                SBC #$F4
0BE1   BF                   ???                ;%10111111
0BE2   A0 00                LDY #$00
0BE4   C8                   INY
0BE5   EF                   ???                ;%11101111
0BE6   F7                   ???                ;%11110111
0BE7   A0 ED                LDY #$ED
0BE9   F5 E3                SBC $E3,X
0BEB   E8                   INX
0BEC   A0 F7                LDY #$F7
0BEE   E9 EC                SBC #$EC
0BF0   EC A0 F9             CPX $F9A0
0BF3   EF                   ???                ;%11101111
0BF4   F5 A0                SBC $A0,X
0BF6   F7                   ???                ;%11110111
0BF7   E9 F4                SBC #$F4
0BF9   E8                   INX
0BFA   E4 F2                CPX $F2
0BFC   E1 F7                SBC ($F7,X)
0BFE   BF                   ???                ;%10111111
0BFF   A0 00                LDY #$00
                            .END

;auto-generated symbols and labels
 L0851      $0851
 L086F      $086F
 L085F      $085F
 L0862      $0862
 L0910      $0910
 L08E8      $08E8
 L08C6      $08C6
 L08C4      $08C4
 L0993      $0993
 L0994      $0994
 L096A      $096A
 L0956      $0956
 L0954      $0954
 L0A17      $0A17
 L084C      $084C
 L0847      $0847
 L0842      $0842
 L083D      $083D
 L0838      $0838
 L09F2      $09F2
 L0A33      $0A33
 L0A42      $0A42
 L0A6B      $0A6B
 L0A48      $0A48
 L0A3B      $0A3B
 L09DC      $09DC
 L0A1D      $0A1D
 L0AFD      $0AFD
 L0AB2      $0AB2
 L0AE7      $0AE7
 L0AD5      $0AD5
 L0A93      $0A93
 L0AF7      $0AF7
 L0B0C      $0B0C
 L0B00      $0B00
 L0B32      $0B32
 L0B25      $0B25
 L0BA5      $0BA5
 L0BC8      $0BC8
 L0BD6      $0BD6
 L0B24      $0B24
 L0B55      $0B55
 L0BBE      $0BBE
 L0BCD      $0BCD
```

#### Assembly code (with `$093F` as `00`) with labels

```none
 OUTDO    = $DB5C
 FRMEVL   = $DD7B
 WAIT     = $FCA8
 CONINT   = $E6FB
 CHKCOM   = $DEBE
 AYINT    = $E10C
 MOVFM    = $EAF9
 RND      = $EFAE
 RDKEY    = $FD0C
;BEEP1    = $0838
;BEEP2    = $083D
;BEEP3    = $0842
;BEEP4A   = $0847
;BEEP4B   = $084C
;GENINP   = $0898
;SHPSNK   = $08B0
;SHKSNK2  = $0910
;SHPFSH   = $0940
;SOUND4   = $09D0
;SPUND3   = $09D6
;SOUND2   = $09D9
;SOUND1   = $09DC
;UNKNWN2  = $09E0
;SELINP   = $0A00
;NUMINP   = $0A78
;SHPMATH  = $0B10
;SNDRTN   = $086F
;GENRTN   = $08AC
;SHPSNKL1 = $08C4
;SHPSNKL2 = $08C6
;SHPSNKL3 = $08E8
;SHPFSHL1 = $0954
;SHPFSHL2 = $0956
;SHPFSHL3 = $096A
;SHPFSHED = $0994
;UNKNWNLP = $09F2
;SELIP1   = $0A1D
;SELIP2   = $0A33
;SELIP3   = $0A3B
;SELIP4   = $0A42
;SELIP5   = $0A48
;SELIP6   = $0A6B
;NUMIP1   = $0A93
;NUMIP2   = $0AB2
;NUMIP3   = $0AD5
;NUMIP4   = $0AE7
;NUMIP5   = $0AF7
;NUMIP6   = $0AFD
;NUMIP7   = $0B00
;NUMRTN   = $0B0C
;BEEPDO   = $0862
;BEEPJMP1 = $0851
;BEEPJMP2 = $085F
;SHPMATH1 = $0B25
;SHPMATH2 = $0B32

                            * = $0800
0800   00                   BRK
0801   21 08                AND ($08,X)
0803   0A                   ASL A
0804   00                   BRK
0805   B9 31 30             LDA $3031,Y
0808   33                   ???                ;%00110011 '3'
0809   2C 31 3A             BIT $3A31
080C   B9 31 30             LDA $3031,Y
080F   34                   ???                ;%00110100 '4'
0810   2C 36 34             BIT $3436
0813   3A                   ???                ;%00111010 ':'
0814   B9 31 36             LDA $3631,Y
0817   33                   ???                ;%00110011 '3'
0818   38                   SEC
0819   34                   ???                ;%00110100 '4'
081A   2C 30 3A             BIT $3A30
081D   AB                   ???                ;%10101011
081E   31 30                AND ($30),Y
0820   00                   BRK
0821   00                   BRK
0822   00                   BRK
0823   00                   BRK
0824   00                   BRK
0825   00                   BRK
0826   00                   BRK
0827   00                   BRK
0828   00                   BRK
0829   00                   BRK
082A   00                   BRK
082B   00                   BRK
082C   00                   BRK
082D   00                   BRK
082E   00                   BRK
082F   00                   BRK
0830   00                   BRK
0831   00                   BRK
0832   00                   BRK
0833   00                   BRK
0834   00                   BRK
0835   00                   BRK
0836   00                   BRK
0837   00                   BRK
0838   A2 01      BEEP1     LDX #$01
083A   4C 51 08             JMP BEEPJMP1
083D   A2 03      BEEP2     LDX #$03
083F   4C 51 08             JMP BEEPJMP1
0842   A2 09      BEEP3     LDX #$09
0844   4C 51 08             JMP BEEPJMP1
0847   A2 19      BEEP4A    LDX #$19
0849   4C 51 08             JMP BEEPJMP1
084C   A2 1B      BEEP4B    LDX #$1B
084E   4C 51 08             JMP BEEPJMP1
0851   BD 70 08   BEEPJMP1  LDA $0870,X
0854   F0 19                BEQ SNDRTN
0856   20 5F 08             JSR BEEPJMP2
0859   E8                   INX
085A   D0 F5                BNE BEEPJMP1
085C   4C 6F 08             JMP SNDRTN
085F   48         BEEPJMP2  PHA
0860   A0 C0                LDY #$C0
0862   68         BEEPDO    PLA
0863   48                   PHA
0864   20 A8 FC             JSR WAIT
0867   AD 30 C0             LDA $C030
086A   88                   DEY
086B   D0 F5                BNE BEEPDO
086D   68                   PLA
086E   60                   RTS
086F   60         SNDRTN    RTS
0870   FF                   ???                ;%11111111
0871   0A                   ASL A
0872   00                   BRK
0873   06 09                ASL $09
0875   08                   PHP
0876   07                   ???                ;%00000111
0877   06 00                ASL $00
0879   08                   PHP
087A   06 05                ASL $05
087C   04                   ???                ;%00000100
087D   03                   ???                ;%00000011
087E   08                   PHP
087F   06 05                ASL $05
0881   04                   ???                ;%00000100
0882   03                   ???                ;%00000011
0883   08                   PHP
0884   06 05                ASL $05
0886   04                   ???                ;%00000100
0887   03                   ???                ;%00000011
0888   00                   BRK
0889   12                   ???                ;%00010010
088A   00                   BRK
088B   07                   ???                ;%00000111
088C   09 08                ORA #$08
088E   09 07                ORA #$07
0890   07                   ???                ;%00000111
0891   09 08                ORA #$08
0893   09 07                ORA #$07
0895   00                   BRK
0896   00                   BRK
0897   00                   BRK
0898   A2 00      GENINP    LDX #$00
089A   20 75 FD             JSR $FD75
089D   A0 02                LDY #$02
089F   8A                   TXA
08A0   91 69                STA ($69),Y
08A2   C8                   INY
08A3   A9 00                LDA #$00
08A5   91 69                STA ($69),Y
08A7   C8                   INY
08A8   A9 02                LDA #$02
08AA   91 69                STA ($69),Y
08AC   60         GENRTN    RTS
08AD   00                   BRK
08AE   00                   BRK
08AF   00                   BRK
08B0   48         SHPSNK    PHA
08B1   8A                   TXA
08B2   48                   PHA
08B3   98                   TYA
08B4   48                   PHA
08B5   A9 20                LDA #$20
08B7   85 E6                STA $E6
08B9   A9 00                LDA #$00
08BB   8D 3C 09             STA $093C
08BE   AD 39 09             LDA $0939
08C1   8D 3B 09             STA $093B
08C4   A2 00      SHPSNKL1  LDX #$00
08C6   AD 39 09   SHPSNKL2  LDA $0939
08C9   20 10 09             JSR SHKSNK2
08CC   A5 26                LDA $26
08CE   85 3E                STA $3E
08D0   A5 27                LDA $27
08D2   85 3F                STA $3F
08D4   CE 39 09             DEC $0939
08D7   E8                   INX
08D8   AD 39 09             LDA $0939
08DB   20 10 09             JSR SHKSNK2
08DE   A5 26                LDA $26
08E0   85 3C                STA $3C
08E2   A5 27                LDA $27
08E4   85 3D                STA $3D
08E6   A0 00                LDY #$00
08E8   B1 3C      SHPSNKL3  LDA ($3C),Y
08EA   91 3E                STA ($3E),Y
08EC   C8                   INY
08ED   C0 07                CPY #$07
08EF   D0 F7                BNE SHPSNKL3
08F1   E0 28                CPX #$28
08F3   D0 D1                BNE SHPSNKL2
08F5   AD 3B 09             LDA $093B
08F8   8D 39 09             STA $0939
08FB   A9 56                LDA #$56
08FD   20 A8 FC             JSR WAIT
0900   EE 3C 09             INC $093C
0903   AD 3C 09             LDA $093C
0906   C9 28                CMP #$28
0908   D0 BA                BNE SHPSNKL1
090A   68                   PLA
090B   A8                   TAY
090C   68                   PLA
090D   AA                   TAX
090E   68                   PLA
090F   60                   RTS
0910   48         SHKSNK2   PHA
0911   29 C0                AND #$C0
0913   85 26                STA $26
0915   4A                   LSR A
0916   4A                   LSR A
0917   05 26                ORA $26
0919   85 26                STA $26
091B   68                   PLA
091C   85 27                STA $27
091E   0A                   ASL A
091F   0A                   ASL A
0920   0A                   ASL A
0921   26 27                ROL $27
0923   0A                   ASL A
0924   26 27                ROL $27
0926   0A                   ASL A
0927   66 26                ROR $26
0929   A5 27                LDA $27
092B   29 1F                AND #$1F
092D   05 E6                ORA $E6
092F   85 27                STA $27
0931   A5 26                LDA $26
0933   6D 3A 09             ADC $093A
0936   85 26                STA $26
0938   60                   RTS
0939   57                   ???                ;%01010111 'W'
093A   10 57                BPL L0993
093C   28                   PLP
093D   01 00                ORA ($00,X)
093F   00                   BRK
0940   48         SHPFSH    PHA
0941   8A                   TXA
0942   48                   PHA
0943   98                   TYA
0944   48                   PHA
0945   A9 20                LDA #$20
0947   85 E6                STA $E6
0949   A9 00                LDA #$00
094B   8D C0 09             STA $09C0
094E   AD BD 09             LDA $09BD
0951   8D BF 09             STA $09BF
0954   A2 00      SHPFSHL1  LDX #$00
0956   AD BD 09   SHPFSHL2  LDA $09BD
0959   20 94 09             JSR SHPFSHED
095C   A5 26                LDA $26
095E   85 3C                STA $3C
0960   A5 27                LDA $27
0962   85 3D                STA $3D
0964   E8                   INX
0965   CE BD 09             DEC $09BD
0968   A0 00                LDY #$00
096A   B1 3C      SHPFSHL3  LDA ($3C),Y
096C   49 FF                EOR #$FF
096E   91 3C                STA ($3C),Y
0970   C8                   INY
0971   C0 07                CPY #$07
0973   D0 F5                BNE SHPFSHL3
0975   E0 2A                CPX #$2A
0977   D0 DD                BNE SHPFSHL2
0979   A9 50                LDA #$50
097B   20 A8 FC             JSR WAIT
097E   AD BF 09             LDA $09BF
0981   8D BD 09             STA $09BD
0984   EE C0 09             INC $09C0
0987   AD C0 09             LDA $09C0
098A   C9 08                CMP #$08
098C   D0 C6                BNE SHPFSHL1
098E   68                   PLA
098F   A8                   TAY
0990   68                   PLA
0991   AA                   TAX
0992   68                   PLA
0993   60         L0993     RTS
0994   48         SHPFSHED  PHA
0995   29 C0                AND #$C0
0997   85 26                STA $26
0999   4A                   LSR A
099A   4A                   LSR A
099B   05 26                ORA $26
099D   85 26                STA $26
099F   68                   PLA
09A0   85 27                STA $27
09A2   0A                   ASL A
09A3   0A                   ASL A
09A4   0A                   ASL A
09A5   26 27                ROL $27
09A7   0A                   ASL A
09A8   26 27                ROL $27
09AA   0A                   ASL A
09AB   66 26                ROR $26
09AD   A5 27                LDA $27
09AF   29 1F                AND #$1F
09B1   05 E6                ORA $E6
09B3   85 27                STA $27
09B5   A5 26                LDA $26
09B7   6D BE 09             ADC $09BE
09BA   85 26                STA $26
09BC   60                   RTS
09BD   57                   ???                ;%01010111 'W'
09BE   10 57                BPL L0A17
09C0   08                   PHP
09C1   01 00                ORA ($00,X)
09C3   00                   BRK
09C4   00                   BRK
09C5   00                   BRK
09C6   00                   BRK
09C7   00                   BRK
09C8   00                   BRK
09C9   00                   BRK
09CA   00                   BRK
09CB   00                   BRK
09CC   00                   BRK
09CD   00                   BRK
09CE   00                   BRK
09CF   00                   BRK
09D0   4C 4C 08   SOUND4    JMP BEEP4B
09D3   4C 47 08             JMP BEEP4A
09D6   4C 42 08   SPUND3    JMP BEEP3
09D9   4C 3D 08   SOUND2    JMP BEEP2
09DC   4C 38 08   SOUND1    JMP BEEP1
09DF   60                   RTS
09E0   20 7B DD   UNKNWN2   JSR FRMEVL
09E3   20 FB E6             JSR CONINT
09E6   8A                   TXA
09E7   48                   PHA
09E8   20 BE DE             JSR CHKCOM
09EB   20 7B DD             JSR FRMEVL
09EE   20 FB E6             JSR CONINT
09F1   68                   PLA
09F2   20 5C DB   UNKNWNLP  JSR OUTDO
09F5   CA                   DEX
09F6   D0 FA                BNE UNKNWNLP
09F8   60                   RTS
09F9   00                   BRK
09FA   00                   BRK
09FB   00                   BRK
09FC   02                   ???                ;%00000010
09FD   02                   ???                ;%00000010
09FE   00                   BRK
09FF   32                   ???                ;%00110010 '2'
0A00   A0 18      SELINP    LDY #$18
0A02   B1 69                LDA ($69),Y
0A04   8D FE 09             STA $09FE
0A07   A0 09                LDY #$09
0A09   B1 69                LDA ($69),Y
0A0B   8D FD 09             STA $09FD
0A0E   38                   SEC
0A0F   C8                   INY
0A10   B1 69                LDA ($69),Y
0A12   E9 01                SBC #$01
0A14   85 83                STA $83
0A16   C8                   INY
0A17   B1 69      L0A17     LDA ($69),Y
0A19   E9 00                SBC #$00
0A1B   85 84                STA $84
0A1D   A4 24      SELIP1    LDY $24
0A1F   20 0C FD             JSR RDKEY
0A22   C9 8D                CMP #$8D
0A24   D0 0D                BNE SELIP2
0A26   2C FE 09             BIT $09FE
0A29   F0 17                BEQ SELIP4
0A2B   A0 00                LDY #$00
0A2D   8C FC 09             STY $09FC
0A30   4C 6B 0A             JMP SELIP6
0A33   29 7F      SELIP2    AND #$7F
0A35   8D FF 09             STA $09FF
0A38   AC FD 09             LDY $09FD
0A3B   D1 83      SELIP3    CMP ($83),Y
0A3D   F0 09                BEQ SELIP5
0A3F   88                   DEY
0A40   D0 F9                BNE SELIP3
0A42   20 DC 09   SELIP4    JSR SOUND1
0A45   4C 1D 0A             JMP SELIP1
0A48   8C FC 09   SELIP5    STY $09FC
0A4B   20 5C DB             JSR OUTDO
0A4E   A4 24                LDY $24
0A50   20 0C FD             JSR RDKEY
0A53   C9 8D                CMP #$8D
0A55   F0 14                BEQ SELIP6
0A57   48                   PHA
0A58   A9 88                LDA #$88
0A5A   20 5C DB             JSR OUTDO
0A5D   A9 A0                LDA #$A0
0A5F   20 5C DB             JSR OUTDO
0A62   A9 88                LDA #$88
0A64   20 5C DB             JSR OUTDO
0A67   68                   PLA
0A68   4C 33 0A             JMP SELIP2
0A6B   AD FC 09   SELIP6    LDA $09FC
0A6E   A0 11                LDY #$11
0A70   91 69                STA ($69),Y
0A72   88                   DEY
0A73   A9 00                LDA #$00
0A75   91 69                STA ($69),Y
0A77   60                   RTS
0A78   A0 02      NUMINP    LDY #$02
0A7A   B1 69                LDA ($69),Y
0A7C   8D FD 09             STA $09FD
0A7F   38                   SEC
0A80   C8                   INY
0A81   B1 69                LDA ($69),Y
0A83   E9 01                SBC #$01
0A85   85 83                STA $83
0A87   C8                   INY
0A88   B1 69                LDA ($69),Y
0A8A   E9 00                SBC #$00
0A8C   85 84                STA $84
0A8E   A9 01                LDA #$01
0A90   8D FC 09             STA $09FC
0A93   A4 24      NUMIP1    LDY $24
0A95   20 0C FD             JSR RDKEY
0A98   29 7F                AND #$7F
0A9A   8D FF 09             STA $09FF
0A9D   C9 0D                CMP #$0D
0A9F   F0 5C                BEQ NUMIP6
0AA1   AD FC 09             LDA $09FC
0AA4   C9 01                CMP #$01
0AA6   D0 0A                BNE NUMIP2
0AA8   AD FF 09             LDA $09FF
0AAB   C9 41                CMP #$41
0AAD   F0 38                BEQ NUMIP4
0AAF   4C D5 0A             JMP NUMIP3
0AB2   AD FF 09   NUMIP2    LDA $09FF
0AB5   C9 08                CMP #$08
0AB7   D0 1C                BNE NUMIP3
0AB9   AC FC 09             LDY $09FC
0ABC   A9 20                LDA #$20
0ABE   91 83                STA ($83),Y
0AC0   A9 88                LDA #$88
0AC2   20 5C DB             JSR OUTDO
0AC5   A9 A0                LDA #$A0
0AC7   20 5C DB             JSR OUTDO
0ACA   A9 88                LDA #$88
0ACC   20 5C DB             JSR OUTDO
0ACF   CE FC 09             DEC $09FC
0AD2   4C 93 0A             JMP NUMIP1
0AD5   AD FC 09   NUMIP3    LDA $09FC
0AD8   C9 0A                CMP #$0A
0ADA   B0 1B                BCS NUMIP5
0ADC   AD FF 09             LDA $09FF
0ADF   C9 30                CMP #$30
0AE1   90 14                BCC NUMIP5
0AE3   C9 3A                CMP #$3A
0AE5   B0 10                BCS NUMIP5
0AE7   AC FC 09   NUMIP4    LDY $09FC
0AEA   91 83                STA ($83),Y
0AEC   EE FC 09             INC $09FC
0AEF   09 80                ORA #$80
0AF1   20 5C DB             JSR OUTDO
0AF4   4C 93 0A             JMP NUMIP1
0AF7   20 DC 09   NUMIP5    JSR SOUND1
0AFA   4C 93 0A             JMP NUMIP1
0AFD   AC FC 09   NUMIP6    LDY $09FC
0B00   C0 0A      NUMIP7    CPY #$0A
0B02   F0 08                BEQ NUMRTN
0B04   A9 20                LDA #$20
0B06   91 83                STA ($83),Y
0B08   C8                   INY
0B09   4C 00 0B             JMP NUMIP7
0B0C   60         NUMRTN    RTS
0B0D   00                   BRK
0B0E   00                   BRK
0B0F   00                   BRK
0B10   20 0C E1   SHPMATH   JSR AYINT
0B13   A5 A1                LDA $A1
0B15   0A                   ASL A
0B16   AA                   TAX
0B17   BD 3D 0B             LDA $0B3D,X
0B1A   8D 26 0B             STA $0B26
0B1D   BD 3E 0B             LDA $0B3E,X
0B20   8D 27 0B             STA $0B27
0B23   A2 00                LDX #$00
0B25   BD 83 0B   SHPMATH1  LDA $0B83,X
0B28   F0 08                BEQ SHPMATH2
0B2A   A4 24                LDY $24
0B2C   20 5C DB             JSR OUTDO
0B2F   E8                   INX
0B30   D0 F3                BNE SHPMATH1
0B32   A0 E9      SHPMATH2  LDY #$E9
0B34   A9 13                LDA #$13
0B36   20 F9 EA             JSR MOVFM
0B39   20 AE EF             JSR RND
0B3C   60                   RTS
0B3D   83                   ???                ;%10000011
0B3E   0B                   ???                ;%00001011
0B3F   84 0B                STY $0B
0B41   8E 0B 99             STX $990B
0B44   0B                   ???                ;%00001011
0B45   A2 0B                LDX #$0B
0B47   B5 0B                LDA $0B,X
0B49   C8                   INY
0B4A   0B                   ???                ;%00001011
0B4B   E4 0B                CPX $0B
0B4D   01 0C                ORA ($0C,X)
0B4F   12                   ???                ;%00010010
0B50   0C                   ???                ;%00001100
0B51   1F                   ???                ;%00011111
0B52   0C                   ???                ;%00001100
0B53   2D 0C 74             AND $740C
0B56   0C                   ???                ;%00001100
0B57   94 0C                STY $0C,X
0B59   D5 0C                CMP $0C,X
0B5B   59 0D C9             EOR $C90D,Y
0B5E   0D 54 0E             ORA $0E54
0B61   6C 0E 8B             JMP ($8B0E)
0B64   0E C9 0E             ASL $0EC9
0B67   43                   ???                ;%01000011 'C'
0B68   0F                   ???                ;%00001111
0B69   D5 0F                CMP $0F,X
0B6B   13                   ???                ;%00010011
0B6C   10 37                BPL L0BA5
0B6E   10 58                BPL L0BC8
0B70   10 64                BPL L0BD6
0B72   10 B0                BPL L0B24
0B74   10 DF                BPL L0B55
0B76   10 46                BPL L0BBE
0B78   11 69                ORA ($69),Y
0B7A   11 8E                ORA ($8E),Y
0B7C   11 B6                ORA ($B6),Y
0B7E   11 D0                ORA ($D0),Y
0B80   11 F2                ORA ($F2),Y
0B82   11 00                ORA ($00),Y
0B84   C8                   INY
0B85   EF                   ???                ;%11101111
0B86   EE E7 A0             INC $A0E7
0B89   CB                   ???                ;%11001011
0B8A   EF                   ???                ;%11101111
0B8B   EE E7 00             INC $00E7
0B8E   A0 D7                LDY #$D7
0B90   E1 F2                SBC ($F2,X)
0B92   E5 E8                SBC $E8
0B94   EF                   ???                ;%11101111
0B95   F5 F3                SBC $F3,X
0B97   E5 00                SBC $00
0B99   CC EF E3             CPY $E3EF
0B9C   E1 F4                SBC ($F4,X)
0B9E   E9 EF                SBC #$EF
0BA0   EE 00 C3             INC $C300
0BA3   EF                   ???                ;%11101111
0BA4   ED F0 F2             SBC $F2F0
0BA7   E1 E4                SBC ($E4,X)
0BA9   EF                   ???                ;%11101111
0BAA   F2                   ???                ;%11110010
0BAB   A7                   ???                ;%10100111
0BAC   F3                   ???                ;%11110011
0BAD   A0 D2                LDY #$D2
0BAF   E5 F0                SBC $F0
0BB1   EF                   ???                ;%11101111
0BB2   F2                   ???                ;%11110010
0BB3   F4                   ???                ;%11110100
0BB4   00                   BRK
0BB5   A0 A0                LDY #$A0
0BB7   C3                   ???                ;%11000011
0BB8   E1 F0                SBC ($F0,X)
0BBA   F4                   ???                ;%11110100
0BBB   E1 E9                SBC ($E9,X)
0BBD   EE A7 F3             INC $F3A7
0BC0   A0 D2                LDY #$D2
0BC2   E5 F0                SBC $F0
0BC4   EF                   ???                ;%11101111
0BC5   F2                   ???                ;%11110010
0BC6   F4                   ???                ;%11110100
0BC7   00                   BRK
0BC8   C8         L0BC8     INY
0BC9   EF                   ???                ;%11101111
0BCA   F7                   ???                ;%11110111
0BCB   A0 ED                LDY #$ED
0BCD   F5 E3      L0BCD     SBC $E3,X
0BCF   E8                   INX
0BD0   A0 F7                LDY #$F7
0BD2   E9 EC                SBC #$EC
0BD4   EC A0 F9             CPX $F9A0
0BD7   EF                   ???                ;%11101111
0BD8   F5 A0                SBC $A0,X
0BDA   E4 E5                CPX $E5
0BDC   F0 EF                BEQ L0BCD
0BDE   F3                   ???                ;%11110011
0BDF   E9 F4                SBC #$F4
0BE1   BF                   ???                ;%10111111
0BE2   A0 00                LDY #$00
0BE4   C8                   INY
0BE5   EF                   ???                ;%11101111
0BE6   F7                   ???                ;%11110111
0BE7   A0 ED                LDY #$ED
0BE9   F5 E3                SBC $E3,X
0BEB   E8                   INX
0BEC   A0 F7                LDY #$F7
0BEE   E9 EC                SBC #$EC
0BF0   EC A0 F9             CPX $F9A0
0BF3   EF                   ???                ;%11101111
0BF4   F5 A0                SBC $A0,X
0BF6   F7                   ???                ;%11110111
0BF7   E9 F4                SBC #$F4
0BF9   E8                   INX
0BFA   E4 F2                CPX $F2
0BFC   E1 F7                SBC ($F7,X)
0BFE   BF                   ???                ;%10111111
0BFF   A0 00                LDY #$00
                            .END

;auto-generated symbols and labels
 L0993      $0993
 L0A17      $0A17
 L0BA5      $0BA5
 L0BC8      $0BC8
 L0BD6      $0BD6
 L0B24      $0B24
 L0B55      $0B55
 L0BBE      $0BBE
 L0BCD      $0BCD

;defined symbols used as labels
 BEEPJMP1   $0851
 SNDRTN     $086F
 BEEPJMP2   $085F
 BEEPDO     $0862
 SHKSNK2    $0910
 SHPSNKL3   $08E8
 SHPSNKL2   $08C6
 SHPSNKL1   $08C4
 SHPFSHED   $0994
 SHPFSHL3   $096A
 SHPFSHL2   $0956
 SHPFSHL1   $0954
 BEEP4B     $084C
 BEEP4A     $0847
 BEEP3      $0842
 BEEP2      $083D
 BEEP1      $0838
 UNKNWNLP   $09F2
 SELIP2     $0A33
 SELIP4     $0A42
 SELIP6     $0A6B
 SELIP5     $0A48
 SELIP3     $0A3B
 SOUND1     $09DC
 SELIP1     $0A1D
 NUMIP6     $0AFD
 NUMIP2     $0AB2
 NUMIP4     $0AE7
 NUMIP3     $0AD5
 NUMIP1     $0A93
 NUMIP5     $0AF7
 NUMRTN     $0B0C
 NUMIP7     $0B00
 SHPMATH2   $0B32
 SHPMATH1   $0B25
 OUTDO      $DB5C
 FRMEVL     $DD7B
 WAIT       $FCA8
 CONINT     $E6FB
 CHKCOM     $DEBE
 AYINT      $E10C
 MOVFM      $EAF9
 RND        $EFAE
 RDKEY      $FD0C
 GENINP     $0898
 SHPSNK     $08B0
 SHPFSH     $0940
 SOUND4     $09D0
 SPUND3     $09D6
 SOUND2     $09D9
 UNKNWN2    $09E0
 SELINP     $0A00
 NUMINP     $0A78
 SHPMATH    $0B10
 GENRTN     $08AC
```

#### Assembly code (with `$093F` as `00`) with symbols - `$0800` to `$0B3C`

Tidier, less labelled jumps from junk bytes.

```none
 OUTDO    = $DB5C
 FRMEVL   = $DD7B
 WAIT     = $FCA8
 CONINT   = $E6FB
 CHKCOM   = $DEBE
 AYINT    = $E10C
 MOVFM    = $EAF9
 RND      = $EFAE
 RDKEY    = $FD0C
;BEEP1    = $0838
;BEEP2    = $083D
;BEEP3    = $0842
;BEEP4A   = $0847
;BEEP4B   = $084C
;GENINP   = $0898
;SHPSNK   = $08B0
;SHKSNK2  = $0910
;SHPFSH   = $0940
;SOUND4   = $09D0
;SPUND3   = $09D6
;SOUND2   = $09D9
;SOUND1   = $09DC
;UNKNWN2  = $09E0
;SELINP   = $0A00
;NUMINP   = $0A78
;SHPMATH  = $0B10
;SNDRTN   = $086F
;GENRTN   = $08AC
;SHPSNKL1 = $08C4
;SHPSNKL2 = $08C6
;SHPSNKL3 = $08E8
;SHPFSHL1 = $0954
;SHPFSHL2 = $0956
;SHPFSHL3 = $096A
;SHPFSHED = $0994
;UNKNWNLP = $09F2
;SELIP1   = $0A1D
;SELIP2   = $0A33
;SELIP3   = $0A3B
;SELIP4   = $0A42
;SELIP5   = $0A48
;SELIP6   = $0A6B
;NUMIP1   = $0A93
;NUMIP2   = $0AB2
;NUMIP3   = $0AD5
;NUMIP4   = $0AE7
;NUMIP5   = $0AF7
;NUMIP6   = $0AFD
;NUMIP7   = $0B00
;NUMRTN   = $0B0C
;BEEPDO   = $0862
;BEEPJMP1 = $0851
;BEEPJMP2 = $085F
;SHPMATH1 = $0B25
;SHPMATH2 = $0B32

                            * = $0800
0800   00                   BRK
0801   21 08                AND ($08,X)
0803   0A                   ASL A
0804   00                   BRK
0805   B9 31 30             LDA $3031,Y
0808   33                   ???                ;%00110011 '3'
0809   2C 31 3A             BIT $3A31
080C   B9 31 30             LDA $3031,Y
080F   34                   ???                ;%00110100 '4'
0810   2C 36 34             BIT $3436
0813   3A                   ???                ;%00111010 ':'
0814   B9 31 36             LDA $3631,Y
0817   33                   ???                ;%00110011 '3'
0818   38                   SEC
0819   34                   ???                ;%00110100 '4'
081A   2C 30 3A             BIT $3A30
081D   AB                   ???                ;%10101011
081E   31 30                AND ($30),Y
0820   00                   BRK
0821   00                   BRK
0822   00                   BRK
0823   00                   BRK
0824   00                   BRK
0825   00                   BRK
0826   00                   BRK
0827   00                   BRK
0828   00                   BRK
0829   00                   BRK
082A   00                   BRK
082B   00                   BRK
082C   00                   BRK
082D   00                   BRK
082E   00                   BRK
082F   00                   BRK
0830   00                   BRK
0831   00                   BRK
0832   00                   BRK
0833   00                   BRK
0834   00                   BRK
0835   00                   BRK
0836   00                   BRK
0837   00                   BRK
0838   A2 01      BEEP1     LDX #$01
083A   4C 51 08             JMP BEEPJMP1
083D   A2 03      BEEP2     LDX #$03
083F   4C 51 08             JMP BEEPJMP1
0842   A2 09      BEEP3     LDX #$09
0844   4C 51 08             JMP BEEPJMP1
0847   A2 19      BEEP4A    LDX #$19
0849   4C 51 08             JMP BEEPJMP1
084C   A2 1B      BEEP4B    LDX #$1B
084E   4C 51 08             JMP BEEPJMP1
0851   BD 70 08   BEEPJMP1  LDA $0870,X
0854   F0 19                BEQ SNDRTN
0856   20 5F 08             JSR BEEPJMP2
0859   E8                   INX
085A   D0 F5                BNE BEEPJMP1
085C   4C 6F 08             JMP SNDRTN
085F   48         BEEPJMP2  PHA
0860   A0 C0                LDY #$C0
0862   68         BEEPDO    PLA
0863   48                   PHA
0864   20 A8 FC             JSR WAIT
0867   AD 30 C0             LDA $C030
086A   88                   DEY
086B   D0 F5                BNE BEEPDO
086D   68                   PLA
086E   60                   RTS
086F   60         SNDRTN    RTS
0870   FF                   ???                ;%11111111
0871   0A                   ASL A
0872   00                   BRK
0873   06 09                ASL $09
0875   08                   PHP
0876   07                   ???                ;%00000111
0877   06 00                ASL $00
0879   08                   PHP
087A   06 05                ASL $05
087C   04                   ???                ;%00000100
087D   03                   ???                ;%00000011
087E   08                   PHP
087F   06 05                ASL $05
0881   04                   ???                ;%00000100
0882   03                   ???                ;%00000011
0883   08                   PHP
0884   06 05                ASL $05
0886   04                   ???                ;%00000100
0887   03                   ???                ;%00000011
0888   00                   BRK
0889   12                   ???                ;%00010010
088A   00                   BRK
088B   07                   ???                ;%00000111
088C   09 08                ORA #$08
088E   09 07                ORA #$07
0890   07                   ???                ;%00000111
0891   09 08                ORA #$08
0893   09 07                ORA #$07
0895   00                   BRK
0896   00                   BRK
0897   00                   BRK
0898   A2 00      GENINP    LDX #$00
089A   20 75 FD             JSR $FD75
089D   A0 02                LDY #$02
089F   8A                   TXA
08A0   91 69                STA ($69),Y
08A2   C8                   INY
08A3   A9 00                LDA #$00
08A5   91 69                STA ($69),Y
08A7   C8                   INY
08A8   A9 02                LDA #$02
08AA   91 69                STA ($69),Y
08AC   60         GENRTN    RTS
08AD   00                   BRK
08AE   00                   BRK
08AF   00                   BRK
08B0   48         SHPSNK    PHA
08B1   8A                   TXA
08B2   48                   PHA
08B3   98                   TYA
08B4   48                   PHA
08B5   A9 20                LDA #$20
08B7   85 E6                STA $E6
08B9   A9 00                LDA #$00
08BB   8D 3C 09             STA $093C
08BE   AD 39 09             LDA $0939
08C1   8D 3B 09             STA $093B
08C4   A2 00      SHPSNKL1  LDX #$00
08C6   AD 39 09   SHPSNKL2  LDA $0939
08C9   20 10 09             JSR SHKSNK2
08CC   A5 26                LDA $26
08CE   85 3E                STA $3E
08D0   A5 27                LDA $27
08D2   85 3F                STA $3F
08D4   CE 39 09             DEC $0939
08D7   E8                   INX
08D8   AD 39 09             LDA $0939
08DB   20 10 09             JSR SHKSNK2
08DE   A5 26                LDA $26
08E0   85 3C                STA $3C
08E2   A5 27                LDA $27
08E4   85 3D                STA $3D
08E6   A0 00                LDY #$00
08E8   B1 3C      SHPSNKL3  LDA ($3C),Y
08EA   91 3E                STA ($3E),Y
08EC   C8                   INY
08ED   C0 07                CPY #$07
08EF   D0 F7                BNE SHPSNKL3
08F1   E0 28                CPX #$28
08F3   D0 D1                BNE SHPSNKL2
08F5   AD 3B 09             LDA $093B
08F8   8D 39 09             STA $0939
08FB   A9 56                LDA #$56
08FD   20 A8 FC             JSR WAIT
0900   EE 3C 09             INC $093C
0903   AD 3C 09             LDA $093C
0906   C9 28                CMP #$28
0908   D0 BA                BNE SHPSNKL1
090A   68                   PLA
090B   A8                   TAY
090C   68                   PLA
090D   AA                   TAX
090E   68                   PLA
090F   60                   RTS
0910   48         SHKSNK2   PHA
0911   29 C0                AND #$C0
0913   85 26                STA $26
0915   4A                   LSR A
0916   4A                   LSR A
0917   05 26                ORA $26
0919   85 26                STA $26
091B   68                   PLA
091C   85 27                STA $27
091E   0A                   ASL A
091F   0A                   ASL A
0920   0A                   ASL A
0921   26 27                ROL $27
0923   0A                   ASL A
0924   26 27                ROL $27
0926   0A                   ASL A
0927   66 26                ROR $26
0929   A5 27                LDA $27
092B   29 1F                AND #$1F
092D   05 E6                ORA $E6
092F   85 27                STA $27
0931   A5 26                LDA $26
0933   6D 3A 09             ADC $093A
0936   85 26                STA $26
0938   60                   RTS
0939   57                   ???                ;%01010111 'W'
093A   10 57                BPL L0993
093C   28                   PLP
093D   01 00                ORA ($00,X)
093F   00                   BRK
0940   48         SHPFSH    PHA
0941   8A                   TXA
0942   48                   PHA
0943   98                   TYA
0944   48                   PHA
0945   A9 20                LDA #$20
0947   85 E6                STA $E6
0949   A9 00                LDA #$00
094B   8D C0 09             STA $09C0
094E   AD BD 09             LDA $09BD
0951   8D BF 09             STA $09BF
0954   A2 00      SHPFSHL1  LDX #$00
0956   AD BD 09   SHPFSHL2  LDA $09BD
0959   20 94 09             JSR SHPFSHED
095C   A5 26                LDA $26
095E   85 3C                STA $3C
0960   A5 27                LDA $27
0962   85 3D                STA $3D
0964   E8                   INX
0965   CE BD 09             DEC $09BD
0968   A0 00                LDY #$00
096A   B1 3C      SHPFSHL3  LDA ($3C),Y
096C   49 FF                EOR #$FF
096E   91 3C                STA ($3C),Y
0970   C8                   INY
0971   C0 07                CPY #$07
0973   D0 F5                BNE SHPFSHL3
0975   E0 2A                CPX #$2A
0977   D0 DD                BNE SHPFSHL2
0979   A9 50                LDA #$50
097B   20 A8 FC             JSR WAIT
097E   AD BF 09             LDA $09BF
0981   8D BD 09             STA $09BD
0984   EE C0 09             INC $09C0
0987   AD C0 09             LDA $09C0
098A   C9 08                CMP #$08
098C   D0 C6                BNE SHPFSHL1
098E   68                   PLA
098F   A8                   TAY
0990   68                   PLA
0991   AA                   TAX
0992   68                   PLA
0993   60         L0993     RTS
0994   48         SHPFSHED  PHA
0995   29 C0                AND #$C0
0997   85 26                STA $26
0999   4A                   LSR A
099A   4A                   LSR A
099B   05 26                ORA $26
099D   85 26                STA $26
099F   68                   PLA
09A0   85 27                STA $27
09A2   0A                   ASL A
09A3   0A                   ASL A
09A4   0A                   ASL A
09A5   26 27                ROL $27
09A7   0A                   ASL A
09A8   26 27                ROL $27
09AA   0A                   ASL A
09AB   66 26                ROR $26
09AD   A5 27                LDA $27
09AF   29 1F                AND #$1F
09B1   05 E6                ORA $E6
09B3   85 27                STA $27
09B5   A5 26                LDA $26
09B7   6D BE 09             ADC $09BE
09BA   85 26                STA $26
09BC   60                   RTS
09BD   57                   ???                ;%01010111 'W'
09BE   10 57                BPL L0A17
09C0   08                   PHP
09C1   01 00                ORA ($00,X)
09C3   00                   BRK
09C4   00                   BRK
09C5   00                   BRK
09C6   00                   BRK
09C7   00                   BRK
09C8   00                   BRK
09C9   00                   BRK
09CA   00                   BRK
09CB   00                   BRK
09CC   00                   BRK
09CD   00                   BRK
09CE   00                   BRK
09CF   00                   BRK
09D0   4C 4C 08   SOUND4    JMP BEEP4B
09D3   4C 47 08             JMP BEEP4A
09D6   4C 42 08   SPUND3    JMP BEEP3
09D9   4C 3D 08   SOUND2    JMP BEEP2
09DC   4C 38 08   SOUND1    JMP BEEP1
09DF   60                   RTS
09E0   20 7B DD   UNKNWN2   JSR FRMEVL
09E3   20 FB E6             JSR CONINT
09E6   8A                   TXA
09E7   48                   PHA
09E8   20 BE DE             JSR CHKCOM
09EB   20 7B DD             JSR FRMEVL
09EE   20 FB E6             JSR CONINT
09F1   68                   PLA
09F2   20 5C DB   UNKNWNLP  JSR OUTDO
09F5   CA                   DEX
09F6   D0 FA                BNE UNKNWNLP
09F8   60                   RTS
09F9   00                   BRK
09FA   00                   BRK
09FB   00                   BRK
09FC   02                   ???                ;%00000010
09FD   02                   ???                ;%00000010
09FE   00                   BRK
09FF   32                   ???                ;%00110010 '2'
0A00   A0 18      SELINP    LDY #$18
0A02   B1 69                LDA ($69),Y
0A04   8D FE 09             STA $09FE
0A07   A0 09                LDY #$09
0A09   B1 69                LDA ($69),Y
0A0B   8D FD 09             STA $09FD
0A0E   38                   SEC
0A0F   C8                   INY
0A10   B1 69                LDA ($69),Y
0A12   E9 01                SBC #$01
0A14   85 83                STA $83
0A16   C8                   INY
0A17   B1 69      L0A17     LDA ($69),Y
0A19   E9 00                SBC #$00
0A1B   85 84                STA $84
0A1D   A4 24      SELIP1    LDY $24
0A1F   20 0C FD             JSR RDKEY
0A22   C9 8D                CMP #$8D
0A24   D0 0D                BNE SELIP2
0A26   2C FE 09             BIT $09FE
0A29   F0 17                BEQ SELIP4
0A2B   A0 00                LDY #$00
0A2D   8C FC 09             STY $09FC
0A30   4C 6B 0A             JMP SELIP6
0A33   29 7F      SELIP2    AND #$7F
0A35   8D FF 09             STA $09FF
0A38   AC FD 09             LDY $09FD
0A3B   D1 83      SELIP3    CMP ($83),Y
0A3D   F0 09                BEQ SELIP5
0A3F   88                   DEY
0A40   D0 F9                BNE SELIP3
0A42   20 DC 09   SELIP4    JSR SOUND1
0A45   4C 1D 0A             JMP SELIP1
0A48   8C FC 09   SELIP5    STY $09FC
0A4B   20 5C DB             JSR OUTDO
0A4E   A4 24                LDY $24
0A50   20 0C FD             JSR RDKEY
0A53   C9 8D                CMP #$8D
0A55   F0 14                BEQ SELIP6
0A57   48                   PHA
0A58   A9 88                LDA #$88
0A5A   20 5C DB             JSR OUTDO
0A5D   A9 A0                LDA #$A0
0A5F   20 5C DB             JSR OUTDO
0A62   A9 88                LDA #$88
0A64   20 5C DB             JSR OUTDO
0A67   68                   PLA
0A68   4C 33 0A             JMP SELIP2
0A6B   AD FC 09   SELIP6    LDA $09FC
0A6E   A0 11                LDY #$11
0A70   91 69                STA ($69),Y
0A72   88                   DEY
0A73   A9 00                LDA #$00
0A75   91 69                STA ($69),Y
0A77   60                   RTS
0A78   A0 02      NUMINP    LDY #$02
0A7A   B1 69                LDA ($69),Y
0A7C   8D FD 09             STA $09FD
0A7F   38                   SEC
0A80   C8                   INY
0A81   B1 69                LDA ($69),Y
0A83   E9 01                SBC #$01
0A85   85 83                STA $83
0A87   C8                   INY
0A88   B1 69                LDA ($69),Y
0A8A   E9 00                SBC #$00
0A8C   85 84                STA $84
0A8E   A9 01                LDA #$01
0A90   8D FC 09             STA $09FC
0A93   A4 24      NUMIP1    LDY $24
0A95   20 0C FD             JSR RDKEY
0A98   29 7F                AND #$7F
0A9A   8D FF 09             STA $09FF
0A9D   C9 0D                CMP #$0D
0A9F   F0 5C                BEQ NUMIP6
0AA1   AD FC 09             LDA $09FC
0AA4   C9 01                CMP #$01
0AA6   D0 0A                BNE NUMIP2
0AA8   AD FF 09             LDA $09FF
0AAB   C9 41                CMP #$41
0AAD   F0 38                BEQ NUMIP4
0AAF   4C D5 0A             JMP NUMIP3
0AB2   AD FF 09   NUMIP2    LDA $09FF
0AB5   C9 08                CMP #$08
0AB7   D0 1C                BNE NUMIP3
0AB9   AC FC 09             LDY $09FC
0ABC   A9 20                LDA #$20
0ABE   91 83                STA ($83),Y
0AC0   A9 88                LDA #$88
0AC2   20 5C DB             JSR OUTDO
0AC5   A9 A0                LDA #$A0
0AC7   20 5C DB             JSR OUTDO
0ACA   A9 88                LDA #$88
0ACC   20 5C DB             JSR OUTDO
0ACF   CE FC 09             DEC $09FC
0AD2   4C 93 0A             JMP NUMIP1
0AD5   AD FC 09   NUMIP3    LDA $09FC
0AD8   C9 0A                CMP #$0A
0ADA   B0 1B                BCS NUMIP5
0ADC   AD FF 09             LDA $09FF
0ADF   C9 30                CMP #$30
0AE1   90 14                BCC NUMIP5
0AE3   C9 3A                CMP #$3A
0AE5   B0 10                BCS NUMIP5
0AE7   AC FC 09   NUMIP4    LDY $09FC
0AEA   91 83                STA ($83),Y
0AEC   EE FC 09             INC $09FC
0AEF   09 80                ORA #$80
0AF1   20 5C DB             JSR OUTDO
0AF4   4C 93 0A             JMP NUMIP1
0AF7   20 DC 09   NUMIP5    JSR SOUND1
0AFA   4C 93 0A             JMP NUMIP1
0AFD   AC FC 09   NUMIP6    LDY $09FC
0B00   C0 0A      NUMIP7    CPY #$0A
0B02   F0 08                BEQ NUMRTN
0B04   A9 20                LDA #$20
0B06   91 83                STA ($83),Y
0B08   C8                   INY
0B09   4C 00 0B             JMP NUMIP7
0B0C   60         NUMRTN    RTS
0B0D   00                   BRK
0B0E   00                   BRK
0B0F   00                   BRK
0B10   20 0C E1   SHPMATH   JSR AYINT
0B13   A5 A1                LDA $A1
0B15   0A                   ASL A
0B16   AA                   TAX
0B17   BD 3D 0B             LDA $0B3D,X
0B1A   8D 26 0B             STA $0B26
0B1D   BD 3E 0B             LDA $0B3E,X
0B20   8D 27 0B             STA $0B27
0B23   A2 00                LDX #$00
0B25   BD 83 0B   SHPMATH1  LDA $0B83,X
0B28   F0 08                BEQ SHPMATH2
0B2A   A4 24                LDY $24
0B2C   20 5C DB             JSR OUTDO
0B2F   E8                   INX
0B30   D0 F3                BNE SHPMATH1
0B32   A0 E9      SHPMATH2  LDY #$E9
0B34   A9 13                LDA #$13
0B36   20 F9 EA             JSR MOVFM
0B39   20 AE EF             JSR RND
0B3C   60                   RTS
                            .END

;auto-generated symbols and labels
 L0993      $0993
 L0A17      $0A17

;defined symbols used as labels
 BEEPJMP1   $0851
 SNDRTN     $086F
 BEEPJMP2   $085F
 BEEPDO     $0862
 SHKSNK2    $0910
 SHPSNKL3   $08E8
 SHPSNKL2   $08C6
 SHPSNKL1   $08C4
 SHPFSHED   $0994
 SHPFSHL3   $096A
 SHPFSHL2   $0956
 SHPFSHL1   $0954
 BEEP4B     $084C
 BEEP4A     $0847
 BEEP3      $0842
 BEEP2      $083D
 BEEP1      $0838
 UNKNWNLP   $09F2
 SELIP2     $0A33
 SELIP4     $0A42
 SELIP6     $0A6B
 SELIP5     $0A48
 SELIP3     $0A3B
 SOUND1     $09DC
 SELIP1     $0A1D
 NUMIP6     $0AFD
 NUMIP2     $0AB2
 NUMIP4     $0AE7
 NUMIP3     $0AD5
 NUMIP1     $0A93
 NUMIP5     $0AF7
 NUMRTN     $0B0C
 NUMIP7     $0B00
 SHPMATH2   $0B32
 SHPMATH1   $0B25
 OUTDO      $DB5C
 FRMEVL     $DD7B
 WAIT       $FCA8
 CONINT     $E6FB
 CHKCOM     $DEBE
 AYINT      $E10C
 MOVFM      $EAF9
 RND        $EFAE
 RDKEY      $FD0C
 GENINP     $0898
 SHPSNK     $08B0
 SHPFSH     $0940
 SOUND4     $09D0
 SPUND3     $09D6
 SOUND2     $09D9
 UNKNWN2    $09E0
 SELINP     $0A00
 NUMINP     $0A78
 SHPMATH    $0B10
 GENRTN     $08AC
```



## Notes
- For ship graphics:
  - What draws the ships to start with? Or is it a BASIC routine?
    - There are only two locations 0 TO 9 loop:
      - 5080
      - 5710
    - But the five subroutines at 5800-5880 do printing of ships in the 2x5 grid
      - 5800 (S) - Print ship (SH - top?)
      - 5820 (S) - Print ship (SB - bottom?)
      - 5840 (S) - Do a ship flash and animation
      - 5860 (S) - Poke at X,Y related location (what for?)
      - 5880 (S) - Calc X and Y for printing ships in battle
  - Variables
    - SH$
      - 10250 SH$ = BD$ + CG$ + "ABCDEFG" + CD$ + "HIJKLMN" + CD$ + "OIJKLPQ" + CD$ + "RSTUVWX" + CD$ + "YJJJJJZ" + DD$
    - SB$
      - 10260 SB$ = BD$: FOR II = 1 TO 5:SB$ = SB$ + "       " + CD$: NEXT II:SB$ = SB$ + DD$
    - BD$, CG$, CD$, DD$?
      - 10040 ... : CG$ =  CHR$ (1) + "2":BD$ =  CHR$ (2):CD$ =  CHR$ (3):DD$ =  CHR$ (4): ...
  - What calls the subroutines at 5800-5880:
    - 5800 called by 5730 only
    - 5820 called by 5330 and 5683
    - 5840 called by 5330 only
    - 5860 called by 5330 only
    - 5880 called by 5683, 5800, 5820, 5840 and 5860
  - CALL 2224 (Ship sink)
    - What determines which ship (screen location)
      - Line 5880???
    - What determines the speed of sinking for CALL
  - CALL 2368 (Ship flash and damage animation???)
    - What determines which ship (screen location)
      - Line 5880???
    - What determines how much damage

## Code inconsistency

### Random bytes at certain locations

While the values held at the memory locations are generally consistent, as one would expect them to be, there are some memory locations that seem to give different values, across systems and game loads.

```none
09F8: 60 00 00 04 07 00 34    IIe (UK)
09F8: 60 00 00 00 02 02 00 32 IIe Enhanced (UK)

0A00: A0 18 B1 69 8D FE 09 A0
...
```
Note that the last 5 bytes (`0x9FB`-`0x9FF`) differ. Is this due to different system, or is this just random byte noise from bytes that wren't loaded? Note also that the subsequent lines are correct and match the previous.

### Location of code

The code is held within "Text Page 2", `0x0800` to `0x0BFF`. 

The main block is from `0x0898` to `0x0B0F` and  preceded by the "sound block" at `0x0838` to `0x086F`

### Byte shift

One installation in MAME suffered a "byte shift of data". Possibly due to ending (quitting) the game and falling back to the one-liner BASIC program at the default memory location, `$4001`, rather than `$8001`. 

```none
10 POKE 103,1: POKE 104,64: POKE 16384,0: GOTO 10
```
Also a soft reset may have been executed - but would this result in wolesale "byte shift"? Seems highly unlikely.

Some examples:

```none
...
0840: 01 4C 51 08 A2 03 4C 51
...
08B0: 02 91 69 60 00 00 00 48
...
0940: 57 08 57 28 01 00 EC 48
...
09F8: 68 20 5C DB CA D0 FA 60
...
0A78: 69 88 A9 00 91 69 60 A0
0A80: 02 B1 69 8D FD 09 38 C8

```

Compare with the "original":

```none
...
...
...
...
...
0838: A2 01 4C 51 08 A2 03 4C
0840: 51 08 A2 09 4C 51 08 A2
...
08A8: A9 02 91 69 60 00 00 00
08B0: 48 8A 48 98 48 A9 20 85
...
0938: 60 57 08 57 28 01 00 EC
0940: 48 8A 48 98 48 A9 20 85
...
09F0: E6 68 20 5C DB CA D0 FA
09F8: 60 00 00 04 07 00 34    IIe (UK)
...
0A70: 91 69 88 A9 00 91 69 60
0A78: A0 02 B1 69 8D FD 09 38
0A80: C8 B1 69 E9 01 85 83 C8

```
#### Inconsistant shifting

The strange thing is that the code appears to have been shifted by differing amounts, depending upon location:

- `0x0839` (`01`) has gone to `0x0840`
  - shifted *up* by seven bytes
- `0x08A9` (`02`) has gone to `0x08B0`
  - shifted *up* by seven bytes
- The contents of `0x0939` (`57`) has gone to `0x0940`
  - shifted *up* by seven bytes
- The contents of `0x09F8` (`60`) has gone to `0x09F7` 
  - shifted *down* by one byte
-  The contents of `0x0A73` (`A9`) has gone to `0x0A7A` 
  - shifted *up* by seven bytes
- The contents of `0x0A79` (`02`) has gone to `0x0A80`
  - shifted *up* by seven bytes

##### A closer look

Between `0x0940` to `0x0A73`

The "shifted" code:
```none
0940: 57 08 57 28 01 00 EC 48
0948: 8A 48 98 48 A9 20 85 E6
0950: A9 00 8D C0 09 AD BD 09
0958: 8D BF 09 A2 A0 AD BD 09
0960: 20 94 09 A5 26 85 3C A5
0968: 27 85 3D E8 CE BD 09 A0
0970: 00 B1 3C 49 FF 91 3C C8
0978: C0 07 D0 F5 E0 2A D0 DD

0980: A9 50 20 A8 FC AD BF 09
0988: 8D BD 09 EE C0 09 AD C0
0990: 09 C9 08 D0 C6 68 A8 68
0998: AA 68 60 48 29 C0 85 26
09A0: 4A 4A 05 26 85 26 68 85
09A8: 27 0A 0A 0A 26 27 0A 26
09B0: 27 0A 66 26 A5 27 29 1F
09B8: 05 E6 85 27 A5 26 6D BE
09C0: 09 85 26 60 57 08 57 08

09C8: 01 00 00 00 00 00 00 00 
09D0: 00 00 00 00 00 00 00 4C
09D8: 4C 08 4C 47 08 4C 42 08
09E0: 4C 3D 08 4C 38 08 60 20
09E8: 7B DD 20 FB E6 8A 48 20
09F0: BE DE 20 7B DD 20 FB E6
09F8: 68 20 5C DB CA D0 FA 60

```

The "original" code:
```none
0940: 48 8A 48 98 48 A9 20 85
0948: E6 A9 00 8D C0 09 AD BD
0950: 09 8D BF 09 A2 A0 AD BD 
0958: 09 20 94 09 A5 26 85 3C 
0960: A5 27 85 3D E8 CE BD 09 
0968: A0 00 B1 3C 49 FF 91 3C 
0970: C8 C0 07 D0 F5 E0 2A D0 
0978: DD A9 50 20 A8 FC AD BF 

0980: 09 8D BD 09 EE C0 09 AD 
0988: C0 09 C9 08 D0 C6 68 A8
0990: 68 AA 68 60 48 29 C0 85
0998: 26 4A 4A 05 26 85 26 68
09A0: 85 27 0A 0A 0A 26 27 0A 
09A8: 26 27 0A 66 26 A5 27 29
09B0: 1F 05 E6 85 27 A5 26 6D
09B8: BE 09 85 26 60 57 08 57
09C0: 08 01 00 00 00 00 00 00

09C8: 00 00 00 00 00 00 00 00
09D0: 4C 4C 08 4C 47 08 4C 42
09D8: 08 4C 3D 08 4C 38 08 60
09E0: 20 7B DD 20 FB E6 8A 48
09E8: 20 BE DE 20 7B DD 20 FB
09F0: E6 68 20 5C DB CA D0 FA
09F8: 60
```


## Changing fonts

How does one change the font set?

One would think that, seeing as there are additional font sets in the memory, that displaying them would involved simply `POKE`ing a couple of memory locations (or `PEEK`ing a software switch) to set the start of the character set location. Something like this:

```none
1 REM PRINT CHAR SET
5 A=PEEK (49166): FOR N = 0 TO 255: PRINT CHR$(N);: NEXT :A=PEEK (49167): FOR N = 0 TO 255: PRINT CHR$(N);: NEXT:A=PEEK (49166):END
```

However, it doesn't seeem to work like that. Although, at [49:22](https://www.youtube.com/watch?v=9EDtnLRuFuU&t=2962) in [0096 Giving my Apple \]\[+ multiple personalities with ROMX+](https://www.youtube.com/watch?v=9EDtnLRuFuU) (a SW switch is *actually* mentioned at [49:45](https://www.youtube.com/watch?v=9EDtnLRuFuU&t=2985))

An interesting reference is the thread, [Custom character set?](https://www.applefritter.com/content/custom-character-set), in particular the posts of Toby Jennings (#5 and #7). It makes reference to *Hennig's Font Foundry*, on Nibble magazine Vol 7, disk, [NIB29B.DSK](https://discmaster.textfiles.com/browse/26751/nib29b.dsk), see `CHAR.ED` and `CHAR.GEN`.

### The ROMX solution

At [53:32](https://www.youtube.com/watch?v=9EDtnLRuFuU&t=3212) in [0096 Giving my Apple \]\[+ multiple personalities with ROMX+](https://www.youtube.com/watch?v=9EDtnLRuFuU), some BASIC code is provided (that may only work with the custom character ROMX+):

```none
340 POKE 785, C+16: REM SET TEXTBANK (FB1n)
345 X = PEEK (49376): REM ZIP CHIP fix (hit $C0E0)
350 CALL 768: REM AND CHANGE IT
```

Note that line 345 is not required.

Also at [54:28](https://www.youtube.com/watch?v=9EDtnLRuFuU&t=3268) in [0096 Giving my Apple \]\[+ multiple personalities with ROMX+](https://www.youtube.com/watch?v=9EDtnLRuFuU), the code to set up the `CALL 768`:

```none
810 FOR I = 0 TO 23: READ N: POKE 768 +I,N : NEXT
820 DATA 72,44,202.250.44.202.250,44,254,250,173
830 DATA 46,208,141,24,3,44,16,248,104,44,81,248,96
840 RETURN
```

This code is

```none
300: 48 2C CA FA 2C CA FA 2C
308: FE FA AD 2E D0 8D 18 03
310: 2C 10 F8 68 2C 51 F8 60
```

Using labels

```none
CHNGTXT = $0300
SELBNK0 = $0301

MAINBKSW = $F851
TXTBANK0 = $F810
```

Assembler:

```none
 MAINBKSW = $F851
 TXTBANK0 = $F810

                            * = $0300
0300   48         CHNGTXT   PHA
0301   2C CA FA   SELBNK0   BIT $FACA
0304   2C CA FA             BIT $FACA
0307   2C FE FA             BIT $FAFE
030A   AD 2E D0             LDA $D02E
030D   8D 18 03             STA $0318
0310   2C 10 F8             BIT TXTBANK0
0313   68                   PLA
0314   2C 51 F8             BIT MAINBKSW
0317   60                   RTS
                            .END

;defined symbols used as labels
 MAINBKSW   $F851
 TXTBANK0   $F810
 CHNGTXT    $0300
 SELBNK0    $0301
```

#### Derivation of the "ROMX" labels

According to [S-C DocuMentor — Applesoft](https://w.txbobsc.com/scsc/scdocumentor/definitions.html)

```none
FCFA-          2550 MON.RD2BIT       .EQ $FCFA
```

According to the user guide for [ROMXce](https://jdmicro.com/documentation/romxce/ROMXce+%20API%20Reference.pdf), page 2,

> $FACA	$FACA	$FAFE			(immediately switches to Bank 0)

According to the user guide for [ROMXce](https://jdmicro.com/documentation/romxce/ROMXce+%20API%20Reference.pdf), page 3, `$F851` is a SW switch that selects the Main Bank (whereas `$F850` selects the "Temporary Bank").

`F810` is used to set the Text ROM bank to 0. From page 4 of [ROMXce](https://jdmicro.com/documentation/romxce/ROMXce+%20API%20Reference.pdf)

> If you have  an optional ROMX Text ROM board installed, while in Bank 0 accessing any address in the range $F81x will preset the Main Text ROM Bank to x (where x is 0-9,A-F). The font will change when exiting Bank 0. If no Text board is installed, this will have no effect. $F815 presets the Main Text ROM to Bank 5.


### AI response for changing fonts

I got this from [this google serch](https://www.google.com/search?q=setting+applesoft+character+set&oq=setting+applesoft+character+set&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIHCAEQIRigATIHCAIQIRiPAtIBCDg2MDdqMGo3qAIIsAIB&sourceid=chrome&ie=UTF-8):

>To change the character set in Applesoft, you typically use the LOADHRCG command and load a character set file, followed by a control-A command with the appropriate number to select the desired character set. The Apple II character set is stored in the Character ROM and can be modified by swapping the EPROM. 
> 
> Elaboration:
> 
> 1. Loading Character Sets:
Applesoft allows you to load alternate character sets, which are stored in files. You can load these files using the LOADHRCG command, prompting for the filename. 
> 2. Selecting Character Sets:
Once the character sets are loaded, you can select the one you want to use by typing CTRL-A followed by the number (1-9) of the alternate character set. Ignoring the "SYNTAX ERROR" message that results from this command is necessary, as it's not a standard BASIC command. 
> 3. Suppressing the Error Message:
To suppress the syntax error message, you can type CTRL-A n CTRL-X (with no spaces). 
> 4. Returning to the Standard Character Set:
To revert to the default character set, type CTRL-A 0 CTRL-X. 
> 5. Hi-Res Character Generator (HRCG):
Using the HRCG can result in a different scrolling behavior when listing programs, scrolling one bit at a time instead of one line. 
> 6. Animating Character Sets:
The Animatix program, part of the Applesoft Toolkit, is used to create and edit character sets for the HRCG. This program allows you to define 7-by-8 pixel images for each character. 
> 7. Character ROM:
The Apple II's character set for text mode (40 columns) is stored in the Character ROM on the mainboard. You can swap this ROM with a custom EPROM to change the character set, but you cannot dynamically change it during runtime. 


## Binary files

From [Programmatically finding address of loaded binary?](https://www.reddit.com/r/apple2/comments/ud50u1/programmatically_finding_address_of_loaded_binary/)


> under DOS 3.3 AA72.AA73 will contain the last BLOAD address.

and

> AA60.AA61 holds the last BLOAD length.

From [this post](https://www.reddit.com/r/apple2/comments/ud50u1/comment/i6fnuxa/):

> One of the dumb design decisions in DOS 3.x was to save 4 bytes of meta data as the first four bytes of the binary file itself (Low/High Address and Low/High Length) instead of in the catalog which contains all the other meta data such as filename, FTOC, size, etc. See [Figure 4.8](https://mirrors.apple2.org.za/Apple%20II%20Documentation%20Project/Books/Beneath%20Apple%20DOS.pdf) of the original *Beneath Apple DOS* or [Page 42](https://archive.org/details/beneath-apple-dos-prodos-2020/page/42/mode/2up) of *Beneath Apple DOS ProDOS 2020*.
> 
> Here are the "Peeks N Pokes" for DOS 3.3 `BLOAD`
> 
> |Address	|Description|
> |---|---|
> |AA60.AA61	|Last BLOAD Length|
> |AA72.AA73	|Last BLOAD Address|
> 
> From AppleSoft
> 
> ```none
> ?PEEK(43616)+PEEK(43617)*256
> ?PEEK(43634)+PEEK(43635)*256
> ```
> 
> From the Monitor:
> 
> ```none
> CALL-151
> AA72.AA73
> AA60.AA61
> ```
> The original source code has these shitty variable names. (I've also included my annotations.)
> 
> ```none
2A60:00 00          022 SVBL        DFB 0,0                                         ; SaVed Binary Length $AA60:Length of last BLOAD, BSAVE
2A62:00             023 SVCMD       DFB 0                                           ; SaVed CoMmanD
2A63:00             024 TEMP1A      DFB 0
2A64:00             025 TEMP2A      DFB 0
2A65:00             026 INOPTS      DFB 0           ; INPUT OPTIONS
                    027 CUROPT      EQU *           ; CURRENT OPTIONS               ; /sarcasm So we can type CUR ... but too lazy to type CUR_VOL, CUR_DRV, CUR_SLT??
                                                                                    ; Note: OPTAB3 and CUROPT must be keep in size sync! 
2A66:00 00          028 CV          DW 0            ; VOLUME                        ; [0] normally wastes 1 byte; all numbers 16-bit, See CMDSCAN, Line #239
2A68:00 00          029 CD          DW 0            ; DRIVE                         ; [2] normally wastes 1 byte, all numbers 16-bit, /sarcasm because DBINIT, OPNSUP
2A6A:00 00          030 CS          DW 0            ; SLOT                          ; [4] normally wastes 1 byte, all numbers 16-bit,      need 2 bytes bytes, right?
2A6C:01 00          031 CL          DW 1            ; RECORD LENGTH                 ; [6]               
2A6E:00 00          032 CR          DW 0            ; RECORD NUMBER                 ; [8] Not Carriage Return, but Current Record. /sarcasm Obvious, right?
2A70:00 00          033 CB          DW 0            ; RECORD BYTE                   ; [A]
2A72:00 00          034 CA          DW 0            ; ADDRESS                       ; [C] $AA72:Address of last BLOAD
> ```
 
 




