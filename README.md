# X99 Carbon
Software and configuration for dev machine.

## Lubuntu 16.04
http://lubuntu.me/downloads/
Use RUFUS or UNetbootin to make bootable USB.
Install with third-party apt sources on.

### Phone USB tether
	sudo systemctl disable ModemManager.service
	sudo reboot -h now

### Wifi - Edimax EW-7811UAC AC600
	sudo apt-get install linux-headers-$(uname -r) build-essential gcc-5
	sudo lsusb -v | grep Edimax

	git clone https://github.com/diederikdehaas/rtl8812AU
	cd rtl8812AU
	make CC=/usr/bin/gcc-5
	sudo make install
	sudo modprobe 8812au

### Fixed LAN IP
todo.

### Cleanup & Update
	sudo apt-get purge lxterminal leafpad mtpaint xpad sylpheed sylpheed-doc gnumeric abiword
	sudo apt-get update && sudo apt-get upgrade

### Sublime 3
	sudo add-apt-repository ppa:webupd8team/sublime-text-3 && sudo apt-get install sublime-text-installer

### Install software
	sudo apt-get install build-essential linux-headers-generic mysql-workbench curl gimp gdebi git hexchat kupfer gnome-terminal lxkeymap ssh gnome-alsamixer gnome-screenshot sqliteman libreoffice unetbootin p7zip-full vlc htop zlib1g-dev libssl-dev libyaml-dev python-pygments gpick sqliteman git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev wine apt-file gparted youtube-dl deluge aptitude

### nVidia GTX 980
Search for driver in: http://www.nvidia.com/Download/index.aspx
900 Series > Show all operating systemas > Linux 64-bit > Displays:

	Version:	**375.39**  (**<-- THIS IS THE INFO WE ARE LOOKING FOR**)
	Release Date:	2017.2.14
	Operating System:	Linux 64-bit
	Language:	English (US)
	File Size:	73.68 MB

	sudo apt-get install nvidia-375

source: https://linuxconfig.org/how-to-install-the-latest-nvidia-drivers-on-ubuntu-16-04-xenial-xerus

### LAMP
	sudo apt-get install mysql-server mysql-client libmysqlclient-dev libmysqlclient-dev apache2 php libapache2-mod-php php-mcrypt php-mysql php-mbstring php-cli php-xml php-curl 

### NodeJS &  Npm
	sudo apt-get install nodejs

### Golang
https://golang.org/dl/

	tar -C /usr/local -xzf go1.8.linux-amd64.tar.gz
	echo "export PATH=\$PATH:/usr/local/go/bin" >> .profile
	echo "export PATH=\$PATH:$(go env GOPATH)/bin" >> .profile
	source ~/.profile

### Visual Studio Code
https://code.visualstudio.com/Download

After installing Golang: Open VS Code. Press CTRL+Shift+P and run: `Go: Install Tools`.

Settings:

	{
		"workbench.welcome.enabled": false,
		// Pick 'gofmt', 'goimports' or 'goreturns' to run on format.
		"go.formatTool": "goimports",
		// Configure glob patterns for excluding files and folders.
		"files.exclude": {
			"**/.git": true,
			"**/.svn": true,
			"**/.hg": true,
			"**/.DS_Store": true,
			"**/debug": true,
			"**/*.exe": true
		},
	// Controls if the minimap is shown
	"editor.minimap.enabled": true,

	// Render the actual characters on a line (as opposed to color blocks)
	"editor.minimap.renderCharacters": false,

	// Limit the width of the minimap to render at most a certain number of columns
	"editor.minimap.maxColumn": 120,

	// A multiplier to be used on the `deltaX` and `deltaY` of mouse wheel scroll events
	"editor.mouseWheelScrollSensitivity": 1,
	"files.autoSave": "afterDelay",
	"files.autoSaveDelay": 1000
	}

##### Extensions
* https://marketplace.visualstudio.com/items?itemName=lukehoban.Go

##### Settings
	{
		"workbench.welcome.enabled": false,
		// Pick 'gofmt', 'goimports' or 'goreturns' to run on format.
		"go.formatTool": "gofmt",
		// Configure glob patterns for excluding files and folders.
		"files.exclude": {
		"**/.git": true,
		"**/.svn": true,
		"**/.hg": true,
		"**/.DS_Store": true,
		"**/debug": true,
		"**/*.exe": true
	},
	// Controls if the minimap is shown
	"editor.minimap.enabled": true,

	// Render the actual characters on a line (as opposed to color blocks)
	"editor.minimap.renderCharacters": false,

	// Limit the width of the minimap to render at most a certain number of columns
	"editor.minimap.maxColumn": 120,

	// A multiplier to be used on the `deltaX` and `deltaY` of mouse wheel scroll events
	"editor.mouseWheelScrollSensitivity": 3
	}

### Git configuration
	git config --global user.name "Bruno Cassol"
	git config --global user.email brunocassol@gmail.com
	git config --global push.default simple
	git config --global color.ui true

### Leviathan - Kraken x61 watercooler controller
https://github.com/jaksi/leviathan

	sudo levctl -s 30 -m off
 
### Configure kupfer
auto start & hotkey

Disable all plugins but:
* Applications
* Clipboard
* Gnome-session
* Multithread

### Configure OpenBox

	obconf

Set:
- Desktops: 1
- Theme: Natura
- Remove shortcuts and spacers from taskbar.

subl ~/.config/openbox/lubuntu-rc.xml

	<!-- Custom Keybindings -->
	<keybind key="W-Print">
		<action name="Execute">
		<command>gnome-screenshot -a -c</command>
		</action>
	</keybind>
	<keybind key="W-w">
		<action name="Execute">
		<command>x-www-browser</command>
		</action>
	</keybind>
	<keybind key="W-t">
		<action name="Execute">
		<command>x-terminal-emulator</command>
		</action>
	</keybind>
	<keybind key="W-l">      
		<action name="Execute">        
		<command>dm-tool lock</command>      
		</action>    
	</keybind>

Replace <keybind key="W-Up"> to:

	<keybind key="W-Up">        # HalfUpperScreen
		<action name="ToggleMaximizeFull"/>
	</keybind>

Remove this:

	<mousebind button="Up" action="Click">
		<action name="GoToDesktop">
		<to>previous</to>
		</action>
	</mousebind>
	<mousebind button="Down" action="Click">
		<action name="GoToDesktop">
		<to>next</to>
		</action>
	</mousebind>

openbox --reconfigure

### Configure shortcuts


### gnome-terminal
Shortcuts:

* reset & clear	ctrl+k
* new tab		ctrl+t
* close tab		ctrl+w
* help		F12

font: DjaVu Sasns Mono 12
Unlimited scrollback.

### Update cache

	sudo apt-file update
	sudo updatedb

### Corsair H20100 Headset 
	
sudo apt-get install pulseaudio pavucontrol && pavucontrol

run pavucontrol without root. Disable all but Headset.

### .bashrc
subl ~/.bashrc

	alias gits='git status'
	alias config='subl ~/.bashrc ~/.config/openbox/lubuntu-rc.xml'
	alias bye='sudo -s -- sleep 7200 && pm-suspend'

	search() {
		grep -Rils $1 $2
	}
	winprocess() {
		xprop | grep WM_CLASS
	}
	extract () {
		if [ -f $1 ] ; then
		case $1 in
			*.tar.bz2)   tar xvjf $1    ;;
			*.tar.gz)    tar xvzf $1    ;;
			*.bz2)       bunzip2 $1     ;;
			*.rar)       unrar x $1       ;;
			*.gz)        gunzip $1      ;;
			*.tar)       tar xvf $1     ;;
			*.tbz2)      tar xvjf $1    ;;
			*.tgz)       tar xvzf $1    ;;
			*.zip)       unzip $1       ;;
			*.Z)         uncompress $1  ;;
			*.7z)        7z x $1        ;;
			*)           echo "don't know how to extract '$1'..." ;;
		esac
		else
		echo "'$1' is not a valid file!"
		fi
	}

	# Optional: This was used as a Guest in Virtual Box
	# set default keyboard layout. we don't have time to fix IBus shenanigans
	# setxkbmap -layout gb

### Sexy bash prompt
https://github.com/twolfson/sexy-bash-prompt

### Google Drive
https://github.com/vitalif/grive2

### Optional: VirtualBox shared folders
- mkdir -p /home/dev/vbox_shared/FOLDER_NAME
- Share folder in virtualbox menu then mount it:
- sudo mount -t vboxsf -o uid=$UID,gid=$(id -g) FOLDER_NAME /home/dev/vbox_shared/FOLDER_NAME

If it works then unmount it: sudo umount FOLDER_NAME

Add this to `/etc/fstab`:

	gdrive /home/dev/vbox_shared/FOLDER_NAME vboxsf comment=systemd.automount,uid=dev,gid=dev,noauto 0 0

### Auto login

	sudo subl /etc/lightdm/lightdm.conf

Paste this:

	[SeatDefaults]
	autologin-user=dev


### Logitech G303 scroll speed and programming

	sudo apt-get install imwheel

Create .imwheelrc

	subl ~/.imwheelrc

Add this:

	".*"
	None,      Up,   Button4, 3
	None,      Down, Button5, 3
	Control_L, Up,   Control_L|Button4
	Control_L, Down, Control_L|Button5
	Shift_L,   Up,   Shift_L|Button4
	Shift_L,   Down, Shift_L|Button5

Open menu > Default applications for LXSession > Autostart > Add:

	imwheel --kill --buttons "4 5"

Run:
\
	imwheel --kill --buttons "4 5"

Source:

- http://www.webupd8.org/2015/12/how-to-change-mouse-scroll-wheel-speed.html
- http://askubuntu.com/questions/285689/increase-mouse-wheel-scroll-speed/621140#621140

### Tips

#### Suspend PC after 2 hours
Use bash alias `bye` to sleep for 2 hours then shutdown PC.