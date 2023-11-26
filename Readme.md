This is a modification of my Breakout boot sector game to test 1024 byte long sectors
I basically added 512 nop instructions so almost all of the code is in the upper 512 bytes of a 1024 byte long sector.
On boot from the floppy only the first 256 bytes of code are copied to 0xc000, the rest of the sector is somewhere else in memory.
That memory location is stored in a variable at 0xf351.
The start of the program copies all 1024 bytes from that location to 0xc000 then carries on.

To write the floppy I added this to my greaseweazle diskdefs.cfg:

```
disk msx1024.2dd
    cyls = 80
    heads = 2
    tracks * ibm.mfm
        secs = 4
        bps = 1024
        gap3 = 84
        rate = 250
    end
end
```

And wrote the .dsk image to the floppy with

`gw write --drive=b --format=msx1024.2dd 1024k.dsk`