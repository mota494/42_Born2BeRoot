<div align="center">
<h1>Born2BeRoot</h1>
<b>Born2BeRoot is one of the projects unlockable by the student after completing libft and it was my fourth 42 project</b>

___

![Static Badge](https://img.shields.io/badge/Score-%3F%2F100-green?style=for-the-badge&logo=42&labelColor=%23183A61&color=%23ffffff)
![Static Badge](https://img.shields.io/badge/Debian-green?style=for-the-badge&logo=Debian&labelColor=%23A81D33&color=ffffff)
<!--![Static Badge](https://img.shields.io/badge/Rocky-Green?style=for-the-badge&logo=Rocky%20Linux&logoColor=%23ffffff&labelColor=%2310B981&color=%23ffffff)-->
![Static Badge](https://img.shields.io/badge/Virtual%20Box-green?style=for-the-badge&logo=VirtualBox&labelColor=%23183A61&color=%23ffffff)

### OS

In this project you're allowed to choose between 2 OS, debian or rocky.

```
Debian:
Open source project oriented for daily usage and more user friendly
        since it provides great stabily as an operating system for workstations and personal computers

Rocky:
Open source project oriented for entreprise usage and is often called an
      "open source version of red hat" and it has been in development since Red Hat discontinued CentOS
```
___

</div>

### Installing sudo

```
sudo (Superuser do) is a program used in UNIX systems that allows users to run commands
as root. It can also be used to run commands as another users in the system
```

`su -`: change your login to a root user

`apt-get update -y`: command that updates the apt a package manager for debian systems

`apt-get upgrade -y`: command that upgrades every package already present

`apt install sudo`: installs a package that configures basic rules for allowing unpriviliged users to run commands as root or another user/group

<b>Now you need to set a user as sudo</b>

`usermod -aG sudo username`: set the user given to the sudo group

`visudo`: opens the sudoers config files

now lastly go to the end of the file at the #User Privilege section add `username  ALL=(ALL) ALL`

___

### Additional installs (optional)

`sudo apt install vim`: install Vi improved for a better experience when editing files

___

### Install and configure SSH

```
SSH (Secure Shell) is a network protocol that gives root users the ability to connect
to other computers through a safe and encrypted data communications using the SSH protocol
```

`sudo apt install openssh-server`: install the openssh package, a connectivity tool that we will use later

`sudo vim /etc/ssh/sshd_config`: opens the config file for the ssh server, here you have to change the line that says `#Port 22` ⇒ `Port 4242`

`sudo vim /etc/ssh/ssh_config`: opens the config gile for the ssh client, here you have to change the line that says `#Port 22` ⇒ `Port 4242`

`sudo service ssh restart`: restart the ssh service and apply the changes made

___

### Install and configure UFW

```
UFW (Uncomplicated Firewall) is a program used for managing UNIX system firewalls
through simple commands and thanks to that minimizes the effort of managing a firewall
```

`sudo apt-get install ufw`: installs the UFW package

`sufo ufw enable`: enables the UFW

`sudo ufw status numbered`: check if the UFW is active or not

`sudo ufw allow [service/port]`: allows the service given through the firewall or opens up the port

you'll need to allow ↴

`sudo ufw allow ssh`: allow the ssh service

`sudo ufw allow 4242`: opens up the 4242 port

now if you input again the command to check the status of UFW you'll your rules added

___

### Connect your VM to your physical machine through ssh

first go to your Virtual Box select the current VM and go to settings->network->change the `Attached to` from `NAT` to `bridge adpater`

after that you'll need to change some settings in your virtual machine `/etc/network/interfaces`

`sudo vim /etc/network/interfaces`: will take you to a config file of the network interfaces

once you're in the file you'll need to add and edit some stuff

`allow-hotplug eth0` ⇒ `auto enp0s3`

`iface enp0s3 inet dhcp` ⇒ `iface enp0s3 inet static`

```bash
address your_ip
netmask 255.255.0.0
gateway 10.11.254.254
dns 10.11.254.254
```

after this you can open a terminal in your physical machine and type

`ssh VMusername@VM_ip_address -p 4242`

and after a while you should have access to your virtual machine through your physical machine terminal

___

### Setup and configure a password policy
