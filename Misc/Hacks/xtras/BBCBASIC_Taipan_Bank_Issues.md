# BBC BANK issues


 - 2020 and 2120 needed shortening, line number too long to enter if all VTAB=x and HTAB=y and INVERSE=0 left in.
 - NUM$ must be set to "" before calling 310 in 2030, to prevent error. Should also add in line 2130 but not essential)
 - No such variable error if `TAB (0,0)` with a space between `TAB` and `(`
 - The Bank value does not fit in stats
 - BANK hack messes up stats display: guns is shifted right, and the next line "debt" and "hold" is blank.
   - Possibly due to the weird "number field" width issue of BBC BASIC?  Nope, a test MRE prove that.
   - Are there just four tab positions that work? Nope.
     - Can we use three of the four number field positions to fit our needs of CASH/BANk/DEBT? Nope.
   - The issue is due to a required reduction of VTAB in line 140 was forgotten. Compare the hack:
```none
140 PRINT TAB(0,2) "CASH ";: Q = C:GOSUB 1330: PRINT TAB(15,2) "BANK ";: Q = BANK:GOSUB 1330:PRINT TAB(28,2) "GUNS ";G: PRINT TAB(0,3) "DEBT ";: Q = D:GOSUB 1330:PRINT TAB(28,3) "HOLD ";: Q = SH:GOSUB 1330
```
to the orignal line in BBC BASIC:

```none
140 INVERSE=0:PRINT TAB(0,1); "CASH ";: Q = C:GOSUB 1330:NORMAL=0:PRINT TAB(25,1); "GUNS ";:Q=G:GOSUB 1330: PRINT TAB(0,2); "DEBT ";: Q = D:GOSUB 1330:PRINT TAB(25,2); "HOLD ";: Q = SH:GOSUB 1330
```

Therefore, the bank hack should be
```none
140 PRINT TAB(0,1) "CASH ";: Q = C:GOSUB 1330: PRINT TAB(15,1) "BANK ";: Q = BANK:GOSUB 1330:PRINT TAB(25,1) "GUNS ";G: PRINT TAB(0,2) "DEBT ";: Q = D:GOSUB 1330:PRINT TAB(25,2) "HOLD ";: Q = SH:GOSUB 1330
```
   - Important Note: Also note that the HTAB changes from 28 to 25. This, in turn does not leave much room for the BANK value.
     - TODO: Check: Why was HTAB reduced from 28 to 25? Setting back to 28 doesn't seem to be a problem. See next point...
 - `HTAB` was set to 25 for the right most stats as the hold value was missing, see **No hold value printed** in blog.
   - The 28 should be reduced to 27 anyway, due to the origin shift between Apple (1,1) and BBC (0,0). However, even at HTAB 28 the hold value is left justifuied and displayed correctly, so what is the problem referred to in **No hold value printed** in blog? It was probably due to a missing preceding semicolon `;` issue (which then causes right justification), see **Reverse justification - Minimum Reproducible example** in blog.
   - All fixed now, stats are back to HTAB 27 and left justified.

 - Renumber alternative bank messages 2020 and 2120 to 2015 and 2115 and REM out the 2020/2120
 - Due to the messed up stats, when using the "permanent" display of the BANK value (by destructive modifying line 140 and using (un-REMming) lines 2010/2110, and REMming 2020/2120), use the *alternative* method instead - display the BANK value only when visiting the bank (by leaving 140 as it is, and un-REMming lines 2020/2120 and REMming 2010/2110). This method sort of makes sense: you have to go to the bank in order to see your current balance, plus interest. You wouldn't have a live update of the bank whilst at sea.


 - Need to reduce the VTABs by one? The Embark, Trade, Bank menu is not cleared by the BANK: <value> message at the top of the bank messages, on VTAB 13. Actually need to reduce by 2

So, 

```none
2010 REM PRINT TAB(0,13) A$: VTAB=14: PRINT TAB(0,14) A$ : PRINT TAB(0,13) "HOW MUCH WILL YOU DEPOSIT?";
2020 PRINT TAB(0,13) A$: PRINT TAB(0,14) A$:  PRINT TAB(0,15) A$ : PRINT TAB(0,13) "BANK: " BANK:  PRINT TAB(0,14) "----------------------": PRINT TAB(0,15) "HOW MUCH WILL YOU DEPOSIT?";
...
2110 REM PRINT TAB(0,13) A$: VTAB=14: PRINT TAB(0,14) A$ : PRINT TAB(0,13) "HOW MUCH WILL YOU WITHDRAW?";
2120 PRINT TAB(0,13) A$:  PRINT TAB(0,14) A$: PRINT TAB(0,15) A$ : PRINT TAB(0,13) "BANK: " BANK: PRINT TAB(0,14) "----------------------": PRINT TAB(0,15) "HOW MUCH WILL YOU WITHDRAW?";
```

becomes

```none
2010 REM PRINT TAB(0,11) A$: VTAB=14: PRINT TAB(0,12) A$ : PRINT TAB(0,11) "HOW MUCH WILL YOU DEPOSIT?";
2020 PRINT TAB(0,11) A$: PRINT TAB(0,12) A$:  PRINT TAB(0,13) A$ : PRINT TAB(0,11) "BANK: " BANK:  PRINT TAB(0,12) "----------------------": PRINT TAB(0,13) "HOW MUCH WILL YOU DEPOSIT?";
...
2110 REM PRINT TAB(0,11) A$: VTAB=14: PRINT TAB(0,12) A$ : PRINT TAB(0,11) "HOW MUCH WILL YOU WITHDRAW?";
2120 PRINT TAB(0,11) A$:  PRINT TAB(0,12) A$: PRINT TAB(0,13) A$ : PRINT TAB(0,11) "BANK: " BANK: PRINT TAB(0,12) "----------------------": PRINT TAB(0,13) "HOW MUCH WILL YOU WITHDRAW?";
```

As proof, BBC VTABs *are* reduced by one, compared to the original! Compare Applesoft version (VTAB 11 for Embark menu)

```none
360 GOSUB 130: GOSUB 1340:VTAB 11:INVERSE:PRINT A$:NORMAL:PRINT " T)RADE, R)ECORDS" ; :IF L = 0 THEN PRINT ", L)ENDER, G)ODOWN OR E)MBARK?":PRINT: PRINT A$
```

with BBC (VTAB 10)

```none
360 GOSUB 130: GOSUB 1340:INVERSE=0:PRINT TAB(0,10) A$:NORMAL=0:PRINT " T)RADE, R)ECORDS" ; :IF L = 0 THEN PRINT ", L)ENDER, G)ODOWN OR E)MBARK?":PRINT: PRINT A$
```

### Adding a BANK menu

You could chose to have the bank balance displayed in the stats, or only upon a visit to the bank. But the initialisation of BANK variable has to move from line 44 to line 36, as the menu will precede the debt menu at lines 42-44

```none
36 BANK = 0

42 VDU12: PRINT TAB(0,6); "Do you wish to show . . .": PRINT: PRINT "  1) Bank balance in the stats": PRINT: PRINT "                >> or <<":PRINT: PRINT "  2) Bank balance only when visiting the bank"
43 GOSUB 60: IF X$="2" THEN DISPLAYBANK=0
44 IF X$ <>"1" THEN GOTO 43 ELSE DISPLAYBANK=1
```

Or... much better. You could leave the bank variable initialisation at line 44 and add the bank menu at lines 41-43. I did this.

```none
41 VDU12: PRINT TAB(0,6); "Do you wish to show . . .": PRINT: PRINT "  1) Bank balance in the stats": PRINT: PRINT "                >> or <<":PRINT: PRINT "  2) Bank balance only when visiting the bank"
42 GOSUB 60: IF X$="2" THEN DISPLAYBANK=0
43 IF X$ <>"1" THEN GOTO 42 ELSE DISPLAYBANK=1

44 BANK = 0

```

Is the bank menu at the beginning, a little gratuitous and may become tedious? Just have a constant set at the start of the code
```none
43 DISPLAYBANK=0 : REM Change to 1 if you want bank balance displayed in the stats
```

Either way, the stats display is "diddled" like this:

```none
139 IF DISPLAYBANK=0 THEN PRINT TAB(0,1) "CASH ";: Q = C:GOSUB 1330: PRINT TAB(27,1) "GUNS ";G: PRINT TAB(0,2) "DEBT ";: Q = D:GOSUB 1330:PRINT TAB(27,2) "HOLD ";: Q = SH:GOSUB 1330
140 IF DISPLAYBANK THEN PRINT TAB(0,1) "CASH ";: Q = C:GOSUB 1330: PRINT TAB(15,1) "BANK ";: Q = BANK:GOSUB 1330:PRINT TAB(27,1) "GUNS ";G: PRINT TAB(0,2) "DEBT ";: Q = D:GOSUB 1330:PRINT TAB(27,2) "HOLD ";: Q = SH:GOSUB 1330
```
and likewise, the bank visit needs to make use of the conditional as well

```none
2010 IF DISPLAYBANK THEN PRINT TAB(0,11) A$: VTAB=14: PRINT TAB(0,12) A$ : PRINT TAB(0,11) "HOW MUCH WILL YOU DEPOSIT?";
2020 F DISPLAYBANK=0 THEN PRINT TAB(0,11) A$: PRINT TAB(0,12) A$:  PRINT TAB(0,13) A$ : PRINT TAB(0,11) "BANK: " BANK:  PRINT TAB(0,12) "----------------------": PRINT TAB(0,13) "HOW MUCH WILL YOU DEPOSIT?";
```

and

```none
2110 IF DISPLAYBANK THEN PRINT TAB(0,11) A$: VTAB=14: PRINT TAB(0,12) A$ : PRINT TAB(0,11) "HOW MUCH WILL YOU WITHDRAW?";
2120 IF DISPLAYBANK=0 THEN PRINT TAB(0,11) A$:  PRINT TAB(0,12) A$: PRINT TAB(0,13) A$ : PRINT TAB(0,11) "BANK: " BANK: PRINT TAB(0,12) "----------------------": PRINT TAB(0,13) "HOW MUCH WILL YOU WITHDRAW?";
```
