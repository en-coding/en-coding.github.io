
# Ubuntu Server 20.04 LTS

### Eijy Nagai

2022-04-22

Documentation for installing Ubuntu Server in a HPE Proliant ML350p 8Gen workstation.



Bioinformatics research involves large datasets analyses therefore a huge computational power. While you can analyse several types of datasets in your own laptop, a larger number of samples or heavier datasets will demand more power. Old PC from a former lab member is often seen accumulating layers of dust at bioinfo labs. Instead of letting that valuable resource becoming little by little a piece of expensive trash, you can ask permission to use it if no one will use and previously backuped.

It can become your own analysis server, in which you can have full control as sys admin and install anything you wish that you can't in usual big sharable servers.  



Ubuntu Server is one of most popular servers and I found an interesting guide [book](https://www.packtpub.com/product/mastering-ubuntu-server-third-edition/9781800564640) to mentor myself towards becoming a (hopefully) sys admin!



Here I spread the word to help you make your own server. Later, I will share how to configure accounts, install popular tools, environment management, container uses, and so on. As for now, let's set our own server.



## 1 Operational system instalation

[Download]([Get Ubuntu Server | Download | Ubuntu](https://ubuntu.com/download/server)) and make a bootable DVD or [USB drive](https://www.balena.io/etcher/).

Insert the media into the computer and turn on pc. If it doesn't go to the installation screen automaticaly you need to reorder the boot. Google <your PC name> + "boot key" to find the right key to press during initialization. In my case, I had to press F8 and reorder the boot preferences.

Install the system as recommended or you can make partitions as you wish. Do no worry about static IP, ssh, or other configurations yet as we will do it later. Include a name for your server, your adminstrator login and password.

When the installation finishes, remove the media and hit enter to reboot.



## 2 Initial configuration

If there is any problem with a cloud-init search in the first time you boot, use the following command to disable the cloud-init network. We do not use cloud so it is not necessary. You can copy and paste the following command as it will create a file in the cloud.cfg.d directory. This file does not exist yet.

`sudo nano /etc/cloud/cloud.cfg.d/subiquity-disable-cloudinit-networking.cfg` or

`sudo nano /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg`



Then type the following line. Hit `Ctrl`+`x`, save with `Y` and enter. 

`network:{config:disabled}`



##### updates:

I found another [reference]([How to remove cloud-init from Ubuntu Server 20.04 - NetworkReverse.com](https://www.networkreverse.com/2020/06/how-to-remove-cloud-init-from-ubuntu.html)) explaning an easier method. Just create an empty file in the /etc/cloud directory as show below.

`touch /etc/cloud/cloud-init.disabled`



You can reboot and check if the message apears.



## 3 Static IP configuration

Next, we can configure the static IP to connect remotely to the server. 

Before changing the IP, you can check if the ethernet port is available by the command `ip link` or `ip a`



You will be able to see 2 or more devices. The first is lo, and the others name may differ based on your system. In my computer, there are 4 ports: eno1, eno2, eno3, eno4 and I connected the cable into the eno1.

![](/Users/eijy/Library/Application%20Support/marktext/images/2022-04-22-14-58-45-image.png)



You need to have in hands the following information:

1. IP address

2. Gateway

3. DNS server



Then, open the configuration file:

`sudo nano /etc/netplan/00-installer-config.yaml`



If you have many ports that you are sure you're not using later, you can just remove everything and leave only the port 

<img src="file:///Users/eijy/Library/Application%20Support/marktext/images/2022-04-22-15-25-25-image.png" title="" alt="" width="506">  

Here, address corresponds to `IP address`, and addresses under nameservers is the `DNS server` IPs. The number `/24` in front of the IP adress is the port to be used. 



One important point is to keep the identation as tidy as possible, otherwise it will raise identation errors later. For reference, I use 3 spaces for a identation.

After finished, hit `Ctrl`+`x` and `Y` to save.



Then you can type `sudo netplan apply` to update your configurations and `ip a` to see the new address.



Most users may connect directly to their server by a ethernet LAN cable connection. But if you have a bigger system and need to connect to another server, you need to ask the IP of that server and use it as `gateway` .



## 4 Remote access

The next step is to install `ssh` in your operational system.

`sudo apt install ssh`

or

`sudo apt-get install openssh-server`



Then enable the use of ssh.

`sudo systemctl enable ssh`



Next we activate ssh.

`sudo systemctl start ssh`



We can check how is the status

`systemctl status`



Also it is helpful to check if you have internet connection as well by testing the latency:

`ping www.google.com` 



If you have done everything correct you might be able to access from a remote computer using the command:

`ssh youruser@192.168.0.1` 



You wil be asked to type the admin password you set in the installation. 



That's it! Congratulations, you succesfully installed Ubuntu Server 20.04 LTS and configured the static IP.





## Useful commands

`lsblk` #partitions and volume

`sudo fdisk -l` # detailed information of partitions

`sudo reboot` or `sudo systemctl reboot` 

`ip link` or `ip a` # to see the machine ports and IP










