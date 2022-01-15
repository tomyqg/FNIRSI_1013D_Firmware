# FNIRSI_1013D_Firmware
New firmware for the FNIRSI-1013D osciloscope.

This new firmware is offered without any warenty and I take no responsibility for any damage.

This repository is a result of the hacking of the original FNIRSI 1013D firmware. To make it easier to just get the new firmware code, this repository is created.

There are four folders with source code projects of which a minimum of two are needed to build a binary that can be loaded onto the SD card that is housed in the scope.

For a version with a startup screen that shows PECOs sCOPE three projects are needed:
1) "fnirsi_1013d_sd_card_bootloader" which loads the startup screen code and executes that
2) "fnirsi_1013d_startup_screen" which shows the startup screen and loads and executes the actual scope code
3) "fnirsi_1013d_scope" this is the actual scope code

For a version without hte startup screen only two projects are needed:
1) "fnirsi_1013d_startup_from_sd_card" which starts the FPGA and loads and executes the actual scope code
2) "fnirsi_1013d_scope" this is the actual scope code

The second option is the fastest since it does not wait to show the startup screen

To load the new firmware on the scope one has to make sure the SD card is partioned correctly.

1)  Connect the scope to the computer via USB.
2)  Turn on the scope and start the USB connection via the main menu option.
3)  Wait until the file manager window opens. (Only if auto mount is working properly)
4)  Close the file manager window.
5)  Open a terminal window (ctrl + alt + t) and type the "lsblk" command (!do not use the quotes!) and check which device the scope is on. (~8GB disk)
6)  Copy the files from the card to have a backup on your computer.
7)  Un-mount the partition. ("sudo umount /dev/sdc1" in my case)
8)  Just to be more safe make a backup with dd. ("sudo dd bs=4M if=/dev/sdc of=sd_card_backup.bin" again in my case)
9)  Open gparted and check if the device is properly formated. (Use right mouse and information to see the sector info)
10) If not delete the partition and make a new one leaving 1M free at the start. Format is fat32.
11) When the partition remounts after the previous step un-mount it again.
12) Use dd to place the backup package on the SD card. ("sudo dd if=fnirsi_1013d.bin of=/dev/sdc bs=1024 seek=8")
13) This will re-mount the partition. Un-mount the partition again. ("sudo umount /dev/sdc1" in my case)
14) Turn of the scope and turn it back on. This will start the new scope firmware

Removing the new firmware is easy:
1) Perform the first steps of the install. (1,2,3,4,5,7)
2) Remove the program with "sudo dd if=/dev/zero of=/dev/sdc bs=1024 seek=8 count=1"

For more information take a look here:
1) https://www.eevblog.com/forum/testgear/fnirsi-1013d-100mhz-tablet-oscilloscope/msg3807689/#msg3807689
2) https://www.eevblog.com/forum/testgear/fnirsi-1013d-100mhz-tablet-oscilloscope/msg3809966/#msg3809966
3) https://www.eevblog.com/forum/testgear/fnirsi-1013d-100mhz-tablet-oscilloscope/msg3908555/#msg3908555
