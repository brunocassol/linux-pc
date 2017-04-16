# X99 Carbon
Software and configuration for development machine.

## Lubuntu 16.04
http://lubuntu.me/downloads/

Use RUFUS or UNetbootin to make bootable USB.

Install with third-party apt sources on.

### Phone USB tether
	sudo systemctl disable ModemManager.service
	sudo reboot -h now

### Fonts

Note: Don't use Segoe UI on Lubuntu desktop because it doesn't looks good. Leave default Ubuntu.

	mkdir ~/.fonts

Copy fonts from gdrive/bin/fonts to ~/.fonts

	sudo fc-cache -fv

### GitKraken
https://www.gitkraken.com/

### Disable screen locker
sudo mv /etc/xdg/autostart/light-locker.desktop /etc/xdg/autostart/light-locker.desktop.bak

### Wifi - Edimax EW-7811UAC AC600
	sudo apt-get install linux-headers-$(uname -r) build-essential gcc-5
	sudo lsusb -v | grep Edimax

	git clone https://github.com/diederikdehaas/rtl8812AU
	cd rtl8812AU
	make CC=/usr/bin/gcc-5
	sudo make install
	sudo modprobe 8812au

### Cleanup & Update
	sudo apt-get purge leafpad mtpaint xpad sylpheed sylpheed-doc gnumeric abiword
	sudo apt-get update && sudo apt-get upgrade

### Sublime 3
	sudo add-apt-repository ppa:webupd8team/sublime-text-3 && sudo apt-get install sublime-text-installer

##### Settings

	{
		"always_show_minimap_viewport": true,
		"font_size": 14,
		"highlight_line": true,

		// By default, shift+tab will only unindent if the selection spans
		// multiple lines. When pressing shift+tab at other times, it'll insert a
		// tab character - this allows tabs to be inserted when tab_completion is
		// enabled. Set this to true to make shift+tab always unindent, instead of
		// inserting tabs.
		"shift_tab_unindent": true,

		// When auto_find_in_selection is enabled, the "Find in Selection" flag
		// will be enabled automatically when multiple lines of text are selected
		"auto_find_in_selection": true,

		// Makes tabs with modified files more visible
		"highlight_modified_tabs": true,

		// Display file encoding in the status bar
		"show_encoding": true,

		// Display line endings in the status bar
		"show_line_endings": true,

		// Set to true to removing trailing white space on save
		"trim_trailing_white_space_on_save": false
	}


### Install software
	sudo apt-get install build-essential linux-headers-generic mysql-workbench curl gimp gdebi git hexchat kupfer lxkeymap ssh gnome-alsamixer gnome-screenshot sqliteman libreoffice unetbootin p7zip-full vlc htop zlib1g-dev libssl-dev libyaml-dev python-pygments gpick sqliteman git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev wine apt-file gparted youtube-dl deluge aptitude ffmpeg gedit roxterm libvlccore-dev pkg-config unrar unzip wget zenity cabextract meld winbind gcc libc6-dev libx11-dev xorg-dev libxtst-dev libpng++-dev xcb libxcb-xkb-dev x11-xkb-utils libx11-xcb-dev libxkbcommon-x11-dev libxkbcommon-dev libpcre++-dev gnome-font-viewer

### Version sensetive packages (check latest)
	sudo apt-get install qt58-meta-full qt58charts-no-lgpl

### NodeJS (via NVM)

Install NVM from the website: `https://github.com/creationix/nvm#installation`

Restart terminal.

List nodejs versions: `nvm ls-remote`

Install latest nodejs: `nvm install 7.8.0`

### Redis commander (requires NodeJS)
see: https://www.npmjs.com/package/redis-commander

	npm install -g redis-commander

	redis-commander

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

### nVidia libEGL bug fix

If you get `/sbin/ldconfig.real: /usr/lib/nvidia-375/libEGL.so.1 is not a symbolic link` after installing packages do this:

	sudo mv /usr/lib/nvidia-375/libEGL.so.1 /usr/lib/nvidia-375/libEGL.so.1.org
	sudo mv /usr/lib32/nvidia-375/libEGL.so.1 /usr/lib32/nvidia-375/libEGL.so.1.org
	sudo ln -s /usr/lib/nvidia-375/libEGL.so.375.39 /usr/lib/nvidia-375/libEGL.so.1
	sudo ln -s /usr/lib32/nvidia-375/libEGL.so.375.39 /usr/lib32/nvidia-375/libEGL.so.1

source: https://bugs.launchpad.net/ubuntu/+source/nvidia-graphics-drivers-375/+bug/1662860

###ROXTerm configuration

Preferences > Edit Current Profile
* Font: Djavu Sans Mono Book 12
* Colour Scheme: Tango

Preferences > Edit Current Shortcuts Scheme

	[roxterm shortcuts scheme]
	File/New Window=<Shift><Control>n
	File/New Tab=<Control>t
	File/Close Window=<Shift><Control>q
	File/Close Tab=<Control>w
	Tabs/Previous Tab=<Control>Page_Up
	Tabs/Next Tab=<Control>Tab
	Edit/Select All=<Shift><Control>a
	Edit/Copy=<Shift><Control>c
	Edit/Paste=<Shift><Control>v
	Edit/Reset And Clear=<Control>k
	View/Zoom In=<Control>plus
	View/Zoom Out=<Control>minus
	View/Normal Size=<Control>0
	View/Full Screen=F11
	View/Scroll Up One Line=<Shift>Up
	View/Scroll Down One Line=<Shift>Down
	Help/Help=F1
	Edit/Copy & Paste=<Shift><Control>y
	Search/Find...=<Shift><Control>f
	Search/Find Next=<Shift><Control>i
	Search/Find Previous=<Shift><Control>p


### Gnome Terminal
Shortcuts:
* reset & clear	ctrl+k
* new tab		ctrl+t
* close tab		ctrl+w
* help		F12

font: DjaVu Sasns Mono 12

Unlimited scrollback.

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

##### Extensions
* https://marketplace.visualstudio.com/items?itemName=lukehoban.Go

After installing Golang: Open VS Code. Press CTRL+Shift+P and run: `Go: Install Tools`.

Preferences -> File Icon Theme -> Seti (Visual Studio Code)

##### Settings:
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
		"editor.minimap.maxColumn": 80,
		// A multiplier to be used on the `deltaX` and `deltaY` of mouse wheel scroll events
		"editor.mouseWheelScrollSensitivity": 1,
		"files.autoSave": "off",
		"files.autoSaveDelay": 1000,
		"editor.fontFamily": "'DejaVu Sans Mono','Droid Sans Mono', 'Courier New', monospace, 'Droid Sans Fallback'",
		"workbench.iconTheme": "vs-seti",
		// Use gotype on the file currently being edited and report any semantic or syntactic errors found after configured delay.
		"go.liveErrors": {
			"enabled": true,
			"delay": 500
		}
	}

##### Tools
HTTP Benchmark

	go get -u github.com/rakyll/hey

### Updated winetricks (Required for Evernote)
	wget  https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
	chmod +x winetricks 
	sudo mv -v winetricks /usr/local/bin

source: http://askubuntu.com/questions/755059/how-do-i-get-the-latest-version-of-winetricks-on-ubuntu

### Evernote
	
Download and install Evernote:
	
	WINEPREFIX="$HOME/.wine32" WINEARCH=win32 wine wineboot
	winetricks -q ie8

	cd ~/Downloads
	wine Evernote_6.5.4.4720.exe

source: https://appdb.winehq.org/objectManager.php?sClass=version&iId=34727

### VLC click to pause plugin

	git clone https://github.com/nurupo/vlc-pause-click-plugin/
	cd vlc-pause-click-plugin/vlc-2.2.x+/
	make
	sudo make install

- Restart VLC to load the newly added plugin
- Go into advanced preferences: Tools -> Preferences -> Show settings -> All
- Enable/Disable the plugin with a checkbox: (in advanced preferences) Interface -> Control Interfaces -> Pause/Play video on mouse click
- Enable/Disable the plugin with a checkbox: (in advanced preferences) Video -> Filters -> Pause/Play video on mouse click
- Change mouse button to the one you want: (in advanced preferences) Video -> Filters -> Pause click -> Mouse Button
- Restart VLC for settings to take place

### VirtualBox

	sudo bash -c "echo \"deb http://download.virtualbox.org/virtualbox/debian xenial contrib\" >> /etc/apt/sources.list"

	wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -

	sudo apt-get update
	sudo apt-get install virtualbox-5.1

source: https://www.virtualbox.org/wiki/Linux_Downloads

### Vagrant

Requires VirtualBox

Ubuntu repo is outdated. Download from https://www.vagrantup.com/downloads.html

Run `vagrant' to create ~/.vagrant.d/

### Docker

Follow https://store.docker.com/editions/community/docker-ce-server-ubuntu?tab=description

### Redis

### Redis Desktop Manager

Requires packages: `qt58-meta-full qt58charts-no-lgpl`

	git clone --recursive https://github.com/uglide/RedisDesktopManager.git -b alpha4 rdm && cd ./rdm/src
	./configure
	source /opt/qt58/bin/qt58-env.sh && qmake && make && sudo make install
	cd /usr/share/redis-desktop-manager/bin
	sudo mv qt.conf qt.backup

	sudo ln -s /usr/share/redis-desktop-manager/bin/rdm.sh /usr/local/bin/rdm

Run with:

	rdm

source: http://docs.redisdesktop.com/en/latest/install/#build-from-source

Add item to Preferences -> Main Menu

	name: Redis Desktop Manager
	command: /usr/share/redis-desktop-manager/bin/rdm
	icon inside /usr/share/redis-desktop-manager/bin

### Git configuration
	git config --global user.name "Bruno Cassol"
	git config --global user.email brunocassol@gmail.com
	git config --global push.default simple
	git config --global color.ui true
	git config --global core.editor subl
	git config --global diff.tool meld

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
    <keybind key="W-s">
      <action name="Execute">
        <command>subl</command>
      </action>
    </keybind>
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


### LXTerminal
Shortcuts:

* reset & clear	ctrl+k
* new tab		ctrl+t
* close tab		ctrl+w

100.000 scrollback.

### Update cache

	sudo apt-file update
	sudo updatedb

### Corsair H20100 Headset 
	
sudo apt-get install pulseaudio pavucontrol && pavucontrol

run pavucontrol without root. Disable all but Headset.

### .bashrc
subl ~/.bashrc

	~/.config/openbox/lubuntu-rc.xml'
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

	alias gits='git status'
	alias gitc='git commit -m "."'
	alias gita='git add . && git status'
	alias gitp='git push'
	alias gitd='git diff'
	alias config='subl ~/.bashrc'
	alias comparedir='diff -qr '

### Sexy bash prompt
https://github.com/twolfson/sexy-bash-prompt

### Google Drive

	go get -u -v github.com/ncw/rclone/...
	rclone config

	name> remote
	type> 7 google drive
	client_id> (ENTER, leave blank)
	client_secret> (ENTER, leave blank)
	remote_config> Y
	(accept browser OAuth permissions in browser that will open)
	is_this_ok?> Y
	anything else?> q

	rclone -v --retries 1 --exclude /gdocs/ sync remote: /media/dev/Data/gdrive
	rclone -n -v --retries 1 --exclude /gdocs/ sync /media/dev/Data/gdrive remote:

another option: https://github.com/vitalif/grive2

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

LXMenu > Preferences > Default applications for LXSession > Autostart > Add:

	imwheel --kill --buttons "4 5"

Run:

	imwheel --kill --buttons "4 5"

Source:

- http://www.webupd8.org/2015/12/how-to-change-mouse-scroll-wheel-speed.html
- http://askubuntu.com/questions/285689/increase-mouse-wheel-scroll-speed/621140#621140

### Set Default Apps

LXMenu > Preferences > Default applications for LXSession:

- Terminal: ROXTerm
- Web browser: Google Chrome
- E-mail: Disable
- Video player: VLC
- Text editor: Visual Studio Code (it now remmebers unsaved files)
- Spreadsheet: Libre Office Calc
- Document: Libre Office Writter


### Tips

- Lubuntu shortcuts: https://help.ubuntu.com/community/Lubuntu/Keyboard
- Use bash alias `bye` to sleep for 2 hours then shutdown PC.
- CTRL+Y in VLC saves playlist
- Use aliases defined in .bashrc