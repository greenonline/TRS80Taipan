# !!!NOT (*yet*)!!! TRS80Taipan
## TRS80 version of Taipan

This *will* have the original 40 column Taipan code for the TRS80 - from whence came the  version of the Apple II<sup>1</sup> - when I can locate it and have played about with it. 

While we wait for that to materialise, instead the repo contains a deconstruction of the Hi-Res version of Taipan, for the Apple II, and some other Taipan related "stuff"...

<sup>1</sup> Evidence of this is in the source code in the book, on page XX. See [this answer](https://retrocomputing.stackexchange.com/a/28132/202) to [Looking for an accurate Apple II(e) character set - in particular, what is CHR$(133)?](https://retrocomputing.stackexchange.com/q/28127/202) for more details.

## Hi-Res Apple II version of Taipan
### An analysis

Source code (BASIC) and cassette media

Massaging the [original BASIC source code](https://taipangame.com/BASIC.txt) from [Taipan!](https://taipangame.com) into formats that are more user friendly for either:
- copying and pasting the BASIC as text into an emulator (in my case `sdltrs` and **VirtualII** on OS X);
- loading from a cassette file;
- loading from a floppy disk file (TBA);
- loading from a hard disk file (TBA).

This reprository contains:
 - A [set of scripts to do the massaging](https://github.com/greenonline/TRS80Taipan/blob/main/Listings/TRS80%20Taipan%20script%20issues.txt).
 - A [considerable number of hacks and mods to the BASIC code](https://github.com/greenonline/TRS80Taipan/blob/main/Listings/TRS80%20Taipan%20script%20issues.txt):
   - To add new features:
     - "Records"
     - "Parley" 
     - Additional data logging
   - To make a self-playing game
 - A [breakdown of the code - including CALLS, POKEs, PEEKs and USR statements](https://github.com/greenonline/TRS80Taipan/blob/main/Listings/TRS80%20Taipan%20script%20issues.txt)
 - A [disassembly of the machine code and assembler calls](https://github.com/greenonline/TRS80Taipan/blob/main/Listings/Apple_II_Hi-Res_routines.md)
 - Some media (cassette/disk) containing the code
 - A guide to [using MAME on macOS](https://github.com/greenonline/TRS80Taipan/blob/main/Misc/MAME%20on%20Mac.md)
 - A guide to [LisaEm](https://github.com/greenonline/TRS80Taipan/blob/main/Misc/LisaEmSetUp.md)
 - [Hacks for the low resolution, 40 column, version of Taipan!](https://github.com/greenonline/TRS80Taipan/blob/main/Misc/Hacks/)
   - To add new features:
     - Optional debt
     - Banking
   - [To automate game play](https://github.com/greenonline/TRS80Taipan/blob/main/Misc/Automating/)

## See also 
- [Taipan for TRS-80](https://gr33nonline.wordpress.com/2024/12/23/taipan-for-trs-80/)

## Related
- [Taipan_40_Column_Apple_II](https://github.com/greenonline/Taipan_40_Column_Apple_II/)
- [BBCTaipan](https://github.com/greenonline/BBCTaipan)
- [MacTaipan](https://github.com/greenonline/MacTaipan)
- [MMBASICTaipan](https://github.com/greenonline/MMBASICTaipan)
- [CommanderX16Taipan](https://github.com/greenonline/CommanderX16Taipan)

