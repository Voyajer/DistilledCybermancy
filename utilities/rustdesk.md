#rustdesk

###install rustdesk server:

https://youtu.be/49EfaA5vRbY?si=Cyz8XU5LasnL4DRx&t=410

open ports
- 21115-21119 TCP
- 21116 UDP
- 8000 TCP

`wget https://raw.githubusercontent.com/techahold/rustdeskinstall/refs/heads/master/install.sh`

`chmod +x isntall.sh`

`./install.sh`

Follow prompt. IP, no.

Save the public key it generates.

###install rustdesk client:

`rustdesk-bin`

Once installed, go to settings>network and add server public ip to ID and Relay servers, and the public key in the Key field.

