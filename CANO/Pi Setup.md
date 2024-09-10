rpi-imager
sudo apt install rpi-imager\n
Debian Bookworm (the first one that appears on the list)\n
When done imaging, insert SD into the pi\n
Use USB-C to hook up the pi to the battery power\n
Connect pi to GLiNet router via ethernet\n
Go to web browser and type 192.168.8.1\n
Identify the IP of the pi using the clients tab\n
After identifying the IP of the pi, ssh into the pi (ssh pi@IPADDRESS)\n
password = raspberry (or what you set in the imager)\n
Update and upgrade software from Raspbian\n
sudo apt update ; sudo apt upgrade -y\n

Configure the Operating System\n
sudo raspi-config\n
1-S3 Change Password\n
1-S4 Change Hostname\n
3-I1 Enable SSH\n
5-L1 Set Locale (set US for both)\n
5-L2 Change Timezone 
5-L4 Set WiFi Country (Japan)
6-A1 Expand Filesystem
6-A2 Interface Names (No)

Finish
Reboot

On laptop Create SSH keys that can be used for secure login to the pi (ensure that the path to the file created matches the filepath in the prompt) ***DONT INPUT A FILENAME***
ssh-keygen -b 4096
Transfer the keys to the pi:
ssh-copy-id pi@192.168.8.196(IPADDRESS)
Test the ssh key to ensure you can now ssh without being prompted for a password
ssh pi@192.168.8.196(IPADDRESS)
make a backup of the key in case you accidentally delete the original
cp ~/.ssh/id_rsa(FILENAME) id_rsa.bak(FILENAME.bak)

on the pi
we can modify the ssh configuration to harden it and make the remote more secure
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo vim /etc/ssh/sshd_config
make the following modifications: (delete the # that comments the lines out)
AddressFamily inet
PermitRootLogin no
PasswordAuthentication no

restart the ssh Daemon to read the changes you just made:
sudo systemctl restart sshd

On pi
Install wireguard program
sudo apt install wireguard resolvconf -y

On laptop 
send the wireguard config that you created to the pi
sudo scp -i ~/.ssh/id_rsa(PREVIOUSFILENAME(keygenfile)) /etc/wireguard/pi-wg0.conf pi@192.168.8.196(IPADDRESS):~


