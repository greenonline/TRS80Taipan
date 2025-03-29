### To run, not from command line
Find **m64**, right click, select,**Package Contents**, and in
`Contents/Resources/script`

```none
#!/bin/sh

#cd ../../..
cd ~/Downloads/mame0261-x86
#./mame64
exec ./mame -video opengl
```

### To run, from command line


Command line SE MAME:

```none
# From [MAME: Trying to Emulate System 6](https://www.emaculation.com/forum/viewtopic.php?t=11247)

./mame macse -hard1 macsehd.chd -flop1 SSW_6.0.8-800k_Disk1of4.img -flop2 SSW_6.0.8-800k_Disk2of4.img

# I used

./mame macse -hard1 Mac40MB.chd -flop1 System_Tools.image -flop2 Utilities_1.image

# You only need

./mame macse -hard1 Mac40MB.chd -flop1 System_Tools.image
```

## Required files

There requires files are:

 - System installation disks
 - ROM
 - Blank hard disk images
 - Particular to Mac:
   - ADB Modem files

### ROM

Download [Mac SE ROM](https://archive.org/download/MAME_0.197_ROMs_merged/MAME_0.197_ROMs_merged.zip/MAME%200.197%20ROMs%20%28merged%29%2Fmacse.zip) from [Merged 0.197 ROMs](https://ia803104.us.archive.org/view_archive.php?archive=/12/items/MAME_0.197_ROMs_merged/MAME_0.197_ROMs_merged.zip). 

IMPORTANT NOTE: Place the ROM file in a directory!!!! Like this

```none
mame0261-x86/roms/macse/macse.rom
```

### Blank hard disk images

[Blank hard disk images from MESS Mac setup made easier](https://rbelmont.mameworld.info/?p=605) 

### System installation disks

[System 6.x - Mac System/OS 6.x (6.0, 6.0.1, 6.0.2, 6.0.3, 6.0.4, 6.0.5 , 6.0.6, 6.0.7, 6.0.8, 6.0.8L](https://www.macintoshrepository.org/1778-mac-system-os-6-x-6-0-6-0-1-6-0-2-6-0-3-6-0-4-6-0-5-6-0-6-6-0-7-6-0-8-6-0-8l-)

### ADB Modem files

An additional [*device set*](https://docs.mamedev.org/usingmame/aboutromsets.html) is required.

[adbmodem.zip](https://archive.org/download/mame251/adbmodem.zip) from [Mame 0.251 ROMS (Merged)](https://archive.org/details/mame251).

IMPORTANT NOTE: Place the (optionally unzipped) `adbmodem.zip` file in the `roms` directory, like this:

```none
mame0261-x86/roms/macse/342s0440-b.bin
```

## Observations

### Command line

Note: Only `-flop1` seems to matter. When within MAME, only floppies inserted into slot 1 seem to  be recognised and installed.

### Switching floppy disks

Switch disk by <kbd>DEL</kbd> key, or <kbd>Fn</kbd> + <kbd>Backspace</kbd>, and then <kbd>Tab</kbd> and then File Manager, and add disks to slot 1 as required.

### Exit

How do you exit MAME?

### Accessing the second floppy disk

Why isn't the second disk used on the Mac SE during system installation?

### Accessing nested ROM

How do you start the coco3 machine within the coco ROM set?

### ZIPped or unzipped?

Is a directory in the `roms/` directory meant to contain a zip file, or is it meant to be unzipped? 

Conflicting info? From [ROMs and CHDs](https://docs.mamedev.org/usingmame/aboutromsets.html)

> As CHD files are already compressed, they should not be stored PKZIP or 7-Zip archives as ROM images would be.

From [System ROMs](https://docs.mamedev.org/usingmame/assetsearch.html#system-roms)

> ... so MAME will look for a folder called jgakuen, a PKZIP archive called jgakuen.zip, or a 7-Zip archive called jgakuen.7z.

I have used both `macplus.zip` and `macplus/` and they both work. The unzipped version may be less processor intensive and/or faster? Plus, wihout wishing to state the obvious, it is also easier to view the contents of the *unzipped* files when trying to debug missing ROM files.

### COCO is just a directory for packaging?

Seeing as [`coco.zip`](https://archive.org/download/mame251/coco.zip) contains `coco3/`, plus a number of others... is the `coco/` directory, actually just a directory that bundles all fo the coco related stuff together? Do the contents need to be brought up to the top level of `roms/`. In other words, should the contents of `coco.zip` be placed directly within `roms/`?

Moving `coco3/` from `roms/coco/coco3` to `roms/coco3/`, actually made the COCO3 emulation work, and strangley, the `Missing "disk11.rom"` error didn't show, even though it isn't in the `coco3/` but rather in `coco/`, like so

```none
mame/
    roms/
        coco3/
             coco3.rom
        coco/
             bas12.rom
             extrabas11.rom
             disk11.rom
             bas10.rom
```

Note that there is no `coco.rom`..!

The answer is revealed if you remove `roms/coco/` and then run `mame coco3` again. This time an error does occur:

```none
disk11.rom NOT FOUND (tried in coco_fdc coco3 coco)
```

So, MAME looks in *both* `coco/` and `coco3/` to find the `disk11.rom` file.

As to whether the *entire* contents of `coco/` should be spilled into the top level of `roms/`, well, if you did that, then:

 - It would look less tidy
 - Where would the "loose" files within `coco/` be placed? Loose within `roms/`? These files are :
   - `bas12.rom`
   - `extbas11.rom`
   - `disk11.rom`
   - `bas10.rom`

It really seems as if there should be a way for accessing "sub machines", like so

```none
mame coco/coco3
```
But this doesn't seem to be the case.

From [Archive files](https://docs.mamedev.org/usingmame/assetsearch.html#archive-files), 

> MAME does not load files from nested archives.

From [How does MAME search for media](https://docs.mamedev.org/usingmame/assetsearch.html#how-does-mame-search-for-media)

> but remember the limitation that MAME cannot load files from an archive contained within another archive.
> 
> ...
> 
> MAME looks for a folder first, then a PKZIP archive, and finally a 7-Zip archive.
> 
> ...
> 
> It’s best to keep CHD format disk images in folders (not in compressed files).

#### Coco2b and Coco3p

Note that from withon `coco/`, the directories `coco2b` and `coco3p` can also be moved to the top level `roms/`.

However, `coco2` can not be moved - it does not make coco2 work - in fact, there is no coco2 machine:

```none
Unknown system 'coco2'

"coco2" approximately matches the following
supported machines (best match first):

coco2b            Color Computer 2B                (Tandy Radio Shack, 1985?)
coco              Color Computer 1/2               (Tandy Radio Shack, 1980)
coco2bh           Color Computer 2B (HD6309)       (Tandy Radio Shack, 19??)
coco3             Color Computer 3 (NTSC)          (Tandy Radio Shack, 1986)
cocoh             Color Computer 1/2 (HD6309)      (Tandy Radio Shack, 19??)
coco3h            Color Computer 3 (NTSC; HD6309)  (Tandy Radio Shack, 19??)
coco3p            Color Computer 3 (PAL)           (Tandy Radio Shack, 1986)
```

### Starting in a window makes it easier to quit

Use the `-window` option, to gain access to the Qut menu (which does not work) ad the close window button that does work. Otherwise, you seem to have to force quit the full screen emulation (Using `Opt-Cmd-Esc`), as close window nor quit work in full screen.

```none
./mame coco3 -window
```

### Where is `mac_flop` and `sys603`?

Where are these images/packages downloaded from? See **Roll your own...** below.

### Roll your own `sys608`

Looking at the contents of [`mac_flop`](https://sources.debian.org/src/mame/0.251%2Bdfsg.1-1/hash/mac_flop.xml/) for the sys60x` entries, they just point to the `.img` files:

```xml
	<software name="sys603">
		<!-- This version of the System Software was shipped with the Macintosh IIcx and SE/30 -->
		<description>System Software 6.0.3</description>
		<year>1990</year>
		<publisher>Apple</publisher>
		<part name="flop1" interface="floppy_3_5">
			<dataarea name="flop" size="819200">
				<rom name="system tools.img" size="819200" crc="297023b0" sha1="ced0f8d2e950ef59bc4ff2477e09ea4cc6f7b24c" offset="0" />
			</dataarea>
		</part>
		<part name="flop2" interface="floppy_3_5">
			<dataarea name="flop" size="819200">
				<rom name="utilities 1.img" size="819200" crc="67ab3033" sha1="247a99f42237a780ea08f068473e6ee127a5eab7" offset="0" />
			</dataarea>
		</part>
		<part name="flop3" interface="floppy_3_5">
			<dataarea name="flop" size="819200">
				<rom name="utilities 2.img" size="819200" crc="0b1d1e4d" sha1="4969936683619562dfd1437849da9119118cb695" offset="0" />
			</dataarea>
		</part>
		<part name="flop4" interface="floppy_3_5">
			<dataarea name="flop" size="819200">
				<rom name="printing tools.img" size="819200" crc="8b69b1ed" sha1="087e19062715c59a9ce01106ed03049a364b4018" offset="0" />
			</dataarea>
		</part>
	</software>
```

So [download the floppy image files](https://www.macintoshrepository.org/1778-mac-system-os-6-x-6-0-6-0-1-6-0-2-6-0-3-6-0-4-6-0-5-6-0-6-6-0-7-6-0-8-6-0-8l-) and make your own "ROM" by placing in directories:

```none
roms/
    sys608/
          Printing_Tools.image
          System_Tools.image
          Utilities_1.image
          Utilities_2.image
```

Note that, according to `mac_flop.xml` the filenames need to be changed slightly:

```none
roms/
    sys608/
          printing tools.img
          system tools.img
          utilities 1.img
          utilities 2.img
```
You might get a checksum error, but it can be ignored:

```none
% ./mame macplus -flop1 sys608:flop1
system tools.img WRONG LENGTH (expected: 819200 found: 838484)
system tools.img WRONG CHECKSUMS:
    EXPECTED: CRC(febb735f) SHA1(fb89deb29ff341e92fd30307934f7c7616c2c8e2)
       FOUND: CRC(ec988f03) SHA1(c43218cec1bab8ded5cefd1ae254e035e16a8ad4)
WARNING: the software item might not run correctly.
```

However, this "package" did not work as expected. After the installation finished reading the first "system tools.img" disk, I received the following message 

```none
Error, unable to truncate image: Invalid argument
```

and it did not "automatically" insert the floppies for me. I still had to manually insert the other three floppies, via the MAME File Manager menu.

Note: The message comes from [line 492, floppy.cpp](https://github.com/mamedev/mame/blob/master/src/devices/imagedev/floppy.cpp#L492)

Maybe due to the invalid checksum error? WHich in turn may be due to my obtaining the disks from a ["combined" System 6 source](https://www.macintoshrepository.org/download.php?id=2969).

Trying again by constructing `sys605` using a different source of installation media, [Apple-Mac-OS-6-0-5--3.5-800k-.zip](https://www.macintoshrepository.org/download.php?id=57510)

This time, I didn't change the file names from their orignal title case (case insensetive???). Fortunately, they *already* had a `.img` extension, unlike those from the combined system 6 `.sit` file, which had the `.image` extension:

```none
roms/
    sys605/
          Printing Tools.img
          System Tools.img
          Utilities 1.img
          Utilities 2.img
```

 and, at least, the "System Tools.img" was recognised. However, I still got the same issue that only the first disk was found, and multiple times - I had to manually insert the disks, via MAME's File Manager. However, this time, no checksum error was issued. 
 
Doing the same with [603](https://www.macintoshrepository.org/download.php?id=57511), I got the checksum error in the terminal and the truncate erros in MAME.

However, the command line was incorrect, sort of. If you wish to specify the entire "software pack", drop the `-flop1` parameter, as that specifies just one disk (`sys608:flop1`). Use instead:

```none
./mame macse sys605 -hard1 Mac80MB.chd -window
```

### Why the truncation error?

When using home-baked software packages (see **Roll your own...** above), why the error:

```none
Error, unable to truncate image: Invalid argument
```

As stated above, the truncation error could be possibly due to (slightly) different disks being used. For example, The 608 disks I obtained from a Macintosh `.sit` file that had to be expanded upon a Macintosh



## Emulated systems

### IBM 5150

Using 229 ROMs.

Requires `ibm5150.zip`.

Also requires:

 - `isa_hdc.zip`
 - `keytronic_pc3270.zip`

 Note: Does not boot.

### IBM 5160

Using 229 ROMs.

Requires `ibm5160.zip`.

Also requires:

 - ????

 Note: Does not work, missing ROM file

### IBM 5170

Using 229 ROMs.

Requires `ibm5170.zip`.

Also requires:

 - `at_keybc.zip`
 - `ega.zip`
 - `kb_pcat84.zip`

 Note: Corrupted ROMs?

### Apple 2

Using 229 ROMs.

Requires:

 - [`apple2`]()
 - [`a2diskiing`]()
 - [`d2fdc.zip`]()

Also requires:

 - `votrsc01a.zip` - which I found in the `Application Support` directory of [Ample](https://github.com/ksherlock/ample). It was *not* in the 229 ROM set.

As does `apple2e`, etc..

### Atari 400

Using 229 ROMs.

Requires `a400.zip`. No additional files required.

Note: ROM files require redump...

```none
co12499b.rom ROM NEEDS REDUMP
co14599b.rom ROM NEEDS REDUMP
```

### Atari 800

Using 229 ROMs.

Requires `a800.zip`. No additional files required.

Note: ROM files require redump...

```none
co12499b.rom ROM NEEDS REDUMP
co14599b.rom ROM NEEDS REDUMP
```

### Atari Cartridges

 - [BASIC ROM cartridge](https://www.atarimania.com/utility-atari-400-800-xl-xe-basic_13284.html) `BASIC_Revision_A.zip`, containing `BASIC - Revision A.car`.
   - May require `basica` or `basicc` containing `atari basic programming language (revision a).rom`
 - [Atari Pascal Language System](https://www.atarimania.com/utility-atari-400-800-xl-xe-atari-pascal-language-system_27758.html), 
 - [Assembler](https://www.atarimania.com/utility-atari-400-800-xl-xe-assembler_12525.html)
 - [Assembler](https://www.atarimania.com/utility-atari-400-800-xl-xe-assembler_36471.html)
 - [Assembler Editor](https://www.atarimania.com/utility-atari-400-800-xl-xe-assembler-editor_19777.html)


### BBC

#### Model B

Using 0229 ROMs.

Required `bbcb.zip`.

Also requires: `saa5050.zip`

Imperfectly emulated features: Graphics

#### Master

Using 0229 ROMs.

Requires `bbcm.zip`. No additional files required.

Imperfectly emulated features: Graphics

### Commodore 64

In addition to the obvious [c64.zip](https://archive.org/download/mame251/c64.zip), you will also need:

 - [c1541.zip](https://archive.org/download/mame251/c1541.zip)

 
### TRS-80

#### COCO

In addition to the not so obvious [coco.zip](https://archive.org/download/mame251/coco.zip), you will also need:


 - [bas12.rom](https://archive.org/download/mame-0.221-roms-merged/coco.zip/coco2%2Fbas12.rom) from [listing of coco.zip
](https://ia903204.us.archive.org/view_archive.php?archive=/29/items/mame-0.221-roms-merged/coco.zip)
 - [extbas.rom](https://archive.org/download/mame-0.221-roms-merged/coco.zip/coco2%2Fextbas11.rom)  from [listing of coco.zip
](https://ia903204.us.archive.org/view_archive.php?archive=/29/items/mame-0.221-roms-merged/coco.zip)
 - [disk11.rom](https://colorcomputerarchive.com/repo/ROMs/XRoar/CoCo/DOS/disk11.rom) from [TRS-80 Color Computer Archive](https://colorcomputerarchive.com/repo/ROMs/XRoar/CoCo/DOS/)

 `disk11.rom` place in `mame0256-x6/roms/coco/disk11.rom`.

#### COCO3

There is no `coco3.zip`, instead... but note that coco3 is *already* in `coco.zip`. 

In addition to the not so obvious [coco3_hdb1.zip](https://archive.org/download/mame251/coco.zip), you will also need:

 - [disk11.rom](https://colorcomputerarchive.com/repo/ROMs/XRoar/CoCo/DOS/disk11.rom) from [TRS-80 Color Computer Archive](https://colorcomputerarchive.com/repo/ROMs/XRoar/CoCo/DOS/)

 `disk11.rom` place in `mame0256-x6/roms/coco/disk11.rom`.???
 
 Trying to run coco3 says that it can not find `coco3.rom`:
 
 ```none
 coco3.rom NOT FOUND (tried in coco3 coco)
 ```
 
 But `coco3.rom` is *already* in `mame/roms/coco/coco3/coco3.rom`
 
 Trying to run `coco3-hdb1`, results in
 
 ```none
 Unknown system 'coco3-hdb1'
```

See **COCO is just a directory for packaging?** above for solution.

#### Coco2b

#### Coco3

#### Coco3p

### Game cube

Using 229 ROMs

Requires `gcjp.zip`. 

Resulting in serial clock error:

```none
Fatal error: :maincpu: PPC: serial clock (3686400) must not be more than half of the system clock (4850000)
```

#### Games - Turok

 - [Turko Evolution](https://www.romsgames.net/gamecube-rom-turok-evolution/)
 - [Turko Evolution](https://www.romspedia.com/roms/nintendo-gamecube/turok-evolution)


### Mac Plus

Trying this example from [Diagnosing missing media](https://docs.mamedev.org/usingmame/assetsearch.html#diagnosing-missing-media)

```none
./mame macplus -flop1 sys603:flop1
```

You need [macplus.zip](https://archive.org/download/mame251/macplus.zip) from [251](https://archive.org/download/mame251)

and for `341-0332-a.bin`

 - [`mackbd_m0110a.zip`](https://archive.org/download/mame251/mackbd_m0110a.zip) from [251](https://archive.org/download/mame251)

For the floppy disk(s):
 
 - [`sys603`]()
 - Software list `mac_flop`

I used this to install

```none
./mame macse -flop1 sys608:flop1 -hard1 Mac80MB.chd  -window
```
 


### PET

Using 0229 ROMs.

Requires `pet4016.zip` or `pet8032.zip`, both of which require additional files:

 - 4016
 
    ```none
   % ./mame pet4016 -window
    901468-14.uj1 NOT FOUND (tried in c4040 pet4016)
   901468-15.ul1 NOT FOUND (tried in c4040 pet4016)
   901468-16.uh1 NOT FOUND (tried in c4040 pet4016)
   901466-04.uk3 NOT FOUND (tried in c4040 pet4016)
   901467.uk6 NOT FOUND (tried in c4040 pet4016)
   901467.uk6 NOT FOUND (tried in c2040_fdc pet4016)
   ``` 


 - 8032
 
  ```none
 % ./mame pet8032 -window
901887-01.ul1 NOT FOUND (tried in c8050 pet8032)
901888-01.uh1 NOT FOUND (tried in c8050 pet8032)
OPTIONAL 901483-02.uk3 NOT FOUND (tried in c8050 pet8032)
OPTIONAL 901483-03.uk3 NOT FOUND (tried in c8050 pet8032)
OPTIONAL 901483-04.uk3 NOT FOUND (tried in c8050 pet8032)
OPTIONAL 901884-01.uk3 NOT FOUND (tried in c8050 pet8032)
OPTIONAL 901885-01.uk3 NOT FOUND (tried in c8050 pet8032)
OPTIONAL 901885-04.uk3 NOT FOUND (tried in c8050 pet8032)
901869-01.uk3 NOT FOUND (tried in c8050 pet8032)
901467.uk6 NOT FOUND (tried in c8050fdc pet8032)
```

Additional ROMs (from 0229):

 - For `901869-01.uk3`, `901887-01` and `901888-01`, try `c8050.zip`, see [listing of c8050.zip](https://ia903204.us.archive.org/view_archive.php?archive=/29/items/mame-0.221-roms-merged/c8050.zip)

 - For `901467.uk6`, try `c3040.zip`, see [listing of c3040.zip](https://archive.org/download/mame-0.221-roms-merged/c3040.zip/).
  - For some reason, this `901467.uk6` ROM is not seen, within `c3040/` or `c3040.zip`, even though it is there. This is because it isn't looked for there as the paths searched are: `901467.uk6 NOT FOUND (tried in c2040_fdc pet4016)` and `901467.uk6 NOT FOUND (tried in c8050fdc pet8032)`
  - Use `c8050fdc.zip` for 8032 (not `c8050.zip`)
  - Use `c2040_fdc.zip` for 4016 (not `c2040.zip`)

 - For `901466-04.uk3`, `901468-14.uj1`, `901468-15.ul1`, and `901468-16.uh1`, see [Comodore 4040](https://www.arcade-museum.com/tech-center/machine/c4040), so try `c4040.zip`.

Note: There may be (corrupted?) ROM issues for the 8032.

### QL

Using 0229 ROMs.

Requires `ql.zip`.

Note: There may be (corrupted?) ROM issues for the QL:

```none
% ./mame ql -window  
hal16l8.ic38 NOT FOUND (NO GOOD DUMP KNOWN) (tried in ql)
WARNING: the machine might not run correctly.
```

### ZX80

Using 0272 ROMs.

Requires `zx80.zip` only.

NOTE: Does not work!


### ZX81

Using 0272 ROMs.

Requires `zx81.zip` only.

### ZXSpectrum

Using 0229 ROMs.

Requires `spectrum.zip` only. No additional files required.

## Arcade

### 1 on 1

Using 229 ROMs.

Requires `1on1gov.zip`. Also requires `coh1002m.zip`.

### Altair 

Using 229 ROMs.

Requires `altair.zip` only.

### Asteroids

Using 229 ROMs.

Requires `asteroid.zip` only.

### Battlezone

Using 229 ROMs.

Requires `bzone.zip` only.

### Dig Dug

Using [** Deprecated ** Miyoo Mini Tiny Best Set](https://archive.org/details/miyoo-mini-tiny-best-set). It may be deprecates but the alternatives either require [logging in](https://archive.org/details/tiny-best-set-go) or [are too complex](https://archive.org/details/tiny-best-set-go-arcade-update_202305) to determine which package to download.

Requires `digdug.zip`.

Also:

 - `51xx.bin`
 - 53xx.bin`, use `nameco51.zip` and `nameco53.zp` (both from 272)

 Note: DigDug2 may have ROM dump issues
 
### Defender

 - [`defender.zip`](https://www.emurom.net/us/download/download-rom-17237-defender.red.label.html)
 - or 229 ROM pack (along with Scramble (which does not work) and Spyhunter (which requires `midscd.zip`)

### Firebird

Note: This is pinball. For video game, see **Space Firebird**

Using 229 ROMs.

Requires `firebird.zip`

Also requires:

```none
unzip: roms/firebird.zip couldn't find ECD
unzip: roms/firebird.zip couldn't find ECD
unzip: roms/firebird.zip couldn't find ECD
nsmf02.ic602 NOT FOUND (tried in firebird)
nsmf03.ic603 NOT FOUND (tried in firebird)
nsmf04.ic604 NOT FOUND (tried in firebird)
```

Note: The `nsm...` files are not included in many ROM packages, nor easily found in google.

### Frogger

Using 0251 ROMs.

Requires `frogger.zip` only.

### Galaxian

Using 0251 ROMs.

Requires `galaxian.zip` only.

### Invaders

Using 0229 ROMs.

Requres `invaders.zip` only.

### Lunar Lander

Using 0251 ROMs.

Requires `llander.zip` only.

### Mortal Kombat II

Using 229 ROMs.

Requires `mk2.zip`

Note: One and Three are `mk.zip` and `mk3.zip`.

### Phoenix

Using 229 ROMs.

Requires `phoenix.zip` only.

### Scramble

Using 0229 ROMs.

Requires `scramble.zip`.

### Sinistar

Using 0229 ROMs.

Requires `sinistar.zip`.

### Space demon

Using 0229 ROMs.

Requires `spacefb.zip`.

[Part of Space firebird](https://mdk.cab/game/spacedem) ROMs, run `mame spacedem`.

### Space Firebird

Using 0229 ROMs.

Requires `spacefb.zip`.

### Spyhunter

Using 0229 ROMs.

Requires `spyhunt.zip` and `midssio.zip` and `midcsd.zip`

### Star wars

Using 229 ROMs.

Requires `starwars.zip` only.

### Tempest

Using 0229 ROMs.

Requires `tempest.zip`.

#### Note:

This doesn't seem to work: [`tempest.zip`](https://www.romsgames.net/mame-037b11-rom-tempest/)

Interesting note: Even without the tempest.zip, MAME knows to look in `tempest` - how does it know that? Because, somewhere in the files, there is a pre-built list of supported ROMs, which mame can use to suggest mis-spelt packages.

The errors are the same, wheter the ZIP file is present or not. None of the files in the ZIP are the files in the list:

```none
136002-133.d1 NOT FOUND (tried in tempest)
136002-134.f1 NOT FOUND (tried in tempest)
136002-235.j1 NOT FOUND (tried in tempest)
136002-136.lm1 NOT FOUND (tried in tempest)
136002-237.p1 NOT FOUND (tried in tempest)
136002-138.np3 NOT FOUND (tried in tempest)
136002-125.d7 NOT FOUND (tried in tempest)
136002-126.a1 NOT FOUND (tried in tempest)
136002-132.l1 NOT FOUND (tried in tempest)
136002-131.k1 NOT FOUND (tried in tempest)
136002-130.j1 NOT FOUND (tried in tempest)
136002-129.h1 NOT FOUND (tried in tempest)
136002-128.f1 NOT FOUND (tried in tempest)
136002-127.e1 NOT FOUND (tried in tempest)
```

## Links

 - [Install Macintosh System 6](http://mess.redump.net/howto/install_mac_system_6) [wayback](http://web.archive.org/web/20160401214242/http://www.mess.org/howto:install_mac_system_6)
 - [MAME: Trying to Emulate System 6](https://www.emaculation.com/forum/viewtopic.php?t=11247)
 - [MAME: Documentatin](https://docs.mamedev.org/whatis.html)

### ROMs

Try to use ROM versions close to your version of MAME, in order to avoid incompatiblities. I am using MAME v.0261-x86.
 
  - [197](https://ia803104.us.archive.org/view_archive.php?archive=/12/items/MAME_0.197_ROMs_merged/MAME_0.197_ROMs_merged.zip) - Merged
  - ***[229](https://thepiratebay.org/description.php?id=41889167) - Merged ([magnet](magnet:?xt=urn:btih:3DC86FE0E8F1716250321432CC1360F75B8F00A1&dn=MAME%200.229%20ROMs%20(merged)&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce&tr=udp%3A%2F%2Ftracker.bittor.pw%3A1337%2Fannounce&tr=udp%3A%2F%2Fpublic.popcorn-tracker.org%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.dler.org%3A6969%2Fannounce&tr=udp%3A%2F%2Fexodus.desync.com%3A6969&tr=udp%3A%2F%2Fopen.demonii.com%3A1337%2Fannounce)) - Complete?***
  - ***[251](https://archive.org/download/mame251) - Merged***
  - [258](https://archive.org/details/mame_20250205) - Complete???
  - ***[272](https://archive.org/details/mame-0.272-romset-complete-merged) - ROMs in `mess/`***
  - [coco-221](https://ia903204.us.archive.org/view_archive.php?archive=/29/items/mame-0.221-roms-merged/coco.zip)
  - [many locked versions](https://archive.org/download/mame-chds-roms-extras-complete)
  - [recent random](https://archive.org/details/mame-rom-set-merged)
    - `college-brawl-1-4-1_archive.torrent` and `mame-rom-set-merged_archive.torrent`
  - [MAME 0.258 Complete: CHDs 935.53GB + ROMs 73.33GB + EXTRAs 64.97GB + Quick Tutorial / How to Guide](https://archive.org/details/mame_20250205)
    - This should be flagged as crap!
    - Source of the codex files: [codex-by-CodeXExecutor.net_archive.torrent	](https://archive.org/download/mame_20250205/codex-by-CodeXExecutor.net_archive.torrent)
    - Crap torrent [mame_20250205_archive.torrent](https://archive.org/download/mame_20250205/mame_20250205_archive.torrent)
      - Contains just sql and xml file
  - [MAME-VeryBestROMsExtended](https://thepiratebay.org/description.php?id=36512750)
    - version 223 (merged)
  - [mame-rom-set-merged]()
    - Contains just sql and xml file
    - Unknown source.
  
### Macintosh installation media

 - [System-6-x.sit](https://www.macintoshrepository.org/download.php?id=2969)
   - I got 6.0.8 from here
 - [605](https://www.macintoshrepository.org/download.php?id=57510)
 - [604](https://www.macintoshrepository.org/download.php?id=57512)
 - [603](https://www.macintoshrepository.org/download.php?id=57511)	
 
 
 
## See also

 - [About ROM sets](https://docs.mamedev.org/usingmame/aboutromsets.html)

 
## References

May be useful (but not read):

 - [Mac MAME Arcade emulation & NeoGeo using OpenEMU and SDLMame for Apple Silicon or Intel Native](https://blog.greggant.com/posts/2021/09/24/mame-on-mac-openemu-sdlmame.html#:~:text=There%20are%20two%20main%20routes,probably%20prefer%20the%20written%20version)
 - [Macintosh SE emulation using lr-mame (GNU/Linux)](https://forums.libretro.com/t/solved-macintosh-se-emulation-using-lr-mame-gnu-linux/44628)
 - [TRS-80 in Mess / Mame - No luck with Disks](https://forums.launchbox-app.com/topic/54899-trs-80-in-mess-mame-no-luck-with-disks/)
