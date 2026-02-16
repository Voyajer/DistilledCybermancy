# CUPS
## Canon Maxify GX2020 example
### Printing
I bought this printer specifically because it had refillable tanks and a scanner, unfortunately I didn't realize how little Canon cared for Linux.

install cups, avahi, and system-config-printer

````
sudo systemctl enable --now cups.service
sudo systemctl enable --now avahi-daemon.service
````
