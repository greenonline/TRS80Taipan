# Hacks for the low resolution, 40 column, version of Taipan!

## Preamble

The [Hi-Res version][1] of [Taipan!][2] has two features that the [40 column version of Taipan][3] does not:

 - Banking
 - Option of starting with debt or not.

I have added these two features to the 40 column version for Apple II, as well as for the ports to  MMBASIC and BBC BASIC.

## Hacks

### Optional debt

#### For AppleSoft

```none
45 HOME: VTAB 6: PRINT "Do you wish to start . . .": PRINT: PRINT "  1) With cash (and a debt)": PRINT: PRINT "                >> or <<":PRINT: PRINT "  2) With five guns and no cash": PRINT "                (But no debt)"
46 GOSUB 60: IF X$="2" THEN FOR I = 0 TO 9: READ LO(I):NEXT I:D = 0: Y=1860: GT = 1: C=0:MW = 50:SH = MW:SR = 1: G=5:V(0) = 1: GOSUB 5000: X$ ="":GOSUB 590: HOME:GOTO 120
47 IF X$ <>"1" THEN GOTO 46
```


#### For MMBASIC

```none
45 PRINT CLRSCRN$: PRINT @(0,6*FONTY) "Do you wish to start . . .": PRINT: PRINT "  1) With cash (and a debt)": PRINT: PRINT "                >> or <<":PRINT: PRINT "  2) With five guns and no cash": PRINT "                (But no debt)":REM NO DEBT
46 GOSUB 60: IF KEY$="2" THEN DIM INTEGER LO(9) = (21,14,7,0,35,49,56,42,84,200):D = 0: Y=1860: GT = 1: C=0:MW = 50:SH = MW:SR = 1: G=5:VISITS(0) = 1: GOSUB 5000: KEY$ ="":GOSUB 590: HOME=0:PRINT CLRSCRN$:GOTO 120:REM NO DEBT
47 IF KEY$ <> "1" THEN GOTO 46:REM NO DEBT
```

As MMBASIC has a number of flaws, with one of them being that you can't just paste in new code and hope that the line numbers sort themselves out, here are the complete files for each debt and bank hack, plus the two combined:

 - [MMBASIC Taipan with debt option][4]
 - [MMBASIC Taipan with banking feature][5]
 - [MMBASIC Taipan with debt option and banking feature][6]


#### For BBC BASIC

```none
45 HOME=0:VDU12: VTAB=6: PRINT TAB (0,6); "Do you wish to start . . .": PRINT: PRINT "  1) With cash (and a debt)": PRINT: PRINT "                >> or <<":PRINT: PRINT "  2) With five guns and no cash": PRINT "                (But no debt)"
46 GOSUB 60: IF X$="2" THEN FOR I = 0 TO 9:READ LO(I):NEXT I:D = 0: Y=1860: GT = 1: C=0:MW = 50:SH = MW:SR = 1: G=5:V(0) = 1: GOSUB 5000: X$ ="":GOSUB 590:HOME=0:VDU12:GOTO 120
47 IF X$ <>"1" THEN GOTO 46
```



### Banking

#### For AppleSoft

```none
44 BANK = 0

140 VTAB 2: INVERSE:PRINT "CASH ";: Q = C:GOSUB 1330: NORMAL:VTAB 2:HTAB 15:PRINT "BANK ";: Q = BANK:GOSUB 1330:VTAB 2:HTAB 28:PRINT "GUNS ";G: VTAB 3:PRINT "DEBT ";: Q = D:GOSUB 1330:VTAB 3:HTAB 28:PRINT "HOLD ";: Q = SH:GOSUB 1330


165 BANK = INT (BANK + BANK * (0.005/30)*ET)

355 REM OTHER OPTIONS (360-381)
360 GOSUB 130: GOSUB 1340:VTAB 11:INVERSE:PRINT A$:NORMAL:PRINT " T)RADE, R)ECORDS" ; :IF L = 0 THEN PRINT ", L)ENDER, G)ODOWN, B)ANK OR E)MBARK?":PRINT: PRINT A$
361 IF L <> 0 THEN PRINT " OR E)MBARK?": PRINT:PRINT: PRINT A$;
370 GOSUB 60:IF (X$ = "G" OR X$ = "g") AND L = 0 THEN X = 1
371 IF X$ = "T" OR X$ = "t" THEN GOTO 120
372 IF X$="R" OR X$ = "r" THEN X=2
373 IF X$="L" OR X$ = "l" THEN X=3
374 IF X$="E" OR X$ = "e" THEN GOTO 730
375 IF (X$="B" OR X$ = "b") AND L = 0 THEN X=4
376 IF ((X$ <> "G" AND X$ <> "g") OR L<>0) AND ((X$ <> "B" AND X$ <> "b") OR L<>0) AND X$ <> "T" AND X$<> "R" AND X$ <> "L" AND X$ <> "E" AND X$ <> "t" AND X$<> "r" AND X$ <> "l" AND X$ <> "e" THEN GOSUB 770: GOTO 370
376 IF (X$ <> "G" OR L<>0) AND (X$ <> "B" OR L<>0) AND X$ <> "T" AND X$<> "R" AND X$ <> "L" AND X$ <> "E" THEN GOSUB 770: GOTO 370


381 IF X <> 3 OR L = 0 THEN ON X GOSUB 390,600,470,2000: GOTO 360



1300 HOME:NW = C + BANK - D:Q = NW / GT: VTAB 4: INVERSE: PRINT A$;: NORMAL: PRINT:PRINT "YOUR SCORE, BASED UPON TIME AND YOUR": PRINT "NET WORTH (EXCLUDING STOCK) IS ";: GOSUB 1330:INVERSE: PRINT A$: NORMAL
1305 PRINT " BANK: " BANK: PRINT "CASH: " C: PRINT "Debt: " D: PRINT "GAME TIME: " GT

2000 REM VISIT BANK ROUTINE (2000-2190)
2005 REM DEPOSIT
2010 REM VTAB 13: HTAB 1
2020 PRINT "HOW MUCH WILL YOU DEPOSIT?                                            "
2020 PRINT "HOW MUCH WILL YOU DEPOSIT?"
2020 VTAB 13: HTAB 1 : PRINT "HOW MUCH WILL YOU DEPOSIT?"
2020 VTAB 11:INVERSE:PRINT A$:NORMAL: PRINT A$: PRINT A$: PRINT A$: PRINT A$: PRINT A$: VTAB 13: PRINT "HOW MUCH WILL YOU DEPOSIT?"
2020 VTAB 11:INVERSE:PRINT A$:NORMAL: PRINT: PRINT: PRINT : PRINT : PRINT : VTAB 13: PRINT "HOW MUCH WILL YOU DEPOSIT?"
2020 VTAB 11:INVERSE:PRINT A$:NORMAL: VTAB 13 : PRINT A$: VTAB 14: PRINT A$ : VTAB 13: PRINT "HOW MUCH WILL YOU DEPOSIT?"
2020 VTAB 13 : PRINT A$: VTAB 14: PRINT A$ : VTAB 13: PRINT "HOW MUCH WILL YOU DEPOSIT?";
2020 VTAB 13 : PRINT A$: VTAB 14: PRINT A$: VTAB 15: PRINT A$ : VTAB 13: PRINT "BANK: " BANK: VTAB 14: PRINT "----------------------": VTAB 15: PRINT "HOW MUCH WILL YOU DEPOSIT?";
2030 GOSUB 310
2040 IF X$ = "A" THEN PRINT CHR$ (8);: INVERSE:PRINT "ALL"; : NORMAL: NUM = C
2050 IF X$ <> "A" THEN NUM = VAL (NUM$)
2060 IF NUM <= C THEN BANK = BANK + NUM: C = C - NUM: GOSUB 130: GOTO 2100
2070 GOSUB 760
2100 REM WITHDRAW
2110 REM VTAB 13 : HTAB 1
2120 REM PRINT "HOW MUCH WILL YOU WITHDRAW?                                           "
2120 REM PRINT "HOW MUCH WILL YOU WITHDRAW?"
2120 VTAB 13 : PRINT A$: VTAB 14: PRINT A$ : VTAB 13: PRINT "HOW MUCH WILL YOU WITHDRAW?";
2120 VTAB 13 : PRINT A$: VTAB 14: PRINT A$: VTAB 15: PRINT A$ : VTAB 13: PRINT "BANK: " BANK: VTAB 14: PRINT "----------------------": VTAB 15: PRINT "HOW MUCH WILL YOU WITHDRAW?";
2130 GOSUB 310
2140 IF X$ = "A" THEN PRINT CHR$ (8);: INVERSE:PRINT "ALL"; : NORMAL: NUM = BANK
2150 IF X$ <> "A" THEN NUM = VAL (NUM$)
2160 IF NUM <= BANK THEN BANK = BANK - NUM: C = C + NUM: GOSUB 130: GOTO 2190
2170 GOSUB 760
2190 RETURN: REM LEAVE BANK


```


#### For MMBASIC

```none
44 BANK = 0 



141 VTAB=2: INVERSE=0:PRINT @(0*FONTX,2*FONTY) "CASH ";: Q = C:GOSUB 1330: PRINT @(15*FONTX,2*FONTY) "BANK ";: Q = BANK:GOSUB 1330

165 BANK = INT (BANK + BANK * (0.005/30)*ET)

355 REM OTHER OPTIONS (360-381)
360 GOSUB 130: GOSUB 1340:VTAB=11:INVERSE=0:PRINT @(0*FONTX,11*FONTY) ABLANK$:NORMAL=0:PRINT " T)RADE, R)ECORDS" ; :IF L = 0 THEN PRINT ", L)ENDER, G)ODOWN, B)ANK OR E)MBARK?":PRINT: PRINT ABLANK$
361 IF L <> 0 THEN PRINT " OR E)MBARK?": PRINT:PRINT: PRINT ABLANK$;
370 GOSUB 60:IF (KEY$ = "G" OR KEY$ = "g") AND L = 0 THEN X = 1
371 IF KEY$ = "T" OR KEY$ = "t" THEN GOTO 120
372 IF KEY$="R" OR KEY$ = "r" THEN X=2
373 IF KEY$="L" OR KEY$ = "l" THEN X=3
374 IF KEY$="E" OR KEY$ = "e" THEN GOTO 730
375 IF (KEY$="B" OR KEY$ = "b") AND L = 0 THEN X=4
376 IF ((KEY$ <> "G" AND KEY$ <> "g") OR L<>0) AND ((KEY$ <> "B" AND KEY$ <> "b") OR L<>0) AND KEY$ <> "T" AND KEY$<> "R" AND KEY$ <> "L" AND KEY$ <> "E" AND KEY$ <> "t" AND KEY$<> "r" AND KEY$ <> "l" AND KEY$ <> "e" THEN GOSUB 770: GOTO 370


381 IF X <> 3 OR L = 0 THEN ON X GOSUB 390,600,470,2000: GOTO 360


1300 HOME=0:PRINT CLRSCRN$:NW = C + BANK - D:Q = NW / GT: VTAB=4: INVERSE=0: PRINT @(0*FONTX,4*FONTY) ABLANK$;: NORMAL=0: PRINT
1301 PRINT "YOUR SCORE, BASED UPON TIME AND YOUR": PRINT "NET WORTH (EXCLUDING STOCK) IS ";: GOSUB 1330:INVERSE=0: PRINT ABLANK$: NORMAL=0
1305 PRINT " BANK: " BANK: PRINT "CASH: " C: PRINT "Debt: " D: PRINT "GAME TIME: " GT


2000 REM VISIT BANK ROUTINE (2000-2190)
2005 REM DEPOSIT
2010 REM VTAB 13: HTAB 1
2020 PRINT @(0, 13*FONTY) A$: PRINT @(0, 14*FONTY) A$ : PRINT @(0, 13*FONTY) "HOW MUCH WILL YOU DEPOSIT?";
2020 PRINT @(0, 13*FONTY) A$: PRINT @(0, 14*FONTY) A$ : PRINT @(0, 15*FONTY) A$ :  PRINT @(0, 13*FONTY)"BANK: " BANK:  PRINT @(0, 14*FONTY)"----------------------":  PRINT @(0, 15*FONTY)"HOW MUCH WILL YOU DEPOSIT?";
2030 GOSUB 310
2040 IF KEY$ = "A" THEN PRINT CHR$ (8);: INVERSE:PRINT "ALL"; : NORMAL: NUM = C
2050 IF KEY$ <> "A" THEN NUM = VAL (NUMBER$)
2060 IF NUM <= C THEN BANK = BANK + NUM: C = C - NUM: GOSUB 130: GOTO 2100
2070 GOSUB 760
2100 REM WITHDRAW
2110 REM VTAB 13 : HTAB 1
2120 PRINT @(0, 13*FONTY) A$: PRINT @(0, 14*FONTY) A$ : PRINT @(0, 13*FONTY) "HOW MUCH WILL YOU WITHDRAW?";
2120 PRINT @(0, 13*FONTY) A$: PRINT @(0, 14*FONTY) A$ : PRINT @(0, 15*FONTY) A$ :  PRINT @(0, 13*FONTY)"BANK: " BANK:  PRINT @(0, 14*FONTY)"----------------------":  PRINT @(0, 15*FONTY) "HOW MUCH WILL YOU WITHDRAW?";
2130 GOSUB 310
2140 IF KEY$ = "A" THEN PRINT CHR$ (8);: INVERSE:PRINT "ALL"; : NORMAL: NUM = BANK
2150 IF KEY$ <> "A" THEN NUM = VAL (NUMBER$)
2160 IF NUM <= BANK THEN BANK = BANK - NUM: C = C + NUM: GOSUB 130: GOTO 2190
2170 GOSUB 760
2190 RETURN: REM LEAVE BANK
```


#### For BBC BASIC

```none
Not complete yet
```



  [1]: https://mirrors.apple2.org.za/ftp.apple.asimov.net/images/games/simulation/taipan%2B.dsk
  [2]: https://taipangame.com
  [3]: https://taipangame.com/pdf/TaipanAHistoricalAdventureForTheAppleComputerAppleIIEdition.pdf
  [4]: https://github.com/greenonline/TRS80Taipan/blob/main/Misc/Hacks/MMBASIC/MMBASICTaipan_Debt.bas
  [5]: https://github.com/greenonline/TRS80Taipan/blob/main/Misc/Hacks/MMBASIC/MMBASICTaipan_Bank.bas
  [6]: https://github.com/greenonline/TRS80Taipan/blob/main/Misc/Hacks/MMBASIC/MMBASICTaipan_DebtBank.bas
