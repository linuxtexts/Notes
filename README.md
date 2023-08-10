# 1.txt

___New User Install ___________________________________________________
-----------------------------------------------------------------------
        Programs:
        #install brew
        #install iterm2
        #install and confugure LuLu

        brew install fuse
        brew install ranger mc mpv watch rar p7zip testdisk nmap exif
                #or for old version MacOS
                sudo port install p7zip
        brew install gfxcardstatus tor
        brew install --cask bitcoin-core
        brew install --cask handbrake avidemux gimp transmission unshaky
        brew install android-platform-tools
        brew install --cask librewolf
        brew install tor
        install monitor cpu temperature - https://github.com/macmade/Hot/releases/tag/1.6.1

        #install to watch in bit-bar free mem
                pip3 install psutil
                curl -O https://raw.githubusercontent.com/giampaolo/psutil/master/scripts/meminfo.py
                python3 ./meminfo.py

        #install Vimari for Safari in MacOS
        #install and config Spreadsheet Calculator for counting (qsc)
        #install and configure mutt (email client)
                copy .gnupg the folder with main keys
                cpoy .mutt the folder with e-mail keys
        #install "noisy" to generate random traffic - https://github.com/1tayH/noisy
                sudo easy_install pip
                pip3 install requests
                git clone https://github.com/1tayH/noisy.git
                cd ~/noisy/
                python noisy.py --config config.json
        #install tail windows manager for Mac - Ametist (brew cask install amethyst)
        #install private browser - Icecat, Librefox
        #install tsocks to configure mutt via socks5 proxy/via ssh tunel

        #install xcode
                sudo rm -rf /Library/Developer/CommandLineTools
                sudo xcode-select --install
        #install wineskil and winetricks
                brew install winetricks

        #install "Play on Mac" and "Wineskin Winery"
        #install fan control program "TG Pro"

        #install Firefox and Plagins
                firefox tree style tabs,
                User-Agent Switcher and Managerby Ray
                CanvasBlocker
        #install PhotoSpace X from AppStort to watch photo


        ___qERRORS___________________________________________________________________________________________________________________

        #Probler - x2go client english language requested not loaded translator
        ##Solution - Please try moving your .bashrc aside and retry.

-----------------------------------------------------------------------------------------------------------------------------

        #connect() failed (111: Connection refused) while connecting to upstream

        https://serverfault.com/questions/317393/connect-failed-111-connection-refused-while-connecting-to-upstream
        It sounds like you haven't started and configured the backend for Nginx. Start php-fpm and add the following to nginx.conf, in the http context:
-----------------------------------------------------------------------------------------------------------------------------
        #Check the folder "3rd party". Running "git submodule update --init" will initialize the git submodule that handles the subfolder "3rd party
                git submodule update --init --recursive

-----------------------------------------------------------------------------------------------------------------------------
        x2go error 7 xQuarts dont connect - check /etc/ssh/ssh_conf
-----------------------------------------------------------------------------------------------------------------------------
        You need a Passphrase to protect your secret key.
        gpg: cancelled by user
        gpg: Key generation canceled.

        It is becouse You cant use the "user" key under the "su user"
        Regen:
        script /dev/null
        gpg2 --gen-key
-----------------------------------------------------------------------------------------------------------------------------
        #nginx failed (13: Permission denied)
        #Need to check all folder permisson and set 755 to each folders (/home/admin , /home/admin/www , /home/admin/www/site )

-----------------------------------------------------------------------------------------------------------------------------
        #Cannot start session without errors, please check errors given in your PHP
        #change unrem in php.ini - session.save_path = "/tmp"
-----------------------------------------------------------------------------------------------------------------------------
        AH01574: module php7_module is already loaded, skipping
        #Apache is running a threaded MPM, but your PHP Module is not compiled to be threadsafe.  You need to recompile PHP.
        #AH00013: Pre-configuration failed

        #Replace in /etc/httpd/conf.modules.d/00-mpm.conf
        "LoadModule mpm_event_module modules/mod_mpm_event.so"
        #with
        "LoadModule mpm_prefork_module modules/mod_mpm_prefork.so"

-----------------------------------------------------------------------------------------------------------------------------
        /bin/bash: /bin/nc: No such file or directory
        #install nc ---> yum install ns -y
-----------------------------------------------------------------------------------------------------------------------------
        nginx warn ---> n upstream response is buffered to a temporary file
        set to config nginx site file ---> proxy_max_temp_file_size 0;
-----------------------------------------------------------------------------------------------------------------------------
        #Invalid command 'SuexecUserGroup'
        type "sudo a2enmod", then type "suexec" to turn on the module

-----------------------------------------------------------------------------------------------------------------------------
        #curl: (7) Failed to connect to ** port 80: No route to host
        iptables -I INPUT -p tcp --dport 80 -j ACCEPT

-----------------------------------------------------------------------------------------------------------------------------
        #Table 'mysql.user' doesn't exist:ERROR
        #https://stackoverflow.com/questions/17780630/table-mysql-user-doesnt-existerror
        mysql_upgrade -u root

-----------------------------------------------------------------------------------------------------------------------------
        #Socket file /var/lib/mysql/mysql.sock exists ---> reinstall mariadb with delete old file bases

-----------------------------------------------------------------------------------------------------------------------------
        #FFMPEG: Too many packets buffered for output stream 0:2
                add ---> -max_muxing_queue_size 1024
-----------------------------------------------------------------------------------------------------------------------------
        #Resolved — 400 Bad Request: Request Header or Cookie too Large via Nginx

        https://websiteforstudents.com/resolved-400-bad-request-request-header-or-cookie-too-large-via-nginx/

        #Add (large_client_header_buffers 4 16k;) to nginx conf file
        server {
                # ...
                large_client_header_buffers 4 16k;
                # ...
        }

        #Commend or remove line in nginx conf file
                proxy_set_header Host app.example.com;

        #Add LimitRequestFieldSize 32380 or 64380 to apach config file (the default value 8190 bytes)
                LimitRequestFieldSize 32380
-----------------------------------------------------------------------------------------------------------------------------
        #How to fix homebrew permissions?
                sudo chown -R $(whoami):admin /usr/local
                sudo chown -R $(whoami):admin /Library/Caches/Homebrew
-----------------------------------------------------------------------------------------------------------------------------
        #Black screen im Mac Os after login -> Command+R boot in Recovery Mode and delete "rm -f /Library/Preferences/com. apple.loginwindow.plist"
-----------------------------------------------------------------------------------------------------------------------------
        #Truble with syncthing (need to reinstall from the stable pocket - https://apt.syncthing.net/)
                curl -s https://syncthing.net/release-key.txt | sudo apt-key add -
                echo "deb https://apt.syncthing.net/ syncthing stable" | sudo tee /etc/apt/sources.list.d/syncthing.list
                sudo apt-get update
                sudo apt-get install syncthing
                -----------------------------------------------------------------------------------------------------------------------------
	#ImportError: No module named selenium
		pip3 install selenium
-----------------------------------------------------------------------------------------------------------------------------
	aclocal not found --> brew install automake
-----------------------------------------------------------------------------------------------------------------------------
	automator The action “Run Shell Script” encountered an error: “”
	put at the begining of screept - PATH="/usr/local/bin:$PATH"

-----------------------------------------------------------------------------------------------------------------------------
	shell-init: error retrieving current directory: getcwd: cannot access parent directories: No such file or directory
	Just change the directory -> cd ~/
-----------------------------------------------------------------------------------------------------------------------------
	bash: shuf: command not found
	Just install - brew install coreutils
-----------------------------------------------------------------------------------------------------------------------------
	Problem with CloudFlare and images (some images may not load)

	Optional Cloudflare Performance features may potentially cause issues with images or pictures on your site. The two features that may cause issues are    
	Rocket Loader or Mirage, so you should try turning them off in your Speed Settings if you see an issue.

	Tips:
	-Rocket Loader would generally impact images that are being delivered by something involving JavaScript or jQuery.

	-Mirage Lazy Loader is generally the cause if the behavior is only happening in certain browsers. Mirage is only included for domains on a paid           
	Cloudflare plan, so you can rule this feature out if your domain is on a free plan.
-----------------------------------------------------------------------------------------------------------------------------
	#error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
	xcode-select --install
-----------------------------------------------------------------------------------------------------------------------------
	ModuleNotFoundError: No module named 'pkg_resources' ---> yum install python-setuptools

-----------------------------------------------------------------------------------------------------------------------------
	Error if PHP Fatal error:  Call to undefined function (if use "<?" instead "<?php")
	set in php.ini ---> short_open_tag=On
-----------------------------------------------------------------------------------------------------------------------------
	#AH01630: client denied by server configuration
	#set permission for folder in httpd site conf just add in <Directory>
		Options Indexes FollowSymLinks
		AllowOverride All
		Require all granted

-----------------------------------------------------------------------------------------------------------------------------
	Connection attempt failed with "ECONNABORTED - Connection aborted".

-----------------------------------------------------------------------------------------------------------------------------
	#Upload file

	#413 "Request Entity Too Large"
	#Set:
		php.ini - post_max_size and upload_max_filesize
		nginx.conf - http { client_max_body_size 100M; }
		httpd.conf - LimitRequestBody (also you cat set it in .htaccess)
---server time----------------------------------------------------------------------------------------------------------------
	#set timezone in php.ini
		date.timezone = America/New_York


------------------------------------------------------------------------------------------------------------------------------
	#upstream timed out (110: Connection timed out) while reading response header
	#set it to site nginx file config 
		server {
			proxy_connect_timeout       6000;
			proxy_send_timeout          6000;
			proxy_read_timeout          6000;
			send_timeout                6000;
		}

------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------
	504 Gateway Timeout Error
	ini_set('max_execution_time', 300); (set it in php file)
	proxy_connect_timeout       600;
	proxy_send_timeout          600;
	proxy_read_timeout          600;
	send_timeout                600;
------------------------------------------------------------------------------------------------------------------------------
	PHP Warning:  curl_setopt_array(): CURLOPT_FOLLOWLOCATION cannot be activated when an open_basedir is set in
	Set safe_mode = Off in your php.ini file (it's usually in /etc/ on the server). If that's already off, then look around for the open_basedir stuff in the php.ini file, and
	change it accordingly

	Set [open_basedir = none] in php.ini
	Set [php_admin_value open_basedir "none"] in sitename.com.conf
	Set [safe_mode = Off] in php.ini
------------------------------------------------------------------------------------------------------------------------------
	Don't work rewrite_mode check http.conf (grep -i AllowOverride /etc/httpd/conf/httpd.conf) and 
	set "AllowOverride All" (https://www.e2enetworks.com/help/knowledge-base/how-to-enable-mod_rewrite-on-apache-on-centos/)
------------------------------------------------------------------------------------------------------------------------------
	Unable to negotiate with 192.168.0.100 port 2244: no matching host key type found. Their offer: ssh-dss
	ssh -oHostKeyAlgorithms=+ssh-dss admin@192.168.0.100 -p 2244
------------------------------------------------------------------------------------------------------------------------------
	Playing mp4 stoped by error network connection -->problem with nginx


___qFirewalls_____________________________________________________________________________________________________
#Stop and disable Firewalld
	#Is firewalld running on my system?
		sudo firewall-cmd --state
	#Stop the the firewalld
		sudo systemctl stop firewalld
	#Disable the FirewallD service at boot time
		sudo systemctl disable firewalld
		sudo systemctl mask --now firewalld
	#Verify that the FirewallD is gone
		sudo systemctl status firewalld

	#How do enable the firewalld again?
		sudo systemctl unmask --now firewalld
		sudo systemctl enable firewalld
		sudo systemctl start firewalld
		## verify that the firewalld started ##
		sudo firewall-cmd --state

#disable ufw based firewall
	#Is the ufw running?
		sudo ufw status
	#Stop the ufw on Linux
		sudo ufw disable
	#Disable the ufw on Linux at boot time
		sudo systemctl disable ufw
	#Verify that the ufw is gone
		sudo ufw status
		sudo systemctl status ufw

	#How do enable the ufw again?
		sudo systemctl enable ufwjjjjjjjjjjjjj
		sudo ufw enable
		## verify that ufw started ##
		sudo ufw status

#disable iptables
	#Stop the iptables service on Linux
		service iptables stop
	#Disable the iptables service at boot time on Linux
		chkconfig iptables off
