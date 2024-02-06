<div align="center">
<h1>Born2BeRoot</h1>
<b>Born2BeRoot is one of the projects unlockable by the student after completing libft and it was my fourth 42 project</b>
<br></br>
	
![Static Badge](https://img.shields.io/badge/Score-%3100%2F100-green?style=for-the-badge&logo=42&labelColor=%23183A61&color=%23ffffff)
![Static Badge](https://img.shields.io/badge/Debian-green?style=for-the-badge&logo=Debian&labelColor=%23A81D33&color=ffffff)
![Static Badge](https://img.shields.io/badge/Virtual%20Box-green?style=for-the-badge&logo=VirtualBox&labelColor=%23183A61&color=%23ffffff)

[![Static Badge](https://img.shields.io/badge/Music%20Suggestion-Click%20Me?style=for-the-badge&logo=YouTube&logoColor=%23000000&labelColor=%23FFFFFF&color=%23FF0000)](https://www.youtube.com/watch?v=ASHG26YDsOs&pp=ygUMc2VtIHBhbGF2cmFz)

___

### OS

In this project you're allowed to choose between 2 OS, debian or rocky.

```
Debian:
Open source project oriented for daily usage and more user friendly
        since it provides great stability as an operating system for workstations and personal computers

Rocky:
Open source project oriented for entreprise usage and is often called an
      "open source version of Red Hat" and it has been in development since Red Hat discontinued CentOS
```
___

</div>

### Installing sudo

```
sudo (Superuser do) is a program used in UNIX systems that allows users to run commands
as root. It can also be used to run commands as other users in the system
```

`su -`: change your login to a root user

`apt-get update -y`: command that updates the apt, a package manager for debian systems

`apt-get upgrade -y`: command that upgrades every package already present

`apt install sudo`: installs a package that configures basic rules for allowing unpriviliged users to run commands as root or another user/group

<b>Now you need to set a user as sudo</b>

`usermod -aG sudo username`: set the user given to the sudo group

`visudo`: opens the sudoers config files

now lastly go to the end of the file at the #User Privilege section and add `username  ALL=(ALL) ALL`

after all this go ahead and reboot your machine with `reboot`

___

### Additional installs (optional)

`sudo apt install vim`: install Vi improved for a better experience when editing files

___

### How to create a group

`sudo groupadd group_name`: Create a group with the group name given 
___

### How to create a user and add it to a group

`vim /etc/passwd`: opens the file where you can check the existing users [if you're not a sudo it's read only]

`sudo adduser user_name`: creates a new user 

`sudo usermod -aG groupname username`: to add the user given to the group given

if the user doesn't appear in the group that you added him when you run `groups` just restart the machine
___

### Install and configure SSH

```
SSH (Secure Shell) is a network protocol that gives root users the ability to connect
to other computers through a safe and encrypted data communications using the SSH protocol
```

`sudo apt install openssh-server`: install the openssh package, a connectivity tool that we will use later

`sudo vim /etc/ssh/sshd_config`: opens the config file for the ssh server, here you have to change the line that says `#Port 22` ⇒ `Port 4242`

`sudo vim /etc/ssh/ssh_config`: opens the config file for the ssh client, here you have to change the line that says `#Port 22` ⇒ `Port 4242`

`sudo service ssh restart`: restart the ssh service and apply the changes made

___

### Install and configure UFW

```
UFW (Uncomplicated Firewall) is a program used for managing UNIX system firewalls
through simple commands and thanks to that minimizes the effort of managing a firewall
```

`sudo apt-get install ufw`: installs the UFW package

`sudo ufw enable`: enables the UFW

`sudo ufw status numbered`: check if the UFW is active or not

`sudo ufw allow [service/port]`: allows the service given through the firewall or opens up the port

you'll need to allow ↴

`sudo ufw allow 4242`: opens up the 4242 port

now if you input again the command to check the status of UFW you'll your rules added

___

### Connect your VM to your physical machine through ssh

```
						NAT VS Bridged Adapter

NAT makes your VM works as guest machine in the network, assigning it the same IP address as your physical machine,
thanks to that your VM is seen as the same device as your machine.

Bridged Adapter the VM uses the same physical adapters as your machine basically connecting to the network as a physical
machine by doing that the VM needs to be assigned an IP address making it possible to connect your physical
machine and VM through network services like SSH
```

first go to your Virtual Box select the current VM and go to settings ⇒ network ⇒ change the `Attached to` from `NAT` to `bridge adapter`

after that you'll need to change some settings in your virtual machine `/etc/network/interfaces`

`sudo vim /etc/network/interfaces`: will take you to a config file of the network interfaces

once you're in the file you'll need to add and edit some stuff

you can view your ip address with `hostname -I`

`allow-hotplug enp0s3` ⇒ `auto enp0s3`

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

```
pwquality is a PAM module that is used to perform password quality checks.
PAM are various libraries that allow multiple authentication mechanisms.
```

`sudo apt-get install libpam-pwquality`: to install the password quality check library

`sudo vim /etc/pam.d/common-password`: opens up the config file for the password quality check

on that file you'll need to edit the line that has the `pam_pwquality.so`, after you find that write right next to it this

`retry=3 minlen=10 ucredit=-1 dcredit=-1 lcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root`

```
retry: number of retries
dcredit: the maximum credit for having digits in the new password. 
ucredit: the maximum credit for having uppercase characters in the new password.
lcredit: the maximum credit for having lowercase characters in the new password.
maxrepeat: the maximum number of allowed same consecutive characters in the new password.
reject_username: doesn't allow the user to input his username in the password
difok: number of characters in the old password that must not be present in the new password
enforce_for_root: the module will return error on failed check even if the user changing the password is root
```

after this you can go ahead and save the document

`sudo vim /etc/login.defs`: opens up the config file for the shadow password suite

<b>in this file you'll need to edit a couple of lines</b>

`PASS_MAX_DAYS 99999` ⇒ `PASS_MAX_DAYS 30`: changes the expiry date of a password

`PASS_MIN_DAYS 0` ⇒ `PASS_MIN_DAYS 2`: changes the minimum amount of time until the user can change a password again

now save the file and leave vim and type `sudo reboot` to apply the changes

### Update main user password policy

```
All the users that we create after these steps will have these rules implemented but the user that we created at the beggining
will not have this policies estabilished so you need to run a couple of commands to update that
```

`sudo chage -l user`: to check the password policy for the user given

`sudo chage -M 30 user`: to set the expiration date for 30 days

`sudo chage -m 2 user`: to set the minimum amount of time until the user can change a password again


### Create a sudo log file

`cd /var/log`: sends you to the folder where the logs of the system are kept

`sudo mkdir sudo`: creates a folder named sudo where we will keep the folder of the sudo logs

`cd sudo && touch sudo.log`: sends you to the folder we just created and creates a file caled sudo.log

after that you will need to edit your sudoers file 

### More sudo configurations

`sudo visudo`: to open the sudoers config files

here you will need to add these lines bellow the others `Defaults`

```
Defaults        logfile="/var/log/sudo/sudo.log" //sets the log file to the one that we created
Defaults        badpass_message="Error, wrong password" //sets the password message when a sudo fails it
Defaults        passwd_tries=3 //sets the number of retries that the sudo has after inputing the wrong password
Defaults        log_input, log_output //adds to the log files inputs and outputs of files
Defaults        requiretty //turns on a security check
```

### Crontab configuration

```
Crontab is a program for UNIX systems that allows a specific set of commands to be ran in a given amount of time
```

`sudo apt-get install -y net-tools`: installs the package where crontab is included

`cd /usr/local/bin/`: opens the default installation location when a user builds and installs an executable application independently

`sudo touch monitoring.sh`: creates the file where we will write our script

`sudo chmod 777 monitoring.sh`: grants executable permissions to every user to the file that we just created

after that you can go ahead and copy this code to the file 

```bash
#!/bin/bash
upt=$(uptime -s | awk '{print $2}' | cut -d ":" -f2)
uptt=$(($upt % 10))
uptts=$(($uptt * 60))
upts=$(uptime -s | awk '{print $2}' | cut -d ":" -f3)
waitime=$(($uptts + $upts))
arc=$(uname -a)
pcpu=$(grep "physical id" /proc/cpuinfo | sort | uniq | wc -l) 
vcpu=$(grep "^processor" /proc/cpuinfo | wc -l)
fram=$(free -m | awk '$1 == "Mem:" {print $2}')
uram=$(free -m | awk '$1 == "Mem:" {print $3}')
pram=$(free | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')
fdisk=$(df -BG | grep "/dev/" | grep -v "/boot" | awk '{disk += $2} END {print disk}')
udisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} END {print ut}')
pdisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} {ft+= $2} END {printf("%d"), ut/ft*100}')
cpul=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')
lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')
lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -eq 0 ]; then echo no; else echo yes; fi)
ctcp=$(ss -neopt state established | wc -l)
ulog=$(users | wc -w)
ip=$(hostname -I)
mac=$(ip link show | grep "ether" | awk '{print $2}')
cmds=$(journalctl _COMM=sudo | grep COMMAND | wc -l)
sleep $waitime
wall "	#Architecture: $arc
	#CPU physical: $pcpu
	#vCPU: $vcpu
	#Memory Usage: $uram/${fram}MB ($pram%)
	#Disk Usage: $udisk/${fdisk}Gb ($pdisk%)
	#CPU load: $cpul
	#Last boot: $lb
	#LVM use: $lvmu
	#Connections TCP: $ctcp ESTABLISHED
	#User log: $ulog
	#Network: IP $ip ($mac)
	#Sudo: $cmds cmd"
```
`sudo chmod 744 monitoring.sh`: another user can't change it :)
after saving your script you need to open your sudoers config file with `sudo visudo` and add this line

`your_username        ALL=(ALL) NOPASSWD: /usr/local/bin/monitoring.sh`: this will allow monitoring.sh to be ran when our session starts

after this you'll need to reboot your VM and after that you'll need to run

`sudo /usr/local/bin/monitoring.sh`: this will execute the script we created as a sudo

`sudo crontab -u root -e`: this will open the crontab config file

on the end of that file write this

`*/10 * * * * /usr/local/bin/monitoring.sh`: this means that our script will run in 10 minutes intervals

___
# Extras
___
### How to edit hostname

`sudo vim /etc/hostname`: opens the file with the hostname, you can edit this to change the hostname

`sudo vim /etc/hosts`: opens the file that maps the IP addresses to the hosts, you can edit this to match the name on the `/etc/hostname`

### Check the existing groups

`getent group`: Used to check the groups that exist

`groups user_name`: Used to check to which groups the current user belongs to

##### Remove an UFW port

`sudo ufw status numbered`: to check what ports are open and their index number

`sudo ufw delete port_index_number`: to delete the port of the index number received
