# How add user to dialout group



On linux if you are using serial ports, you may have `Permission denied` issue on serial opening.

Suppose that your serial is `/dev/ttyUSB0`, check dialout by typing

```bash
ls -l /dev/ttyUSB0
```

If response is `dialout 188` , your user haven't got any permission to open it.

To avoid that, you need to add it to dialout:

```bash
sudo usermod -a -G dialout $USER
sudo reboot
```

If you want to check, now dialout must be `166`
