#PIGame Cocktail Install

##Image SD card on a Unix Box

Use df to determine what drive we wish to wipe

	> df
	Filesystem		512-blocks Used  		Available Capacity  iused    ifree %iused  Mounted on
	/dev/disk1  	487830528  393417552   93900976    81% 49241192 11737622   81%   /
	devfs			406        406         0   	100%      703        0  100%   /dev
	map -hosts		0          0          	0   100%        0        0  100%   /net
	map auto_home	0          0          	0   100%        0        0  100%   /home
	/dev/disk7s1	31100      23184       7916    75%      512        0  100%   /Volumes/NONAME
	/dev/disk7s2	15217064   15154696    62368   100%        0        0  100%   /Volumes/SDM
	
In our case these two partitions are assocated with the SD card, first we need to unmount them, so that's the device `/dev/disk7` remember that for later.

**OSX Unmounting**

`sudo diskutil umount /Volumes/NONAME`

and

`sudo diskutil umount /Volumes/SDM`

**Linux Unmounting**

`sudo umount /Volumes/NONAME`

_Although they won't be mounted under volumes, and I assume if you're using Linux you already know this_

###Using dd to image
	
`sudo dd bs=1m if=~/Downloads/piplay-0.8-beta7.img of=/dev/disk7`

_where disk7 is the disk we just unmounted (the __s1__ and __s2__ correspond to partitions, you don't want to write to the partition)_

##Rotate Screen

Rotating the entire screen causes problems for advancemame, fortunatly we can fix these up.

`sudo nano /boot/config.txt`

add 

` display_rotate=1`

_ctrl-X_ exits then _Y_ to save

then use

`avcfg`, `avv` to fix up your issues after rotation, In `avv` you will have to add some custom display resolutions that match your back to front width and height.

##Give a little more to the GPU

GPU Memory 256

##Add Your ROMS

###Using web interface

pigame has a very nice web interface for adding ROMS just go to the ip address in your webbrowser thats listed on the top of the screen.

###Using SCP

If you have a mass of ROMs that you wish to copy across and honestly can't be bothered with the manual process involved in using the webbrowser _(I don't blame you)_ then you could use SCP upload your games.

`/home/pi/pimame/roms` contains the following directories for ROMs

	2600     c64  gba      gg        neogeo  ngpc   psx      sms   tg16
	advmame  gb   genesis  mame4all  nes     pifba  scummvm  snes
	
####Windows

[Download pscp](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) place it in your path (copy it into \Windows\System32) then issue `pscp` and not `scp` in the following commands.

or

[Download winscp](http://winscp.net/eng/download.php) and use it's nice GUI to copy your files.

####Unix

Issue the following command to copy your ROMs to the correct place.

`scp -r <localdirector> pi@ipaddress:/home/pi/pimame/roms/<platform>`

when prompted for a password its _raspberry_


#Retrogame Cocktail Install

##Rotate the screen

Rotating the entire screen causes problems for advancemame, fortunatly we can fix these up.

`sudo nano /boot/config.txt`

add 

` display_rotate=1`

_ctrl-X_ exits then _Y_ to save

###Issues

The interface nolonger quite fits on the screen, let's check if we can fix that later.

##Install AdvanceMame

`cd /home/pi/RetroPie-Setup` 

then run 

`sudo ./retropie_packages.sh 101` 

Have a cup of tea, the RaspberryPI is compiling AdvancedMame, this could take a while

essystems.conf

##Image your disk because that was painful

`sudo dd if=/dev/disk3 of=MyRetroPie.img`

Go make a coffee again.

##Other Stuff.

###Faster Scraping

A bug in emulationstation prevents scraping without user input when the screen is flipped, When you want to scrape your game collection you must first adjust the setting in `/boot/config.txt` and comment out `display_rotate=1` then change the setting back once scraping has been completed.

###Finding wiring information on Game Controller

Anyone reading these insructions possibly will not have this issue but, if you don't know anything about the game controller you have bought or the wiring diagram I suggest the following.

My game controller either did not come with a wiring diagram, or I lost it, to find out what model the controller is run `lsusb` on the PI then search the internet for a wiring diagram.

 
