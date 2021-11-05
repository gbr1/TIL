# Force screen resolution in Ubuntu 20.04

*This is a raw way to do it!*

Type:

`$``sudo nano /etc/default/grub`

Go to the line: `#GRUBGFXMODE=`, uncomment by removing `#` and put the desired resolution. Save the file and exit.

Type:

`$``sudo update-grub`

and then rebot:

`$``sudo reboot`






