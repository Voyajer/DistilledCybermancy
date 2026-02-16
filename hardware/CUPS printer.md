# CUPS
## Canon Maxify GX2020 example
### Printing
I bought this printer specifically because it had refillable tanks and a scanner, unfortunately I didn't realize how little Canon cared for Linux.

install cups, avahi, capt-src, and system-config-printer

````
sudo systemctl enable --now cups.service
sudo systemctl enable --now avahi-daemon.service
````
Run driverless
````driverless````
Then run the output as an argument with the output into a file:
````driverless "ipps://Canon%20GX2000%20series._ipps._tcp.local" > ~/canon_gx2020.ppd````

go to http://localhost:631/help/overview.html

click on the Administration tab and log in with your root account and password.

Add printer, select the printer (Canon_GX2000_series_4.020). select whatever mentions driverless, or IPP everywhere if driverless does not work.



And I can't fucking recreate the process on my other computer to document it... fuck off canon...

### Scanning

install sane, sane-airscan, and simple-scan

In simple-scan you can click on the scan button to scan an image on the printer and save on your computer.
