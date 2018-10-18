# ubuntu-server
Guideline


# Making the Switch from RHEL to Ubuntu Server?
# I needed to start supporting Ubuntu 12.0.4 Servers after three years of working with Scientific Linux 5/6 (RHEL-variant)
# This is the cheatsheet I put together to help get over some of the differences
# Last Edit 20130508

_Root_
The root account is not enabled by default. Ubuntu wants to to use sudo to elevate privileges. The first created account will be an administrator in the sudoers group.

Config file: /etc/sudoers (use visudo)

Further information:
https://help.ubuntu.com/community/RootSudo
https://help.ubuntu.com/community/SwitchingToUbuntu/FromLinux

_Shell_
Bash is still the default login shell, but dash is used for running things. As such, scripts that call #!/bin/sh might 
fail. Use #!/bin/bash

Further information:
https://wiki.ubuntu.com/DashAsBinSh

_Networking_
Networking configuration file: /etc/network/interfaces
Set hostname: /bin/hostname [name]
Set DNS: /etc/resolv.conf

Further information:
https://help.ubuntu.com/12.04/serverguide/network-configuration.html

_Kernel|OS Security_
RHEL has SELinux. Ubuntu uses AppArmor.

Further Information: 
https://en.wikipedia.org/wiki/AppArmor
https://wiki.ubuntu.com/AppArmor

_Installing|Updating Software_
RHEL uses yum and rpm (both command and package format). Ubuntu uses apt-get (or aptitude), dpkg and the .deb package format. n.b. Unlike yum, apt-get can't install local packages, use dpkg instead.

<action>					<yum|rpm command>			<apt*|dpkg command>
Refresh available packages			—					apt-get update		
Search for packages				yum search [name]			apt-cache search [name]		
Get package info				yum info [name]				apt-cache show [name]		   	
Install a package from repo			yum install [name]			apt-get install [name]
Install from package				yum install /path/to			dpkg --install /path/to
Remove a package				rpm -e					apt-get remove [name]
                                                            
Check for package upgrades			yum check-update			apt-get -s upgrade
Upgrade packages				yum update				apt-get upgrade

List all installed packages			yum list installed			dpkg --list
						(rpm -qa)               		   
Info about installed package			rpm -qi [name]				dpkg --status  [name]
List files in installed pkg			rpm -ql  [name]				dpkg --listfiles  [name]
List config files in install pkg		rpm -qc  [name]				dpkg-query --show -f '${Conffile}\n'  [name]
Get info about package file			rpm -qpi [name]				dpkg --info [name]
List files in a package file			rpm -qpl [name]				dpkg --contents [name]

Show package dependencies			rpm -qR  [name]				apt-cache depends
Show reverse dependency				rpm -q -whatrequires [name] 		apt-cache rdepends
Find package that installed file		rpm -qf filename			dpkg --search filename
Find package that providess file		yum provides filename			apt-file search filename

Verify all installed packages			rpm -Va					debsums
Clear local cache				yum clean packages			apt-get clean
Clear only obsolete from cache			—					apt-get autoclean
Upgrade system					yum upgrade				apt-get dist-upgrade

Repo configuration				/etc/yum.conf				/etc/apt/sources.list


List of yum and rpm commands and their apt-get and dpkg equivalents: 
https://help.ubuntu.com/community/SwitchingToUbuntu/FromLinux/RedHatEnterpriseLinuxAndFedora

Expanded list comparing apt*|dpkg|aptitude commands to yum|rpm commands: 
http://www.jpsdomain.org/linux/apt.html


_Runlevels_
Ubuntu defaults to runlevel 2 (multi-user console login) rather than runlevel 3 or 5 like RHEL. 
Runlevel 1 (single-user mode), 0 (halt) and 6 (reboot) are the same in both OSes.

Use runlevel to get the current runlevel setting. tellinit will set the runlevel

Key man pages:
http://manpages.ubuntu.com/manpages/lucid/man7/runlevel.7.html
http://manpages.ubuntu.com/manpages/lucid/man8/telinit.8.html

Further information:
https://en.wikipedia.org/wiki/Runlevel#Ubuntu
https://help.ubuntu.com/community/UpstartHowto

_Working with Services_
Ubuntu 9 and up uses Upstart to manage the boot process and services. Upstart will recognize instructions from both the service command and the initctl command. Not all applications and services have been updated to work with initctl and some have been updated to only work with initctl. As such, some services and applications will only respond to commands issued with service (e.g. apache2), some will only respond to commands from initctl and others will respond to both (e.g. ssh).

Initctl also has several shortcuts that can be used: status|restart|stop [process]
You can also use the following: /etc/init.d/[service name] status|start|stop to control certain processes

OS Upstart config scripts live in /etc/init
Application and service Upstart config scripts live in /etc/init.d

ckckconfig is not installed by default. The closest equivalent is update-rc.d, but it is rather awkward to use. chckconfig can be installed via apt-get. Another alternative is sysv-rc-conf, a curses-like utility to set process run levels.

Key man pages:
http://manpages.ubuntu.com/manpages/natty/man8/initctl.8.html
http://manpages.ubuntu.com/manpages/hardy/man8/service.8.html
http://manpages.ubuntu.com/manpages/lucid/man8/update-rc.d.8.html

Further information:
https://help.ubuntu.com/community/UbuntuBootupHowto
https://help.ubuntu.com/community/UpstartHowto'
https://wiki.ubuntu.com/InitScriptHumanDescriptions
http://upstart.ubuntu.com/getting-started.html
http://askubuntu.com/questions/19320/whats-the-recommended-way-to-enable-disable-services
http://sysadvent.blogspot.com/2010/12/day-19-upstart.html
http://askubuntu.com/questions/2263/chkconfig-alternative-for-ubuntu-server

_Useful Official Sites_
Man pages: http://manpages.ubuntu.com
Documentation: https://help.ubuntu.com
Help Wiki: https://help.ubuntu.com/community/TitleIndex
Releases:  https://wiki.ubuntu.com/Releases
Bugs: https://wiki.ubuntu.com/BugSquad/KnowledgeBase
Server FAQ: https://help.ubuntu.com/community/ServerFaq

_Third-Party Info Sites_
Releases: http://en.wikipedia.org/wiki/List_of_Ubuntu_releases
Misc: http://serverfault.com/questions/350881/what-does-a-ubuntu-server-admin-need-to-know-about-centos
