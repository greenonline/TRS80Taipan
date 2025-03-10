
## Guides

 - This [post#3](https://68kmla.org/bb/index.php?threads/is-anyone-successfully-emulating-a-lisa.46324/#post-519202) on [Is anyone successfully emulating a Lisa?](https://68kmla.org/bb/index.php?threads/is-anyone-successfully-emulating-a-lisa.46324/)
 - This [post#8](https://www.applefritter.com/comment/45735#comment-45735) on [A working Lisa Emulator](https://www.applefritter.com/node/20185)

### Maybe useful for resolving disk issue?

 - [Re: LisaEm 1.2.7-RC3a support bug reports](https://lisalist2.com/index.php?topic=52.120)
   - Loooong thread!

## Process

Note: Running on Catalina

Create a folder in the Applications folder called LisaEm.

Download [Lisa Em](https://lisaem.sunder.net/downloads.html) (use `2022.04.01 LisaEm 1.2.7 Release Candidate 4`) and move to Applications/LisaEm.

Download [ROM](http://www.apple2.org.za/gswv/a2zine/System/A2_ROMsCollection.zip)

Rename 3410175H.bin to boot.hi and 3410176H.bin to boot.lo, and feed LisaEm with boot.hi. Note: Rename using Terminal and `mv` as OSX leaves a `.BIN` extension. However, once the `.BIN` has been removed, then both the `.LO` and `.HI` extensions are not visible in the Finder and both files appear as just `BOOT`. To allow you to select the correct ROM, in the LisaEm preferences, just copy over the `BOOT.HI` file first, to the LisaEm folder, and select that in the preferences, as it will be the only `BOOT` file present. Once the ROM has been selected in LisaEm preferences, then move the remaining `BOOT.LO` file in to the LisaEm directory.

Note: A `BOOT.ROM` file should have appeared in the LisaEm directory.

Note: From [this post](https://lisalist2.com/index.php/topic,354.msg2610.html?PHPSESSID=4tsnj193pj9n294pnl1eb5grtr#msg2610) on [LisaEm Broken](https://lisalist2.com/index.php?topic=354.0)

> Apparently, this is a two-step process. If the ROM is split into hi and lo, and you select bootrom.hi in the Preferences menu and attempt to boot, ROMless mode will still be used *until* LisaEm has joined the hi/lo ROM files into a single boot.ROM. I seem to recall this happening automatically upon closing Prefs, but it's inconsistent now. You can attempt to boot and get presented with the "ROMless Boot" window, then hit cancel. The ROM files will be joined, then subsequent boot successful

Download the [LOS files](http://www.applerepairmanuals.com/lisa/software/LOS_archives/).
Double click to expand the `.hqx`. Then use StuffIt 16 (OSX) to expand the resultant `.archive` files.

Otherwise you get 

```none
error message: DC42 volume name size wrong,or version not 0100
```

Power on (via the power button in the on screen image of the Lisa). 

Create a "10 MB (Any)" disk.

Note: A `lisaem-profile.dc42` file will appear in the LisaEm folder - this is the hard disk image.

Mount (or insert) the first floppy, by clicking on the floppy drive in the on screen image of the Lisa). In the Lisa menu "Startup from", select floppy (the first/top menu item). Install. The install will try to format the disk. Note: This takes a *looong* time, maybe five minutes or more. The machine has not hung (hopefully)..!

Then I get "Lisa could not write to the disk." error, which is reported on Macintosh Repository, [How to fix "Damaged disk problem" under LisaEm](https://www.macintoshrepository.org/articles/622-how-to-fix-damaged-disk-problem-under-lisaem), and suggests using instead:

 - [Lisa Office System 3.0](https://www.macintoshrepository.org/23273-lisa-office-system-3-0)

This set (the `.zip` file), from Macitosh Repository, fixed the image for me. Again, insert the first disk, `LOS 3.0 1.image`. Once the hard disk was formatted, the software installed, *sans probleme* (I spoke too soon!). Insert the remaining four disks, as required. Note that the second floppy install takes a long time, and doesn't really finish...

Upon, re-attempts, I often got the "can not write message" a few more times. Even if I delete the old disk image and start a-fresh. Is it due to LisaEm being put in the background? I suspect that the application needs to remain in the foreground. Nope, even keeping in the foreground, even the first floppy of the new 3.0 set still fails, whereas it had got past the first floppy, the first time that I tried the new set..! There seems to be an inconsistent bug.

I also see "Danger! Both the Data and Tag checksums failed for this disk!" message dialogs, when inserting a floppy (especially after a hung or rudely quit/powered off Lisa) - has the floppy become corrupted?

Also see [Re: LisaEM crashes with ipc=NULL](https://lisalist2.com/index.php/topic,417.msg3059.html?PHPSESSID=tr0cv91je39c4s51ptqu76gk1v#msg3059), mentions the "can not write to the disk" issue due to AntiVirus (on Windows?)

Maybe corrupted floppies, [Bringing My Apple Lisa Back to Life](https://lowendmac.com/2007/bringing-my-apple-lisa-back-to-life/). Unzip again and retry... Nope, keeping in foreground and using new floppy 3.0 set, still the format fails.

## See also

 - [Lisa articles](https://www.macintoshrepository.org/article_search.php?c=58)

## Other versions

I tried MAME, but the ROMs seem to be corrupted. Is it possible to use the ROM created by LisaEm?

From [post#18](https://www.applefritter.com/comment/46611#comment-46611), there are three emulators, created in the following chronological order: MAME, [IDLE](http://idle-lisa-emu.sourceforge.net/) and LisaEm