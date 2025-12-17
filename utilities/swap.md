#make swap, 16GB

sudo dd if=/dev/zero of="swap location" bs=1G count=16 status=progress

sudo mkswap "swap location"

sudo chmod 6000 "swap location"
sudo chmod 0600 "swap location"

#turn swap on/off

sudo swapon "swap location"

sudo swapoff "swap location"

#check swap status and priorities

swapon -s

#change swappiness, 60 default, higher swaps more easily

sudo sysctl vm.swappiness=60

#temporarily change swap priority, higher is higher, same is use both at same time

sudo swapoff "swap location" && sudo swapon -p {number} "swap location"

#permanently, in fstab:

# /swapfile
/swapfile               none    swap    nofail,pri=10   0       0
