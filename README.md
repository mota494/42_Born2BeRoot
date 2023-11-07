<div align="center">
<h1>Born2BeRoot</h1>
<b>Born2BeRoot is one of the projects unlockable by the student after completing libft and it was my fourth 42 project</b>

___

![Static Badge](https://img.shields.io/badge/Score-%3F%2F100-green?style=for-the-badge&logo=42&labelColor=%23183A61&color=%23ffffff)
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

`sudo apt install openssh-server`: install the openssh package, a connectivity tool that we will use later

`sudo vim /etc/ssh/sshd_config`: opens the config file for the ssh server, here you have to change the line that says `#Port 22` -> `Port 4242`

`sudo vim /etc/ssh/ssh_config`: opens the config gile for the ssh client, here you have to change the line that says `#Port 22` -> `Port 4242`
