# Automating the 40 column version of Taipan!

Some of these are automation hacks, and some are downright cheats:

 - Hacks
   - Auto leave trading
   - Auto Embark
   - Auto destination
   - No donation
   - No spacebar at splash
 - Cheats
   - No storm
   - No pirates
   - No debt

The individual hacks are listed below, for each of the platforms that the 40 column version of Taipan has been ported to:

 - [AppleSoft BASIC](https://github.com/greenonline/Taipan_40_Column_Apple_II) (AS)
 - [BBCBASIC](https://github.com/greenonline/BBCTaipan)        (BB)
 - [MMBASIC](https://github.com/greenonline/CommanderMMBASICTaipan)         (MM)
 - [Commander X16](https://github.com/greenonline/CommanderX16Taipan)   (CX)

## The hacks

Note: These are for the 40 column version of Taipan! For automation hacks of the Hi-Res version, please see the poorly named repository: [TRS80Taipan](https://github.com/greenonline/TRS80Taipan) and the file [`script_issues.txt`](https://github.com/greenonline/TRS80Taipan/blob/main/Listings/TRS80%20Taipan%20script%20issues.txt).

### Auto Embark

#### Non-Destructive (MM, AS, CX)

```none
369 GOTO 730 : REM HACK
```
#### Destructive (MM, AS, CX)
```none
370 GOTO 730 : REM HACK
```
### Auto Destination

From Hong Kong to Liverpool.

#### Non-Destructive (MM, AS, CX)

```none
745 PO=ABS(9-L):GOTO 980 : REM HACK
```

### CHEAT: Always ride out the storm
#### Non-Destructive (MM, AS, CX)
```none
995 GOTO 1020 : REM HACK
```
### CHEAT: Pirates never attack
#### Non-Destructive (MM, AS, CX)
```none
1035 GOTO 1290 : REM HACK
```
### No donation
#### Non-Destructive (MM)
```none
835 KEY$="N":GOTO 841 : REM HACK
```
#### Non-Destructive (AS)
```none
835 X$="N":GOTO 841 : REM HACK
```
#### Non-Destructive (CX)
```none
836 GOTO 850 : REM HACK
```
or better still,
```none
836 X$="N": GOTO 846 : REM HACK
```
### Leave trading automatically
#### Non-Destructive (MM)
```none
236 KEY$="L":GOTO 242 : REM HACK
```
#### Non-Destructive (AS, CX)
```none
236 X$="L":GOTO 242 : REM HACK
```
### No spacebar at splash
#### Non-Destructive (MM)

Does not work, as there is a `GOSUB 60` in line 590, which resets `KEY$`.
```none
49 DIM INTEGER LO(9) = (21,14,7,0,35,49,56,42,84,200) : D = 1000: Y=1860: GT = 1: C=400:MW = 50:SH = MW:SR = 1: G=1:VISITS(0) = 1: GOSUB 5000: KEY$ = " ":GOSUB 590: HOME=0:PRINT CLRSCRN$:GOTO 120 : REM HACK
```
Better this which skips the GOSUB 590:
```none
49 DIM INTEGER LO(9) = (21,14,7,0,35,49,56,42,84,200) : D = 1000: Y=1860: GT = 1: C=400:MW = 50:SH = MW:SR = 1: G=1:VISITS(0) = 1: GOSUB 5000: KEY$ = "": HOME=0:PRINT CLRSCRN$:GOTO 120 : REM HACK
```
#### Non-Destructive (AS)
```none
49 FOR I = 0 TO 9:READ LO(I):NEXT I:D = 1000: Y=1860: GT = 1: C=400:MW = 50:SH = MW:SR = 1: G=1:V(0) = 1: GOSUB 5000: X$ ="": HOME:GOTO 120: REM HACK
```
#### Non-Destructive (CX)
```none
52 GOSUB 5000: X$ ="":GOTO 120: REM HACK
```
### CHEAT: Start with no debt

Supercede line 50 with line 49 and initialise `D` to zero, rather than `1000`.

Note the following is a combined fix with **No spacebar at splash above**, with the `GOSUB 590` removed.

#### Non-Destructive (MM)
```none
49 DIM INTEGER LO(9) = (21,14,7,0,35,49,56,42,84,200) : D = 0: Y=1860: GT = 1: C=400:MW = 50:SH = MW:SR = 1: G=1:VISITS(0) = 1: GOSUB 5000: KEY$ = "": HOME=0:PRINT CLRSCRN$:GOTO 120 : REM HACK
```
#### Non-Destructive (AS)
```none
49 FOR I = 0 TO 9:READ LO(I):NEXT I:D = 0: Y=1860: GT = 1: C=400:MW = 50:SH = MW:SR = 1: G=1:V(0) = 1: GOSUB 5000: X$ ="": HOME:GOTO 120: REM HACK
```
## Applesoft only: Overflow error in 160

If the code is autorun on the Apple Soft version, then an error eventually occurs:
```none
?OVERFLOW ERROR IN 160
]PRINT D
1.60215034E+38
]PRINT Y
1962
```

Again, as per the similar issue on MMBASIC, this is due to the absence of a cap on the debt.

Fixes include:

1. Game over when debt reaches MAXINT
   ```none
   162 IF D > MAXINT THEN PRINT CLRSCRN$: PRINT @(0,0) "GAME OVER!": PRINT "DEBT CEILING REACHED!": GOSUB 780: GOTO 1300
   ```
2. Cap debt at MAXINT
   ```none
   162 IF D > MAXINT THEN D = MAXINT
   ```
From page 31 of the Green AppleSoft BASIC manual

In APPLESOFT, reals must be in the range -1E38 to 1E38 or you risk the ?OVERFLOW ERROR message. Using addition or subtraction, you may be able to generate numbers as large as 1.7E38 without receiving this message.

So, hardcoding in the maximum value
```none
162 IF D > 1E38 THEN D = 1E38
```
Or add the line
```none
4 MAXINT=1E38
```
This is the end, my friend.

## Related repos

 - [!!!NOT!!! TRS80Taipan](https://github.com/greenonline/TRS80Taipan)
 - [MMBASICTaipan](https://github.com/greenonline/MMBASICTaipan)
 - [MacTaipan](https://github.com/greenonline/MacTaipan)
 - [Taipan_40_Column_Apple_II](https://github.com/greenonline/Taipan_40_Column_Apple_II)
 - [BBCTaipan](https://github.com/greenonline/BBCTaipan)
 - [CommanderX16Taipan]()


## See also

 - [Cheats and Hacks - For X16BASIC, MMBASIC, AppleSoft BASIC and BBC BASIC versions of Taipan.][1]




  [1]: https://gr33nonline.wordpress.com/2025/06/24/automating-40-column-taipan/
