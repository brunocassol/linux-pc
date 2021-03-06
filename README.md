# X99 Carbon
Software and configuration for development machine.

## Lubuntu 16.04
http://lubuntu.me/downloads/

Use RUFUS or UNetbootin to make bootable USB.

Install with third-party apt sources on.

### Phone USB tether

If it keeps disconnecting do:

	sudo systemctl disable ModemManager.service
	sudo reboot -h now

### Fonts

Note: Don't use Segoe UI on Lubuntu desktop because it looks bad. Leave default Ubuntu font.

	mkdir ~/.fonts

Copy fonts from gdrive/bin/fonts to ~/.fonts

	sudo fc-cache -fv

### Disable screen locker
sudo mv /etc/xdg/autostart/light-locker.desktop /etc/xdg/autostart/light-locker.desktop.bak

### Transparency Composition in LXDE

source: https://askubuntu.com/questions/548799/how-do-i-start-compositing-in-lxde-for-programs-like-docky

	sudo apt-get update && sudo apt-get install xcompmgr
	subl ~/.config/autostart/xcompmgr.desktop

Paste and save:

	[Desktop Entry]
	Name=XCompositing Manager
	GenericName=Compiz
	Comment=This enables Compositing for LXDE
	Exec=xcompmgr -n
	Terminal=false
	Type=Application
	Icon=dropbox
	Categories=
	StartupNotify=false
	Name[en_US]=Xcompmgr
	Comment[en_US]=This was created by Geary

Start using xcompmgr without havign to retart. Press Win+R:

	xcompmgr -n

### Pin linux kernel (prevent updates)

	for i in $(dpkg -l "*$(uname -r)*" | grep image | awk '{print $2}'); do echo $i hold | sudo dpkg --set-selections; done

Remove pin:

	for i in $(dpkg -l "*$(uname -r)*" | grep image | awk '{print $2}'); do echo $i install | sudo dpkg --set-selections; done

source: https://askubuntu.com/questions/178324/how-to-skip-kernel-update

### PHPStorm on Docker

see https://github.com/brunocassol/phpstorm.git

### git SSH keys (tested on Windows)

	eval $(ssh-agent -s)
	ssh-add ~/.ssh/*

### Wifi - Edimax EW-7811UAC AC600

	mkdir -p ~/drivers
	sudo apt-get install linux-headers-$(uname -r) build-essential gcc-5
	sudo lsusb -v | grep Edimax

	git clone https://github.com/diederikdehaas/rtl8812AU
	cd rtl8812AU
	make CC=/usr/bin/gcc-5
	sudo make install
	sudo modprobe 8812au

To reinstall wifi after kernel upgrade:
	cd ~/drivers/rtl8812AU && make CC=/usr/bin/gcc-5 && sudo make install && sudo modprobe 8812au

### Wifi DKMS:
cd ~/drivers/rtl8812AU
sudo su
DRV_NAME=rtl8812AU
DRV_VERSION=4.3.14
mkdir /usr/src/${DRV_NAME}-${DRV_VERSION}
git archive driver-${DRV_VERSION} | tar -x -C /usr/src/${DRV_NAME}-${DRV_VERSION}
dkms add -m ${DRV_NAME} -v ${DRV_VERSION}
dkms build -m ${DRV_NAME} -v ${DRV_VERSION}
dkms install -m ${DRV_NAME} -v ${DRV_VERSION}

### Wifi Delete DKMS ###
dkms remove rtl8812AU/4.3.14 --all


If wifi stops connecting but still lists networks:

- run connection editor with `sudo nm-connection-editor`
- edit wifi you previously tried to connect to
- type password
- mark [x] allow all users to connect
- save
- restart network with:		sudo service network-manager restart


### Wifi 2nd option:

source: https://github.com/gnab/rtl8812au

	cd /media/dev/Data/downloads/Drivers/Carbon-x99/driver3/
	OPTIONAL: git clone https://github.com/gnab/rtl8812au .

	make
	sudo make install

Replug wifi if need be

### Wifi In case of error implicit declaration of function ‘allow_signal’


	/media/dev/Data/downloads/Drivers/Carbon-x99/rtl8812AU/include/osdep_service.h:343:2: error: implicit declaration of function ‘allow_signal’ [-Werror=implicit-function-declaration]
	allow_signal(SIGTERM);

##### Extra info

- modules installed to: `/lib/modules/4.4.0-75-generic/kernel/drivers/net/wireless`
- install mod with: `sudo insmod 8812au.ko`
- remove mod with `sudo rmmod 8812au`
- restart network applet with: `killall nm-applet && nm-applet &`
- restart network service: `sudo service network-manager restart`
- invoke connection editor manually: `nm-connection-editor`
- alternative driver: https://github.com/gnab/rtl8812au

### Cleanup & Update
	sudo apt-get purge leafpad mtpaint xpad sylpheed sylpheed-doc gnumeric abiword winbind samba
	sudo apt-get update && sudo apt-get upgrade

### Sublime 3
	https://www.sublimetext.com/docs/3/linux_repositories.html#apt

##### Fix sublime 3 hot_exit not working

	rm ~/.config/sublime-text-3/Local/Session.sublime_session

##### Sublime 3 Settings

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

### Set python3 as default
try:
	sudo update-alternatives --config python

else:
	sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 10

### Docker apt-cacher-ng

see: https://github.com/sameersbn/docker-apt-cacher-ng

### handbreak

	sudo add-apt-repository ppa:stebbins/handbrake-releases
	sudo apt-get update
	sudo apt-get install handbrake-gtk handbrake-cli

### Install software
	sudo apt-get install alacarte apt-file aptitude audacity build-essential bzr cabextract calibre cups curl deluge ffmpeg filezilla gameconqueror gcc gdebi gdmap gedit gimp git git-core gitg gmtp gnome-alsamixer gnome-font-viewer gnome-screenshot gparted gpick hexchat htop inkscape iotop kupfer libc6-dev libcurl4-openssl-dev libffi-dev libgtk-3-dev libmp3lame0 libpcre++-dev libpng++-dev libreadline-dev libreoffice libsqlite3-dev libssl-dev libvlccore-dev libwebkitgtk-3.0-dev libwebkitgtk-dev libx11-dev libx11-xcb-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev libxml2-dev libxslt1-dev libxtst-dev libyaml-dev linux-headers-generic lm-sensors lxkeymap lynx meld mysql-workbench nethogs p7zip-full pkg-config powertop python-pygments python-software-properties python3-setuptools roxterm scrot sqlite3 sqliteman ssh tmux tree unetbootin unrar unzip vlc wget wine x11-xkb-utils xcb xclip xdotool xorg-dev youtube-dl zenity zlib1g-dev mercurial


### Fix ttf fonts error
source: http://www.diolinux.com.br/2017/01/ttf-mscorefonts-installer-erro-ubuntu-fix.html

	sudo apt purge ttf-mscorefonts-installer
	wget http://ftp.de.debian.org/debian/pool/contrib/m/msttcorefonts/ttf-mscorefonts-installer_3.6_all.deb -P ~/Downloads
	sudo apt install ~/Downloads/ttf-mscorefonts-installer_3.6_all.deb

### Mount & Blade Warban Fix

sudo apt-get install libaudio2:i386 libglib2.0-0:i386

### SQLiteBrowser

	sudo add-apt-repository -y ppa:linuxgndu/sqlitebrowser
	sudo apt-get update
	sudo apt-get install sqlitebrowser

### Version sensetive packages (check latest)
	sudo apt-get install qt58-meta-full qt58charts-no-lgpl

Add qt58 to ld_config

	sudo bash -c "echo \"/opt/qt58/lib\" > /etc/ld.so.conf.d/qt.conf"

	sudo ldconfig

### NodeJS (via NVM)

Install NVM from the website: `https://github.com/creationix/nvm#installation`

Restart terminal.

List nodejs versions: `nvm ls-remote`

Install latest nodejs: `nvm install 7.8.0`

### nVidia GTX 980
Search for driver in: http://www.nvidia.com/Download/index.aspx
900 Series > Show all operating systemas > Linux 64-bit > Displays:

	Version:	**375.39**  (**<-- THIS IS THE INFO WE ARE LOOKING FOR**)
	Release Date:	2017.2.14
	Operating System:	Linux 64-bit
	Language:	English (US)
	File Size:	73.68 MB

	sudo apt-get install nvidia-375

Reboot

	Open Nvidia Control Panel
	Change Resolution from Auto to Monitor's Native. And change Frequency from Auto to monitor's native (144hz for me).

source: https://linuxconfig.org/how-to-install-the-latest-nvidia-drivers-on-ubuntu-16-04-xenial-xerus

### nVidia libEGL bug fix

If you get `/sbin/ldconfig.real: /usr/lib/nvidia-375/libEGL.so.1 is not a symbolic link` after installing packages do this:

	sudo mv /usr/lib/nvidia-375/libEGL.so.1 /usr/lib/nvidia-375/libEGL.so.1.org
	sudo mv /usr/lib32/nvidia-375/libEGL.so.1 /usr/lib32/nvidia-375/libEGL.so.1.org
	sudo ln -s /usr/lib/nvidia-375/libEGL.so.375.39 /usr/lib/nvidia-375/libEGL.so.1
	sudo ln -s /usr/lib32/nvidia-375/libEGL.so.375.39 /usr/lib32/nvidia-375/libEGL.so.1

source: https://bugs.launchpad.net/ubuntu/+source/nvidia-graphics-drivers-375/+bug/1662860

### ROXTerm configuration

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

### Automount disks

Create directory to mount partition in:

	mkdir /media/dev/Data
	sudo chown dev:dev /media/dev/Data

Find name of partition to be mounted (/dev/sdb2, /dev/sdc2):

	sudo gnome-disk

Find UUID of partition to be mounted (UUID is better in case you change disk order):

	sudo blkid

Insert mount into fstab:

	sudo subl /etc/fstab

	UUID="0CA490C8A490B5A4"		/media/dev/Data		ntfs		user,exec,uid=dev,gid=dev,default_permissions,allow_other   0   0

Save then issue a mount --all:

	sudo mount -a

### nginx & PHP 7.1
	mkdir ~/docker
	cd ~/docker
	git clone https://github.com/ngineered/nginx-php-fpm.git
	cd nginx-php-fpm


old: sudo apt-get install mysql-server mysql-client libmysqlclient-dev libmysqlclient-dev php libapache2-mod-php php-mcrypt php-mysql php-mbstring php-cli php-xml php-curl

### Lubuntu clock ###
Right click, format: %A %d/%m/%Y %B %r (%j)

### NodeJS &  Npm
	sudo apt-get install nodejs

### Golang

##### Install

	Download from https://golang.org/dl/

	tar -C /usr/local -xzf go1.8.linux-amd64.tar.gz

	subl ~/.profile

PATH="$HOME/bin:$HOME/.local/bin:/usr/local/go/bin:$HOME/go/bin:$PATH"

##### Upgrade

Download new version from https://golang.org/dl/

	go version
	sudo rm -rf /usr/local/go
	cd ~/Downloads
	sudo tar -C /usr/local -xzf go1.9.linux-amd64.tar.gz
	go version


### Visual Studio Code
https://code.visualstudio.com/Download

##### Custom Icon (differ from Sublime 3)

Download: https://iconverticons.com/icons/a6f4b751accdedb6/
(in /media/dev/Data/dev/vscode_icon)

	sudo cp /media/dev/Data/dev/vscode_icon/512x512.png /usr/share/code/resources/app/resources/linux/code.png
	sudo cp /media/dev/Data/dev/vscode_icon/512x512.png /usr/share/pixmaps/code.png

	cp /media/dev/Data/dev/vscode_icon/16x16.png ~/.local/share/icons/hicolor/16x16/apps/VSCode.png
	cp /media/dev/Data/dev/vscode_icon/24x24.png ~/.local/share/icons/hicolor/24x24/apps/VSCode.png
	cp /media/dev/Data/dev/vscode_icon/32x32.png ~/.local/share/icons/hicolor/32x32/apps/VSCode.png
	cp /media/dev/Data/dev/vscode_icon/48x48.png ~/.local/share/icons/hicolor/48x48/apps/VSCode.png
	cp /media/dev/Data/dev/vscode_icon/64x64.png ~/.local/share/icons/hicolor/64x64/apps/VSCode.png
	cp /media/dev/Data/dev/vscode_icon/128x128.png ~/.local/share/icons/hicolor/128x128/apps/VSCode.png
	cp /media/dev/Data/dev/vscode_icon/256x256.png ~/.local/share/icons/hicolor/256x256/apps/VSCode.png
	cp /media/dev/Data/dev/vscode_icon/512x512.png ~/.local/share/icons/hicolor/512x512/apps/VSCode.png

	subl ~/.local/share/applications/code.desktop

Set this in both lines:

	Icon=VSCode

Logout and in again if needed.

##### Extensions
* https://marketplace.visualstudio.com/items?itemName=lukehoban.Go
* https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker

After installing Golang: Open VS Code. Press CTRL+Shift+P and run: `Go: Install Tools`.

File -> Preferences -> File Icon Theme -> Seti (Visual Studio Code)

##### VS Code Custom keybindings.json:
	[
		{
			"key": "ctrl+shift+a",
			"command": "-editor.action.blockComment",
			"when": "editorTextFocus && !editorReadonly"
		},
		{
			"key": "ctrl+;",
			"command": "editor.action.commentLine"
		},
		{
			"key": "f7",
			"command": "workbench.action.terminal.runActiveFile"
		},
		{
			"key": "f4",
			"command": "-search.action.focusNextSearchResult"
		},
		{
			"key": "f4",
			"command": "workbench.action.tasks.build"
		},
		{
			"key": "ctrl+shift+b",
			"command": "-workbench.action.tasks.build"
		},
		{
			"key": "ctrl+k",
			"command": "workbench.action.terminal.clear"
		},
		{
			"key": "ctrl+t",
			"command": "workbench.action.terminal.toggleTerminal"
		},
		{
			"key": "ctrl+shift+[BracketLeft]",
			"command": "-workbench.action.terminal.toggleTerminal"
		}
	]

##### VS Code settings.json:
	{
	    "workbench.welcome.enabled": false,
	    "editor.insertSpaces": false,
	    "editor.fontSize": 16,
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
	  // When enabled, will trim trailing whitespace when saving a file.
	    "files.trimTrailingWhitespace": true,
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
	    },
	    "go.useLanguageServer": false,
	    "go.autocompleteUnimportedPackages": true,
	    "telemetry.enableTelemetry": false,
	    "telemetry.enableCrashReporter": false,
	    "window.zoomLevel": 0,

	      // Flags to pass to `go vet` (e.g. ['-all', '-shadow'])
	      "go.vetFlags": ["-all", "-shadow", "-shadowstrict"]
	}

##### VS Code tasks.json:
	{
	"version": "0.1.0",
	"command": "go",
	"isShellCommand": true,
	"showOutput": "always",
	"_runner": "terminal",
	"tasks": [
		{
		"taskName": "go run",
		"suppressTaskName": true,
		"isBuildCommand": true,
		"args": [
			"run ${file}"
		]
		}
	]
	}

##### VS Code always highlight minimap

Change file
sudo subl /usr/share/code/resources/app/out/vs/workbench/workbench.main.css

.minimap-slider{opacity:0 --> .minimap-slider{opacity:1

for highlight always

VSCode will complain that the installation is corrupt. To disable this use this setting:

	"telemetry.enableCrashReporter": false

##### VS Code Tips:

- `F4` - `go run` current file
- `F7` - Runs go file in terminal if it is executable and has this shebang: `//usr/bin/env go run "$0" "$@"; exit "$?"`
- `F8` - Go to next problem
- `CTRL` + `;` - Comment line(s)
- `CTRL` + `B` - Show/hide file tree
- `CTRL` + `T` - Toggle terminal
- `CTRL` + `L` - Clear terminal
- `F1` - list commands
- `CTRL` + `P` - Open file
- `CTRL` + `SHIFT` + `P`: Go: Add tags... Add json tags to structure using tool

### Updated winetricks (Required for Evernote)
	wget  https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
	chmod +x winetricks
	sudo mv -v winetricks /usr/local/bin

source: http://askubuntu.com/questions/755059/how-do-i-get-the-latest-version-of-winetricks-on-ubuntu

### Chrome Remote Desktop

	To fix this, please follow these steps:

	Stop it:
	/opt/google/chrome-remote-desktop/chrome-remote-desktop --stop

	Back it up:
	cp /opt/google/chrome-remote-desktop/chrome-remote-desktop /opt/google/chrome-remote-desktop/chrome-remote-desktop.orig

	Edit:  /opt/google/chrome-remote-desktop/chrome-remote-desktop

	Change this to the size I wanted.
	DEFAULT_SIZES = "2560x1440"

	Change this to desktop zero which is the console.
	FIRST_X_DISPLAY_NUMBER = 0

	Comment this out so it doesn't increment for a new desktop.:
	#while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
	#  display += 1

	Comment this out so that it doesn't attempt to start a new display session since the console on desktop zero is already running :
		#logging .info("Starting %s on display :%d" % (xvfb, display))
		#screen_option = "%dx%dx24" % (max_width, max_height)
		#self .x_proc = subprocess.Popen(
		#    [xvfb, ":%d" % display,
		#     "-auth", x_auth_file,
		#     "-nolisten", "tcp",
		#     "-noreset",
		#     "-screen", "0", screen_option
		#    ] + extra_x_args)
		#if not self.x_proc.pid:
		#  raise Exception("Could not start Xvfb.")

	Start it:
	/opt/google/chrome-remote-desktop/chrome-remote-desktop --start

	DONE.  Now it attaches to Unity's existing X Server on display :0.﻿

source: https://productforums.google.com/d/msg/chrome/LJgIh-IJ9Lk/JXxoVc-lDxYJ

### Evernote

Download and install Evernote:

	WINEPREFIX="$HOME/.wine32" WINEARCH=win32 wine wineboot
	winetricks -q ie8

	cd ~/Downloads
	wine Evernote_6.5.4.4720.exe

Evernote menu > Options:

- General > Show advanced > mark
- General > Check for updates > unmark
- General > Left panel theme > light
- Note > Create new note in a new window > unmark
- Shortcut keys > Clear all


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

VLC Bonus: Fix playback for some mp4 videos:

source: https://www.idealshare.net/video-converter/vlc-mp4-solution.html

	Preferences -> Video
	Check the box for Window decorations (if it's unchecked).
	Set the Output drop-down menu to X11 video output (XCB).


### VirtualBox

First install latest Qt online installer: https://www1.qt.io/download-open-source/#section-2

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

docker-ce:
	https://docs.docker.com/engine/installation/linux/ubuntu/#install-using-the-repository

docker-compose:
	https://docs.docker.com/compose/install/

	sudo su
	curl -L https://github.com/docker/compose/releases/download/1.13.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
	chmod +x /usr/local/bin/docker-compose

To use docker without sudo:

	sudo groupadd docker
	sudo gpasswd -a ${USER} docker
	sudo service docker restart
	newgrp docker

### Redis

todo: user docker or vagrant.

### Redis Desktop Manager

Requires packages: `qt58-meta-full qt58charts-no-lgpl`

	git clone --recursive https://github.com/uglide/RedisDesktopManager.git -b alpha4 rdm && cd ./rdm/src
	./configure
	source /opt/qt58/bin/qt58-env.sh && qmake && make && sudo make install
	cd /usr/share/redis-desktop-manager/bin
	sudo mv qt.conf qt.backup

	sudo ln -s /usr/share/redis-desktop-manager/bin/rdm.sh /usr/local/bin/rdm

export LD_LIBRARY_PATH=/opt/qt58/lib

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

	sudo easy_install3 pip
	sudo python3 -m pip install leviathan
	sudo levctl -s 30 -m off

### Configure kupfer
auto start & hotkey

Disable all plugins but:
* Applications
* Clipboard
* Gnome-session
* Multithread

### HP wireless printer

- Download latest HPLIP `https://sourceforge.net/projects/hplip/files/hplip/`

	cd ~/Downloads
	chmod +x hplip-3.16.11.run
	./hplip-3.16.11.run

- Reboot PC
- Click on HP tray icon HP tray icon should appear.
- Click CUPS web interface (opens browser on http://localhost:631/admin)
- Click `Add Printer` button
- Type linux username/password
- Select `Deskjet 3540 series (HP Deskjet 3540 series)` from Discovered Printers
- Next, Next, Add Printer
- Set defaults: Black Ink Only, Draft Quality
- Reboot PC

### Configure OpenBox

	obconf

Set:
- Desktops: 1
- Theme: Natura
- Remove shortcuts and spacers from taskbar.

subl ~/.config/openbox/lubuntu-rc.xml

	<!-- Custom Keybindings -->
    <keybind key="W-q">
      <action name="Execute">
        <command>/home/dev/Qt/Tools/QtCreator/bin/qtcreator</command>
      </action>
    </keybind>
    <keybind key="W-c">
      <action name="Execute">
        <command>code</command>
      </action>
    </keybind>
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
    	<command>roxterm -d /home/dev/go/src/github.com/brunocassol/bot</command>
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

Also remove this:
      <mousebind button="Middle" action="Press">
        <action name="Lower"/>
        <action name="FocusToBottom"/>
        <action name="Unfocus"/>
      </mousebind>
      <mousebind button="Middle" action="Click">
        <action name="ToggleMaximize">
          <direction>vertical</direction>
        </action>
      </mousebind>
      <mousebind button="A-Middle" action="Press">
        <action name="Lower"/>
        <action name="FocusToBottom"/>
        <action name="Unfocus"/>
      </mousebind>

And this:
    <keybind key="F11">
      <action name="ToggleFullscreen"/>
    </keybind>

openbox --reconfigure

### Fix reconnect wifi after suspend

	cd /lib/systemd/system-sleep/
	sudo subl restart_wifi

	#!/bin/sh
	set -e

	if [ "$2" = "suspend" ] || [ "$2" = "hybrid-sleep" ]; then
	    case "$1" in
	        pre) echo "Suspending. Will restart network-manager after." ;;
	        post) sleep 5; /etc/init.d/network-manager restart ;;
	    esac
	fi

	sudo chmod +x restart_wifi

### Update cache

	sudo apt-file update
	sudo updatedb

### Corsair H20100 Headset

Use pulseaudio. Much better.

sudo apt-get install pulseaudio pavucontrol && pavucontrol

run pavucontrol without root. Disable all but Headset.

##### Configuring volume controls

To set Pulseaudio volume using commandline:

	amixer -D pulse sset Master 8%+
	amixer -D pulse sset Master 8%-

Run `xev` and scroll headset volume control to get keys (XF86AudioLowerVolume and XF86AudioRaiseVolume):

	FocusOut event, serial 48, synthetic NO, window 0x1800001,
	    mode NotifyGrab, detail NotifyAncestor

	FocusIn event, serial 48, synthetic NO, window 0x1800001,
	    mode NotifyUngrab, detail NotifyAncestor

	KeymapNotify event, serial 48, synthetic NO, window 0x0,
	    keys:  2   0   0   0   0   0   0   0   0   0   0   0   0   0   0   4
	           0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0

	KeyRelease event, serial 48, synthetic NO, window 0x1800001,
	    root 0x1d1, subw 0x0, time 4969627, (-2174,124), root:(207,1358),
	    state 0x0, keycode 122 (keysym 0x1008ff11, XF86AudioLowerVolume), same_screen YES,
	    XLookupString gives 0 bytes:
	    XFilterEvent returns: False

	FocusOut event, serial 48, synthetic NO, window 0x1800001,
	    mode NotifyGrab, detail NotifyAncestor

	FocusIn event, serial 48, synthetic NO, window 0x1800001,
	    mode NotifyUngrab, detail NotifyAncestor

	KeymapNotify event, serial 48, synthetic NO, window 0x0,
	    keys:  2   0   0   0   0   0   0   0   0   0   0   0   0   0   0   8
	           0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0

	KeyRelease event, serial 48, synthetic NO, window 0x1800001,
	    root 0x1d1, subw 0x0, time 4970164, (-2174,124), root:(207,1358),
	    state 0x0, keycode 123 (keysym 0x1008ff13, XF86AudioRaiseVolume), same_screen YES,
	    XLookupString gives 0 bytes:
	    XFilterEvent returns: False

Set openbox shortcuts to call the volume commands:

	subl ~/.config/openbox/lubuntu-rc.xml

Make sure the volume shotcuts look like this:

    <!-- Keybinding for Volume management -->
    <keybind key="XF86AudioRaiseVolume">
      <action name="Execute">
        <command>amixer -D pulse sset Master 8%+ unmute</command>
      </action>
    </keybind>
    <keybind key="XF86AudioLowerVolume">
      <action name="Execute">
        <command>amixer -D pulse sset Master 8%- unmute</command>
      </action>
    </keybind>

source: https://unix.stackexchange.com/questions/62887/volume-hot-keys-in-crunchbang-dont-work

### .bashrc
	alias gits='git status'
	alias gita='git add . && git status'
	alias gitc='git commit -m "."'
	alias gitp='git push'
	alias gitd='git diff'
	alias gitall='git add . && git status && git commit -m "." && git push'
	alias wififix='cd /media/dev/Data/downloads/Drivers/Carbon-x99/rtl8812AU && sudo make CC=/usr/bin/gcc-5 && sudo make install && sudo modprobe 8812au'

	alias config='subl ~/.bashrc'
	alias comparedir='diff -qr '
	alias gitgo='cd ~/go/src/github.com/brunocassol'

	alias config='subl ~/.bashrc ~/.config/openbox/lubuntu-rc.xml'
	alias bye='sudo bash -c "sleep 7200; pm-suspend"'
	alias gdrivecompareremote='rclone -n -v --retries 1 --exclude /gdocs/ sync remote: /media/dev/Data/gdrive'


	alias dockerphpstorm='docker run --name phpstorm -it --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v ~/.PhpStorm2017.1:/home/duser/.PhpStorm2017.1 -v ~/php:/workspace brunocassol/phpstorm'
	alias dockernginxphp='docker run --name nginx-php -d nginx-php-fpm'
	alias dockeraptcachestart='docker run --name apt-cacher-ng -d --restart=always --publish 3142:3142 --volume /srv/docker/apt-cacher-ng:/var/cache/apt-cacher-ng sameersbn/apt-cacher-ng:latest'
	alias dockeraptcachelog='docker exec -it apt-cacher-ng tail -f /var/log/apt-cacher-ng/apt-cacher.log'

	alias youtube-mp3='youtube-dl --extract-audio --audio-format mp3'

	pidusingport() {
		sudo netstat -nlp | grep :$1
	}
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
			*.tar.xz)    tar xf $1    ;;
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

Examples (-n is simulate-only):

- rclone -n -v --retries 1 --exclude /gdocs/ sync remote: /media/dev/Data/gdrive
- rclone -v --retries 1 --exclude /gdocs/ copy /media/dev/Data/gdrive remote:

another option: https://github.com/vitalif/grive2

### Crontabs

	mkdir ~/log
	crontab -e

	# Upload to Google Drive every 30 minutes
	*/30 * * * * /home/dev/go/bin/rclone -v --retries 1 --exclude /gdocs/ copy /media/dev/Data/gdrive remote: > /home/dev/log/cron.log 2>&1

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


### Logitech G303

##### Scroll speed and programming

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

Source:

- http://www.webupd8.org/2015/12/how-to-change-mouse-scroll-wheel-speed.html
- http://askubuntu.com/questions/285689/increase-mouse-wheel-scroll-speed/621140#621140

##### Broken mouse button: 1 click = multiple clicks fix
Diagnose it with:

	xev | grep ButtonRelease

	or

	http://unixpapa.com/js/testmouse.html

Fix with:
	https://www.youtube.com/watch?v=eDoXMJyimDU

Note: you need a very good mini screwdriver.

### Set Default Apps

LXMenu > Preferences > Default applications for LXSession:

- Terminal: ROXTerm
- Web browser: Google Chrome
- E-mail: Disable
- Video player: VLC
- Text editor: Visual Studio Code (it now remmebers unsaved files)
- Spreadsheet: Libre Office Calc
- Document: Libre Office Writter

Find a DPF file. Right-click > Open with... > Chrome set default

### Autostart applications
mkdir ~/utils && cd ~/utils
touch .autostart.sh && chmod +x .autostart.sh && subl .autostart.sh

	sleep 5
	imwheel --kill --buttons "4 5"
	env WINEPREFIX="/home/dev/.wine" wine C:\\Program\ Files\ \(x86\)\\Evernote\\Evernote\\Evernote.exe &

LXMenu > Preferences > Default applications for LXSession > Autostart > Add:

	/home/dev/utils/.autostart.sh

### Tips

- Lubuntu shortcuts: https://help.ubuntu.com/community/Lubuntu/Keyboard
- Use bash alias `bye` to sleep for 2 hours then shutdown PC.
- CTRL+Y in VLC saves playlist
- Use aliases defined in .bashrc
