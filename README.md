# deb7
SWGEmu Development Environment setup for Debian 7 OS

VirtualBox, VMWare, or native install. Edit as needed  
https://www.virtualbox.org/wiki/Downloads  
https://my.vmware.com/web/vmware/free#desktop_end_user_computing/vmware_player/7_0
	
I used:

	8g mem
	32g virtual drive
	max cores
	bridged network

# Install Debian 7 
https://www.debian.org/distrib/

	(net install) 64 bit 
	Should work on any debian package distro
****************
	username=swgemu  
	password=123456
	root pw=12345678
****************
Predefined software selections - Default selection

	(*)Debian Desktop
	(*)Print Server
	(*)Standard System Utilities
****************
Config sudoer as needed

Run all CLI commands as sudo with user 'swgemu'. 

https://www.digitalocean.com/community/tutorials/how-to-add-delete-and-grant-sudo-privileges-to-users-on-a-debian-vps

We give users access to the sudo command with the visudo command. If you have not assigned additional privileges to any user yet, you will need to be logged in as root to access this command:

	visudo
	
When you type this command, you will be taken into a text editor session with the file that defines sudo privileges pre-loaded. We will have to add our user to this file to grant our desired access rights.

Find the part of the file that is labeled "Allow members of group sudo to execute any command". It should look something like this:

	# Allow members of group sudo to execute any command
	%sudo   ALL=(ALL:ALL) ALL

	
We give a user sudo privileges by copying the line beginning with "%sudo" and pasting it after. We then change the user "%sudo" on the new line to our new user, like this:

	# Allow members of group sudo to execute any command
	%sudo   ALL=(ALL:ALL) ALL
	%newuser   ALL=(ALL:ALL) ALL
	
We can now save the file and close it. By default, you can do that by typing Ctrl-X and then typing "Y" and pressing "Enter".
	
=====================
# Import scripts  
=====================
Copy and paste this into a terminal to Download and install these scripts. System will reboot. (This is the 'bang' script.)

	cd && sudo apt-get update && sudo apt-get install -y -q git && git clone https://github.com/Scurby/deb7.git && mkdir bin && cp -i /home/swgemu/deb7/bin/* /home/swgemu/bin/ && mkdir setup && cp -i /home/swgemu/deb7/setup/* /home/swgemu/setup/ && mkdir run && cp -r /home/swgemu/deb7/run/* /home/swgemu/run/ && chmod -v +x /home/swgemu/bin/* && sudo /sbin/reboot


***********

Copy all files/folders into home folder

/bin/ - place all shell scripts here - set as read/write/executable
	TODO - Be sure that is executable:
	chmod +x /path/to/script

/run/ - place run_gdb here

/run/conf/ - should be empty.

/setup/ - eclipse tarball and Egit-prop tarball here

***********

Code:

	mkdir bin
	cp -r /home/swgemu/deb7/bin/* /home/swgemu/bin/
	mkdir setup
	cp -r /home/swgemu/deb7/setup/* /home/swgemu/setup/
	mkdir run
	cp -r /home/swgemu/deb7/run/* /home/swgemu/run/
	chmod -v +x /home/swgemu/bin/*

=====================
# Restart
=====================
RESTART!!!

=====================
# Run setup scripts
=====================
The following shell scripts can be run from the command line. They are numbered in the order I use them.

1. options - Installs Optional packages

	- xclip 
	- terminator 
	- vim 
	- chromium 
	- quassel
	- first;
        
2. first - Installs required packages and programs

	- gcc, g++, git, gdb, automake, make, libreadline-gplv2-dev
	- libncurses5-dev, libneon27, libaprutil1-dev, libtool
	- openjdk-6-jre, openjdk-6-jre-headless, libgtest-dev, screen
	- Lua-5.1 - Berkely DB 5.0 - MySQL Server and Workbench
	- start;

3. start - Initial setup of development environment

	- Choose editor
	- Setup git user.* config
	- Setup ssh key
	- Register on gerrit
	- Test Gerrit setup
	- Clone repos and checkout a local branch of Core3 origin/unstable
	- Symlinks (idlc)
	- Engine library
	- MySQL database checks
	- Server configuration
	- Tre files
	- build config; run_dev;

4. build - simple build script

	- build config does 'make config' and 'make clean'
	- build clean does 'make clean'

5. run_dev - Build and run the development server and launch it under gdb on a 'screen'

	***NOTE: run_dev uses gdb in batch mode and starts with the commands
	in ~/run/run_gdb which you can change to your pleasing;
	(breakpoints, dumps, settings etc.)

**************************************************************************************
# The following scripts are also useful...

ack - Nice source grep tool (try: cd ~/workspace/MMOCoreORB/src; ack PlanetManager)

myip -  display the ip of the VM and login port for quick configuration of the windows client

updateip - Get ip address of local eth0 and update galaxy table in MySQL as needed

latest - do a quick git-stash, git-pull, and git-stash-apply so you can get to the latest code w/o loosing local work

freeze - Save your devenv state so you can repeat the same tests over and over

thaw - allow server to continue from previous state each time you run it

installed - Package and version check sent to /home/*

**************************************************************************************
*FIXME* *FIXME* *FIXME*

openfile {filename} - open file in eclipse *FIXME*

eclipse - install eclipse, import project and set git properties. *FIXME*
	(Requires Egit-properties.tar.gz in /home/setup/

**************************************************************************************
**************************************************************************************

===============
# Eclipse- Not complete
http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/kepler/SR2/eclipse-cpp-kepler-SR2-linux-gtk-x86_64.tar.gz&mirror_id=272
---------------
*Install Eclipse*

	Base install - CDT required
	http://eclipse.org/downloads/packages/release/Kepler/SR2.
	Place tarball in /home/setup/ for 'ide' script. Edit ide script file name as needed

**************************************************************************************
---------------
***How to Upgrade Eclipse on the Development VM***

	***Thanks Valkyra***

Close Eclipse. Download http://www.eclipse.org/downloads/dow...PACKAGENAME.tar.gz to ~/Downloads

Start a console session and type the below commands one after each other;

	cd Downloads
	tar -xvf eclipse-cpp-PACKAGENAME.tar.gz
	mv ../eclipse ../eclipse-old
	mv ./eclipse ../eclipse

Now open eclipse again as normal and select workspace will come up, just choose 'OK'

Right click on the Project MMOCorb-Testing and select 'properties'
Add the c++ include paths as shown in the linked image

http://i.imgur.com/N4rq7jb.png

Then add the pre-processor macro as shown in

http://i.imgur.com/k2CMtHM.jpg

Click OK and then Apply

Once added, select the project again and right click, then index/rebuild (may take a while)

Close Eclipse and then re-open it, Carry on working as normal but with the newer version

