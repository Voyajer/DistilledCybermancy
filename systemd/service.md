### Services in systemd





# Example: "mangosd.service"

---
[Unit]
Description=mangosd
After=network.target mysql.service

[Service]
ExecStart=screen -Dms mangosd /opt/cmangos/run/bin/mangosd
WorkingDirectory=/opt/cmangos/run/etc
Restart=always

[Install]
WantedBy=default.target
---
