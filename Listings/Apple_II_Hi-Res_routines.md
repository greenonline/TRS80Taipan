# TAIPAN (Hi-Res version) for APPLE II - DISASSEMBLY

## LINKS
http://forum.6502.org/viewtopic.php?f=3&t=5517

White flame?

https://groups.google.com/g/comp.sys.apple2/c/SBBdH1ChMwQ

Mini-Assembler/Disassembler.
https://downloads.reactivemicro.com/Users/Grant_Stockley/Apple2WozMiniAssembler.pdf
### Stack Exchange
[CALL -151 What did it do on the APPLE \]\[](https://stackoverflow.com/q/143374/4424636)
### Monitor
- [Monitor reference](http://www.easy68k.com/paulrsm/6502/MONREF.HTM)
- [Monitor source](http://www.easy68k.com/paulrsm/6502/MON.TXT)

[](https://www.masswerk.at/6502/disassembler.html)

### For ROM hooks, 
#### `$DB5C` (`OUTDO`), `$DD7B` (`FRMEVL`): 
[Apple IIc and II e Assembly Language](http://www.apple-iigs.info/doc/fichiers/Apple%20IIc%20and%20IIe%20Assembly%20Language%20(Jules%20H.%20Gilder).pdf)

#### `$E6FB` (`CONINT`) and `$DEBE` (`CHKCOM`):
[Inside the Apple II e](https://fabiensanglard.net/fd_proxy/prince_of_persia/Inside%20the%20Apple%20IIe.pdf)


## HOW TO DISASSEMBLE PRE-LOADED CODE
Instead of trying to find an AppleSoft compatible disassembler that can load non-destructively, once the code to be disassembled has been loaded, just use the built in monitor ([CALL -151](https://stackoverflow.com/q/143374/4424636)) to get the hex dump and then use an [online disassembler](https://www.masswerk.at/6502/disassembler.html) to get the assembler code.

Enter `3D0G` to return to BASIC

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

### SHIP SINK RELATED?????
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
```
#### Assembler
```none
                            * = $0940
0940   48                   PHA
0941   8A                   TXA
0942   48                   PHA
0943   98                   TYA
0944   48         L0994     PHA
0945   A9 20                LDA #$20
0947   85 E6                STA $E6
0949   A9 00                LDA #$00
094B   8D C0 09             STA $09C0
094E   AD BD 09   L094E     LDA $09BD
0951   20 94 09             JSR L0994
0954   A5 26                LDA $26
0956   85 3C                STA $3C
0958   A5 27                LDA $27
095A   85 3D                STA $3D
095C   E8                   INX
095D   CE BD 09             DEC $09BD
0960   A0 00                LDY #$00
0962   B1 3C      L0962     LDA ($3C),Y
0964   49 FF                EOR #$FF
0966   91 3C                STA ($3C),Y
0968   C8                   INY
0969   C0 07                CPY #$07
096B   D0 F5                BNE L0962
096D   E0 2A                CPX #$2A
096F   D0 DD                BNE L094E
0971   A9 50                LDA #$50
0973   20 A8 FC             JSR $FCA8  (MON: WAIT)
0976   AD BF 09             LDA $09BF
0979   8D BD 09             STA $09BD
097C   EE C0 09             INC $09C0
097F   AD C0 09             LDA $09C0
0982   C9 08                CMP #$08
0984   D0 C6                BNE L094C
0986   68                   PLA
0987   A8                   TAY
0988   68                   PLA
0989   AA                   TAX
098A   68                   PLA
098B   60                   RTS
098C   48                   PHA
098D   29 C0                AND #$C0
098F   85 26                STA $26
0991   4A                   LSR A
0992   4A                   LSR A
0993   05 26                ORA $26
0995   85 26                STA $26
0997   68                   PLA
0998   85 27                STA $27
099A   0A                   ASL A
099B   0A                   ASL A
099C   0A                   ASL A
099D   26 27                ROL $27
099F   0A                   ASL A
09A0   26 27                ROL $27
09A2   0A                   ASL A
09A3   66 26                ROR $26
09A5   A5 27                LDA $27
09A7   29 1F                AND #$1F
09A9   05 E6                ORA $E6
09AB   85 27                STA $27
09AD   A5 26                LDA $26
09AF   6D BE 09             ADC $09BE
09B2   85 26                STA $26
09B4   60                   RTS
09B5   57                   ???                ;%01010111 'W'
09B6   08                   PHP
09B7   57                   ???                ;%01010111 'W'
09B8   08                   PHP
09B9   01 00                ORA ($00,X)
09BB   00                   BRK
09BC   00                   BRK
09BD   00                   BRK
09BE   00                   BRK
09BF   00                   BRK
09C0   00                   BRK
09C1   00                   BRK
09C2   00                   BRK
09C3   00                   BRK
09C4   00                   BRK
09C5   00                   BRK
09C6   00                   BRK
09C7   00                   BRK
                            .END

;auto-generated symbols and labels
 L0994      $0994
 L0962      $0962
 L094E      $094E
 L094C      $094C
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

### ?????
`2528` (`0x09E0`) (could be nothing)

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

## Additional code


### Sound routines

- `0838` - One beep (CALL 2104)
- `083D` - Two beep (CALL 2109)
- `0842` - Three beep (CALL 2114)
- `0847` - Four beep (CALL 2119)
- `084C` - Four beep (CALL 2124)

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
