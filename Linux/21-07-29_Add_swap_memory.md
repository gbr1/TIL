# How add swap memory on RPI3B+

This method should work on any linux platform!

1. Disable swap:

`$``sudo dphys-swapfile swapoff`

2. Edit swap file:

`$``sudo nano /etc/dphys-swapfile` and edit `CONF_SWAPSIZE=1024` exit and save (ctrl+x -> y -> enter)

3. Make changes effective:

`$``sudo dphys-swapfile setup`

4. Enable swap

`$``sudo dphys-swapfile swapon`

Run `htop` to check memory.
