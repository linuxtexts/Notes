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

___qIPTABLES______________________________________________________________________________________________________
------------------------------------------------------------------------------------------------------------------
	https://notessysadmin.com/minimum-rules-for-iptables
	https://toster.ru/q/14066
	http://farmal.in/2011/07/borba-s-ddos-atakami-sredstvami-iptables-i-sysctl/

	#redirect (forwarding) port form 80 to 8080 on local PC
		iptables -t nat -A OUTPUT -o lo -p tcp --dport 80 -j REDIRECT --to-port 8080

	#INSTALL IPTABLES at NEW SERVER
		1. Install IPTABLES
			systemctl disable firewalld
			systemctl mask firewalld
			yum install iptables-services -y
			systemctl enable iptables
			systemctl enable ip6tables
........................................................................ /etc/sysconfig/iptables
	*filter
	:INPUT ACCEPT [0:0]
	:FORWARD ACCEPT [0:0]
	:OUTPUT ACCEPT [214:43782]
	-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
	-A INPUT -i lo -j ACCEPT
	-A INPUT -j REJECT --reject-with icmp-port-unreachable
	COMMIT

........................................................................ /etc/sysconfig/ip6tables
	*filter
	:INPUT ACCEPT [0:0]
	:FORWARD ACCEPT [0:0]
	:OUTPUT ACCEPT [214:43782]
	-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
	-A INPUT -i lo -j ACCEPT
	-A INPUT -j REJECT --reject-with icmp6-adm-prohibited
	COMMIT
............................................................................................. END
		2. Save rules (it save rules to /etc/sysconfig/iptables) ---> service iptables save
		3. Restart ---> service iptables restart

	#Save iptable rules to file       ---> iptables-save > /etc/iptables.rules (">" resave to the file, ">>" save to the end of the file)
	#Restore iptables rules from file ---> iptables-restore < /etc/iptables.rules
	#show all rules and usage traffic ---> iptables -L or iptables -n -L -v --line-numbers

	#DELETE RULE    ---> iptables -D INPUT 4
	#close port 25  ---> iptables -A INPUT -p tcp --destination-port 25 -j DROP
	#open port 25   ---> iptables -A INPUT -p tcp --destination-port 25 -j ACCEPT
	#BLOCK IP       ---> iptables -I INPUT -s 115.214.193.224 -j DROP

	#Drop traffic STATISTIC to zerio ---> iptables -Z INPUT
	#Disable ip6 ipv6 service ip6tables stopchkconfig ip6tables off -->/etc/sysconfig/iptables systemctl stop firewalld
	# Default iptable rules
		iptables -P INPUT DROP
		iptables -P FORWARD DROP
		iptables -P OUTPUT ACCEPT

	# Allow input connection from localhost       ---> iptables -A INPUT -i lo -j ACCEPT
	# Allow already installed inbound connections ---> iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

	# HTTP port ---> iptables -A INPUT -p TCP --dport 80 -j ACCEPT

	# Show IPtables REAL TIME ---> watch -n 2 -d iptables -nv20L

	# Set limit max connection from one IP adress (max 100 connection from IP)
		iptables -t filter -I INPUT -p tcp --syn --dport 80 -m connlimit --connlimit-above 100 --connlimit-mask 32 -j DROP
		(https://unix.stackexchange.com/questions/139285/limit-max-connections-per-ip-address-and-new-connections-per-second-with-iptable)




  ___qDDOS______________________________________________________________________________________________________________________
------------------------------------------------------------------------------------------------------------------------------
	http://mycrimea.su/partners/web/access/ 

	#Number of connection to 80 port ---> netstat -na | grep :80 | wc -l
	#The same in the SYN status 	 ---> netstat -na | grep :80 | grep syn
	#Show online 			 ---> netstat -na | grep :80 | grep EST | wc -l

	#Popular User-Agents
		cat /var/www/admin/data/logs/sitename.net.access.log | awk -F\" '{print $6}' | sort | uniq -c | sort -n
		netstat -ntu | awk ' $5 ~ /^[0-9]/ {print $5}' | cut -d: -f1 | sort | uniq -c | sort -n
		netstat -nt | awk -F":" '{print $2}' | sort | uniq -c | sort -n (show with port)

		alalis last 500 string of log file -->tail -n 500 /var/www/admin/data/logs/sitename.net.access.log | cut -d' ' -f1 | sort | uniq -c | sort -gr

		tail -n 2000000 /var/www/admin/data/logs/sitename.net.access.log | cut -d' ' -f1 | sort | uniq -c | sort -n
					
		___qlogtop____________________________________________________________________________________________________
		#What does logtop shows?
		#line1 - Just number
		#line2 - amount of query from ip
		#line3 - amount of query from ip in one second
		#line4 - ip adress
					
		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $1; fflush();'} | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | cut -d ' ' -f 1 | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $7; fflush();'} | logtop
		---------------------------------------------------------------------------------------------------------------
		awk '{print $4}' /var/www/httpd-logs/text.access.log | cut -d: -f1 | uniq -c

		#Show refferer to site (use acess log file)
			grep "200 " /home/admin/httpd-logs/site.com.access.log | cut -d '"' -f 4 | sort | uniq -c | sort -rn | grep -v "site.com" | less

	
	#Pratektion from flud by nginx

	#In http section
	
	map $http_user_agent $bad_useragent {                                                                                                                    
        default 0;                                                                                                                                               
        ~*ia_archiver 1;                                                                                                                                         
        ~*Curl 1;                                                                                                                                                
        ~*libwww 1;                                                                                                                                              
        ~*BLEXBot 1;                                                                                                                                             
        ~*SBooksNet 1;                                                                                                                                           
        ~*MJ12bot 1;                                                                                                                                             
        ~*Java 1;                                                                                                                                                
        ~*NTENTbot 1;                                                                                                                                            
        ~*GetIntent 1;                                                                                                                                           
        ~*SemrushBot 1;                                                                                                                                          
        ~*HybridBot 1;                                                                                                                                           
        ~*AhrefsBot 1;                                                                                                                                           
        ~*SeznamBot 1;                                                                                                                                           
        ~*DeuSu 1;                                                                                                                                               
        ~*GrapeshotCrawler 1;                                                                                                                                    
        ~*SentiBot 1;                                                                                                                                            
        ~*default 1;                                                                                                                                             
        ~*Virusdie 1;                                                                                                                                            
        ~*WordPress 1;                                                                                                                                           
        ~*WhatsApp 1;                                                                                                                                            
        ~*SeopultContentAnalyzer 1;                                                                                                                              
        ~*WinHTTP 1;                                                                                                                                             
        ~*MauiBot 1;                                                                                                                                             
        ~*weborama 1;                                                                                                                                            
        ~*Python 1;                                                                                                                                              
        ~*Go-http-client 1;                                                                                                                                      
        ~*VelenPublicWebCrawler 1;                                                                                                                               
        }
      	                                                                                                                                                         
      	#in server section                                                                                                                                       
      	                                                                                                                                                         
      	if ($bad_useragent) {                                                                                                                                    
      		return 444;                                                                                                                                              
      	}

___qTEST SERVER_______________________________________________________________________________________________________________
-------------------------------------------------------------------------------------------------------------------------------
	#TEST(iftop) loading HDD realtime ("yum install sysstat") (https://pingvinoff.net/2009/01/06/komanda-iostat/)
                iostat -xmd 1
                iostat 5 3
		#on mac os use ---> iostat -w1 disk0

		# rrqm/s 	обобщенное количество запросов на чтение в секунду;
		# wrqm/s 	обобщенное количество запросов на запись в секунду;
		# r/s 	количество запросов на чтение в секунду;
		# w/s 	количество запросов на запись в секунду;
		# rMB/s 	количество МБ при чтении с диска в секунду;
		# wMB/s 	количество МБ при записи на диск в секунду;
		# avgrq-sz 	средний размер (в секторах) запросов к диску;
		# avgqu-sz 	средний размер очереди запросов к диску;
		# await 	среднее время (милисекунды) на обработку запросов к диску (включает в себя время, потраченное в очереди на обработку и время на обработку запроса);
		# r_await 	среднее время (милисекунды) на обработку запросов чтения к диску (включает в себя время, потраченное в очереди на обработку и время на обработку запроса);
		# w_await 	среднее время (милисекунды) на обработку запросов запи ик диску (включает в себя время, потраченное в очереди на обработку и время на обработку запроса);
		# svctm 	среднее время (милисекунды) I/O запросов (не образайте внимания на неё — будет удалена в ближайшем будущем);
		# %util 	% CPU, затраченный на передачу I/O запросов к диску («пропускная способность» диска);

		# %util if it 90-100% hdd speed not enough

	#REAL TIME Bandwich Usages (http://palexa.pp.ua/blog/linux/setevye-resheniya/utilita-iftop-kontrol-trafika-v-rezhime-realnogo-vremeni-online.html)
                iftop -i [eth interface]
                iftop -i eth0 -f "dst port domain" (DNS traffic)
                iftop -i eth0 -f "icmp" (ICMP traff)
                iftop -i eth0 -f "dst port http" (HTTP traff)
                iftop -i eth0 -f "dst port 22" (Traff going to 22 port
                iftop -i eth0 -f "dst 8.8.8.8" (Traff going to ip 8.8.8.8)

	#Test RAID 	---> cat /proc/mdstat
	#Test speed bandwidth traff speed ---> speedtest-cli-master
	#Test free space ---> df -h
	#Show CPU temperature ---> sensors
	#Temperature HDD     ---> hddtemp /dev/sda



___qhistory____________________________________________________________________________________________________________________
	#Show history ---> history
	#Search in history ---> history | grep word
	#Reply command number 3 ---> !number (!3)


___qRAID_______________________________________________________________________________________________________________________
	#stop RAID  	  	---> echo idle > /sys/block/md1/md/sync_action
	#start RAID 	  	---> echo check > /sys/block/md0/md/sync_action
	#stats RAID 	  	---> cat /proc/mdstat
	#stop RAID by speed	---> echo 0 >/proc/sys/dev/raid/speed_limit_max 
	#turn off RAID 	  	---> set all speed limin to 0 in /proc/sys/dev/raid/speed_limit_max and speed_limit_min

	-------Create RAID--------------------------------------------------------------------------------------------------
	#qraid 0 is fast RAID (https://www.tecmint.com/create-raid0-in-linux/)
	#raid 1 is standard mirror RAID
	#configure and install raid0 - https://www.tecmint.com/create-raid0-in-linux/

	###1. Install mdadm
	yum clean all && yum update
	yum install mdadm -y

	###2. Before creating RAID 0, make sure to verify that the attached two hard drives are detected or not, using the following command.
	ls -l /dev | grep sd

	###3. Once the new hard drives detected, it’s time to check whether the attached drives are already using any existing raid with the help of following ‘mdadm’ command.
	mdadm --examine /dev/sd[b-c]

	###4. Now create sdb and sdc partitions for raid, with the help of following fdisk command. Here, I will show how to create partition on sdb drive.
	fdisk /dev/sdb

	    Press ‘n‘ for creating new partition.
	    Then choose ‘P‘ for Primary partition.
	    Next select the partition number as 1.
	    Give the default value by just pressing two times Enter key.
	    Next press ‘P‘ to print the defined partition.

	Follow below instructions for creating Linux raid auto on partitions.

	    Press ‘L‘ to list all available types.
	    Type ‘t‘to choose the partitions.
	    Choose ‘fd‘ for Linux raid auto and press Enter to apply.
	    Then again use ‘P‘ to print the changes what we have made.
	    Use ‘w‘ to write the changes.

	Please follow same above instructions to create partition on sdc drive now.

	###5. After creating partitions, verify both the drivers are correctly defined for RAID using following command.
	mdadm --examine /dev/sd[b-c]
	mdadm --examine /dev/sd[b-c]1

	###6. Now create md device (i.e. /dev/md0) and apply raid level using below command.
	mdadm -C /dev/md0 -l raid0 -n 2 /dev/sd[b-c]1
	#or
	mdadm --create /dev/md0 --level=stripe --raid-devices=2 /dev/sd[b-c]1
		
	    -C – create
	    -l – level
	    -n – Number of raid-devices

	###7. Once md device has been created, now verify the status of RAID Level, Devices and Array used, with the help of following series of commands as shown.
	cat /proc/mdstat
	mdadm -E /dev/sd[b-c]1
	mdadm --detail /dev/md0

	###8. Create a ext4 filesystem for a RAID device /dev/md0 and mount it under /dev/raid0.
	mkfs.ext4 /dev/md0

	###9. Once ext4 filesystem has been created for Raid device, now create a mount point directory (i.e. /mnt/raid0) and mount the device /dev/md0 under it.
	mkdir /mnt/raid0
	mount /dev/md0 /home/

	###10.  Next, verify that the device /dev/md0 is mounted under /home directory using df command.
	df -h

	###11. Once you’ve verified mount points, it’s time to create an fstab entry in /etc/fstab file.
	vim /etc/fstab

	#Add the following entry as described. May vary according to your mount location and filesystem you using.
	/dev/md0                /home/              ext4    defaults         0 0


	###12. Run mount ‘-a‘ to check if there is any error in fstab entry.
	mount -av

	###Saving RAID Configurations
	###13. Finally, save the raid configuration to one of the file to keep the configurations for future use. Again we use ‘mdadm’ command with ‘-s‘ (scan) and ‘-v‘ (verbose) options as shown.
	mdadm -E -s -v >> /etc/mdadm.conf
	mdadm --detail --scan --verbose >> /etc/mdadm.conf
	cat /etc/mdadm.conf

	#That’s it, we have seen here, how to configure RAID0 striping with raid levels by using two hard disks. In next article, we will see how to setup RAID5.
	#https://www.tecmint.com/create-raid0-in-linux/

	___qps_________________________________________________________
	#Show all process with comfortable view
		ps -ejH
	#Show all process
		ps -elf

	___qnetstat____SS analog netstat_______________________________
	#Show all connection
		netstat -tunup

	___qss_________________________________________________________
	#Show all connection
		ss -tunup
	
	___qcp ________________________________________________________
	#Copy folder 		    ---> cp -a /source/. /dest/
	#Copy files (limited spee)d ---> rsync -avu --bwlimit=20000 --progress /source/files/ /destination/files/ (speed limit 20000kb)
		#--chmod=Du=rwx,Dg=rx,Do=rx,Fu=rw,Fg=r,Fo=r
		#Du = Directory Owner (Read, write, execute)
		#Dg = Directory Group (Read, execute)
		#Do = Directory Users (all) (Read, execute)
		#Fu = File Owner (Read, write)
		#Fg = File Group (Read)
		#Fo = File Users (all) (Read)

	#Copy file by using scp
		scp -P 443 ~/Downloads/Homer-Conferencing.sh root@8.9.9.9:/root
	#Copy file by using scp via proxy
		scp -P2277 "/usr/bin/ssh -o ProxyCommand=nc -X 5 -x 127.0.0.1:9050 %h %p" ~/Downloads/file.txt admin@192.168.0.1:/home/admin/www/1main/qwe/1s/source/

	___qtouch_______________________________________________________
	#Change "Data Creation" and "Last Modified of file"
		touch -a -m  -t 201001011212 temp.txt
		# -a = accessed
		# -m = modified
		# -t  = timestamp - use [[CC]YY]MMDDhhmm[.ss] time format

	#Transfer file from local server to remout server
		rsync -v -e "ssh -p2222" /home/localuser/testfile.txt remoteuser@X.X.X.X:~/transfer


  	___qrsync_______________________________________________________
	#https://www.linuxtechi.com/rsync-command-examples-linux/
	#Transfer file from remout server to local server
		rsync -v -e ssh remoteuser@X.X.X.X:/home/remoteuser/transfer/testfile.txt /home/localuser/
		rsync -avu --bwlimit=5000 --chmod=Fu=rwx,Fg=r,Fo=r --progress /from_folder/ ~/destination_folder/
	#rsync for resume download use "--partial"
		rsync -azvvP /home/path/folder1/ /home/path/folder2

	#rsync for cloud storage
		rsync -azvvpu --delete -e "ssh -p443" ~/.A8fhd4ksDk root@ip-address:/home/
		#--ignore-existing - copy only new file

	#rsync exclude directorie
		rsync -avzP --bwlimit=5000 --exclude {'file1.txt','dir1/*','dir2'} /source-directory /destination-directory
		#example
			rsync -avzP --exclude {'/.save/', '/save/'} ~/ /Volumes/hdd/john/
		#rsync without recurcive ---> -d

		#example exclude ONE directorie
			rsync -avzP --exclude '/save/' ~/ /Volumes/hdd/john/

	#rsync transfer file to server via socks5 proxy
		rsync --progress -avrz -e 'ssh -p2244 -o ProxyCommand="nc -X 5 -x 127.0.0.1:5060 %h %p"' /Users/john/Downloads/test.mp4  admin@remoute-server:/name-folder/filename

	___qhosts__/etc/hosts_______________________________________________
	127.0.0.1 analytics.google.com
	127.0.0.1 www.google-analytics.com
	127.0.0.1 google-analytics.com
	127.0.0.1 ssl.google-analytics.com


___qExif___________________________________________________________
	#Install ---> brew install exiftool
	#Show all date for file ---> exiftool -time:all -a -G0:1 -s file.mp4
	#Change exif set CreateDate = CreationDate (So the main is set CreateDate, CreationDate and FileModifyDate the date you'l need)
		exiftool "-CreateDate<CreationDate" "-FileModifyDate<CreateDate" file.mp4

	#Example, if Photo has a wrong time sequence
		1. See (exiftool -time:all -a -G0:1 -s IMG_0531.mov) if CreationDate has right time then Step2 change "CreateDate" and "FileModifyDate"
		2. exiftool "-CreateDate<CreationDate" "-FileModifyDate<CreationDate" file.mpr

	#Manual set date
		exiftool -AllDates="2013:08:27 08:50:14" ~/Camera3/Camera/IMG_8458_1.mov

___qSMART__________________________________________________________
	#install smart on centos ---> yum install smartmontools
	#Show SMART HDD smart ---> smartctl -A /dev/sda 
	#Show SMART HDD smart ---> smartctl -a disk3s2  
	#The main parametr 5 - "Reallocated_Sector"
	second 197 - "Current_Pending_Sector"

	#insatall smart on macos ---> brew install smartmontools
	#run -> smartctl -a disk0 (and watch "Percentage Used" if it more than 30% soom mac will die)


 ___qMySQL_________________________________________________________________________________________________________
------------------------------------------------------------------------------------------------------------------
	#Connect to BD and make Query
		mysql -u username -puserpass dbname -e "UPDATE mytable SET mycolumn = 'myvalue' WHERE id='myid'";
		mysql --user=username --password=passwordtext basename
		mysql --user=username --password=passwordtext basename -e "SELECT * FROM main WHERE id='1'";
		mysql --user=username --password=passwordtext basename -e "SELECT * FROM user WHERE password='aaaaabbbbb'";
		
	#Show tables of database
		#Enter in mysql
		use name_of_database;
		show tables;
	#Show columns from [table name];
		show columns from table;
	
	select post_text from posts where post_text like "%http:%" and id<100;
	update posts SET post_text = REPLACE(post_text, 'http://name', 'https://name') WHERE post_text LIKE '%http://name%';
	
	#Replace
	UPDATE main SET other_cont_en = REPLACE(other_cont_en, ' width="800" height="600"', '') WHERE INSTR(other_cont_en, '%width="800"%') > 0;
	SELECT * FROM `main` WHERE `other_cont_en` like '% width="800" height="600"%'

	#Make dump-file(backup base) with base MySQL  
		mysqldump --single-transaction -u user -p DBNAME > backup.sql

	#Recover from dump-file to MySQL
		mysql basename -u username -p < base.sql
	-------------------------------------------------------------------------------
	/usr/bin/mysqladmin -u root password 'new-password'
	mysql -u root -p
	mysql> CREATE DATABASE name_bae;
	mysqladmin -u root -p'oldpassword' password newpass
	-------------------------------------------------------------------------------
	#Install qMariaDB
		yum remove mysql mysql-server
		mv /var/lib/mysql /var/lib/mysql_old_backup
		yum install mariadb-server -y
		yum install mariadb -y
		chmod -R 777 /var/lib/mysql/
		systemctl enable mariadb.service
		service mariadb start
		/usr/bin/mysql_secure_installation

		#Enter current password for root (enter for none): Enter
		#Set root password? [Y/n]: Y
		#New password: <your-password>
		#Re-enter new password: <your-password>
		#Remove anonymous users? [Y/n]: Y
		#Disallow root login remotely? [Y/n]: Y
		#Remove test database and access to it? [Y/n]: Y
		#Reload privilege tables now? [Y/n]: Y

	-------------------------------------------------------------------------------
	#Reset password for MySQL
		service mariadb stop
		mysqld_safe --skip-grant-tables &
		mysql -u root
		mysql> use mysql;
		mysql> update user set password=PASSWORD("NEW-ROOT-PASSWORD") where User='root';
		mysql> flush privileges;
		mysql> quit
		service mariadb start
		mysql -u root -p (check)
	-------------------------------------------------------------------------------
	#Create user and database
		mysql -u root -p
		GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost' IDENTIFIED BY '5w7PJcCp';
	#then login as new user ---> mysql -u newuser -p
	#and create database for user ---> CREATE DATABASE `newuser`;
	-------------------------------------------------------------------------------
	#check and fix mysql errors and problem ---> mysql_upgrade -uroot -p
	#chemk if service mariadb active ---> systemctl is-active mariadb

	#drop database
		mysql -u root -p
		show databases;
		drop database name; (if is there "-" use ---> drop database `name`;)


	#ERROR mariadb cant start after Reset password for MySQL
	#use ---> ps aux | grep -i mysql
	#and kill all mysql processes and start MariaDB again	

___ssh-chat_qchat ssh__________________________SSH CHAT_____________________________________________________________
--------------------------------------------------------------------------------------------------------------------
	https://github.com/shazow/ssh-chat
	#Start ssh chat
		ssh-chat --verbose --bind ":22" --identity ~/.ssh/id_dsa
	#Connect to chat
		ssh ip-adress 


___qOpenVPN_________________________________________________________________________________________________________
--------------------------------------------------------------------------------------------------------------------
	#https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-centos-7
	#STEP 1
		#Install OpenVPN
			yum install openvpn
		#Download easy-rsa and copy some files to the speciad directory
			wget -O /tmp/easyrsa https://github.com/OpenVPN/easy-rsa-old/archive/2.3.3.tar.gz
			tar xfz /tmp/easyrsa
			mkdir /etc/openvpn/easy-rsa
			cp -rf easy-rsa-old-2.3.3/easy-rsa/2.0/* /etc/openvpn/easy-rsa
		#If needed ---> chown sammy /etc/openvpn/easy-rsa/

	#STEP 2 Configure OpenVPN
		#Copy config file
			cp /usr/share/doc/openvpn-2.4.8/sample/sample-config-files/server.conf /etc/openvpn/
		#Edit config file /etc/openvpn/server.conf
		#push all traffic on client to VPN server
			push "redirect-gateway def1 bypass-dhcp"
		#DNS for client
			push "dhcp-option DNS 8.8.8.8"
			push "dhcp-option DNS 8.8.4.4"
			user nobody
			group nobody
			topology subnet

			#remote-cert-eku "TLS Web Client Authentication" (Centos 7)
			;tls-auth ta.key 0
			#tls-crypt myvpn.tlsauth (Centos 7)
_____________________________________________________________START____server.conf
	port 1194
	proto udp
	dev tun
	ca /etc/openvpn/easy-rsa/keys/ca.crt
	cert /etc/openvpn/easy-rsa/keys/server.crt
	key /etc/openvpn/easy-rsa/keys/server.key
	dh /etc/openvpn/easy-rsa/keys/dh2048.pem
	topology subnet
	server 10.8.0.0 255.255.255.0
	ifconfig-pool-persist ipp.txt
	push "redirect-gateway def1 bypass-dhcp"
	push "dhcp-option DNS 8.8.8.8"
	push "dhcp-option DNS 8.8.4.4"
	keepalive 10 120
	cipher AES-256-CBC
	user nobody
	group nobody
	persist-key
	persist-tun
	status openvpn-status.log
	log         openvpn.log
	verb 3
	explicit-exit-notify 1
__________________________________________________________________END__server.conf
	systemctl restart openvpn@server.service

	#Generate kay (if You useing TLS auth)
		openvpn --genkey --secret /etc/openvpn/myvpn.tlsauth

	#STEP 3 Generating Keys and Certificates
			mkdir /etc/openvpn/easy-rsa/keys	
		#vim /etc/openvpn/easy-rsa/vars

			cd /etc/openvpn/easy-rsa
			source ./vars
			./clean-all
			./build-ca
			./build-key-server server
			./build-dh
		#This may take a few minutes to complete.

			cd /etc/openvpn/easy-rsa/keys
			cp dh2048.pem ca.crt server.crt server.key /etc/openvpn
		#Create client sertificates
			cd /etc/openvpn/easy-rsa
			./build-key client
			#cp /etc/openvpn/easy-rsa/openssl-1.0.0.cnf /etc/openvpn/easy-rsa/openssl.cnf (Centos 7)

		#Note!!! the path to all keys must be relative
			ca /etc/openvpn/easy-rsa/keys/ca.crt
			cert /etc/openvpn/easy-rsa/keys/server.crt
			key /etc/openvpn/easy-rsa/keys/server.key
			dh /etc/openvpn/easy-rsa/keys/dh2048.pem

	#Step 4 — Routing
		#To have NAT on clien need add route on a server by using iptables
			iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
			service iptables save
			service iptables restart
		#Then, enable IP Forwarding in sysctl:
			vim /etc/sysctl.conf
			#add
			#Controls IP packet forwarding
			net.ipv4.ip_forward = 1
		#Finally, apply our new sysctl settings. Start the server and assure that it starts automatically on boot:
			sysctl -p
			service openvpn start
			chkconfig openvpn on

	#Step 5 - Configure client (OpenVPN client for Mac "Tunnelblick")
		#the client.crt and client.key files will are automatically named based on the parameters used with "./build-key" earlier

..............................................................START...client.ovpn
	client
	dev tun
	proto udp
	remote ip-address port
	resolv-retry infinite
	nobind
	persist-key
	persist-tun
	verb 3
	ca /Users/john/Documents/vpn_key/ca.crt
	cert /Users/john/Documents/vpn_key/client.crt
	key /Users/john/Documents/vpn_key/client.key
	;tls-crypt /Users/john/Documents/vpn_key/myvpn.tlsauth
	redirect-gateway def1 block-local

..............................................................START...client2.ovpn
	client 2
	dev tun
	proto udp
	remote ip-address port
	resolv-retry infinite
	nobind
	persist-key
	persist-tun
	verb 3
	ca /Users/john/Documents/vpn_key/ca.crt
	cert /Users/john/Documents/vpn_key/client2.crt
	key /Users/john/Documents/vpn_key/client2.key
	redirect-gateway def1 block-local
.................................................................END..client2.ovpn

	#Add additional route to client to connect another subnte
		push "route 198.168.10.0 255.255.255.0" (add 198.168.10.0 subnet to client)
		push "route 198.168.5.0 255.255.255.0" (add 198.168.5.0 subnet to client)
	#mkdir ~/vpn-keys/

	ca /Users/john/Documents/vpn_key/ca.crt
	cert /Users/john/Documents/vpn_key/client.crt
	key /Users/john/Documents/vpn_key/client.key



___qWireGuard_______________________________________________________________________________________________________
--------------------------------------------------------------------------------------------------------------------
	#Install Centos7
	#https://www.wireguard.com/install/#red-hat-enterprise-linux-7-centos-7-module-tools
	#https://www.cyberciti.biz/faq/ubuntu-20-04-set-up-wireguard-vpn-server/

	sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	sudo curl -o /etc/yum.repos.d/jdoss-wireguard-epel-7.repo https://copr.fedorainfracloud.org/coprs/jdoss/wireguard/repo/epel-7/jdoss-wireguard-epel-7.repo
	sudo yum install wireguard-dkms wireguard-tools


	#install server then install desctop ubuntu (link install any ubuntu desctop - https://www.makeuseof.com/install-desktop-environment-gui-ubuntu-server/)
		sudo apt install ubuntu-desktop


	#Install wireguard (https://www.the-digital-life.com/wireguard-installation-and-configuration/)
	# https://www.youtube.com/watch?v=bVKNSf1p1d0

	1. Install WireGuard on Server and Client
		sudo apt update && sudo apt install wireguard
	2. Generate keys on Server and Client
		wg genkey | tee privatekey | wg pubkey > publickey
	3. Configure server (/etc/wireguard/wg0.conf)
		----------------------------------------------------------------------------------------------------------------
		[Interface]
		PrivateKey=<server-private-key>
		Address=<server-ip-address>/<subnet>
		SaveConfig=true
		PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o <public-interface> -j MASQUERADE;
		PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o <public-interface> -j MASQUERADE;
		ListenPort = 51820
		----------------------------------------------------------------------------------------------------------------

		---------------Example------------------------------------------------------------------------------------------
		[Interface]
		Address = 10.0.0.1/8
		PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE;
		PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE;
		ListenPort = 51820
		PrivateKey = cMe2MDoXJ8be09bYhd0azj/PPJEPGi1riHEpnYP8lVI=

		[Peer]
		PublicKey = J+RoaunI4NENqLrRMsc3ofz/cfW/KYa3etpRfFHhAmY=
		AllowedIPs = 10.0.0.2/32
		----------------------------------------------------------------------------------------------------------------
	4. Start wg on Server
			wg-quick up wg0
		#or for Mac OS
			wg-quick up ~/.config/wireguard/wg0.conf

	5. Configure client (/etc/wireguard/wg0.conf)
		----------------------------------------------------------------------------------------------------------------
		[Interface]
		PrivateKey = <client-private-key>
		Address = <client-ip-address>/<subnet>

		[Peer]
		PublicKey = <server-public-key>
		Endpoint = <server-public-ip-address>:51820
		AllowedIPs = 0.0.0.0/0
		----------------------------------------------------------------------------------------------------------------
		[Interface]
		Address = 10.0.0.2/8
		PrivateKey = eIurbkxVvISdOuiGnr4fGkjofbvueoC/qeNGy45l61A=

		[Peer]
		PublicKey = fUDlm68ko0wxGBNA7cWj1V1sBoH5bQl8suCioYRVjzU=
		AllowedIPs = 0.0.0.0/0
		Endpoint = 94.130.176.20:51820
		PersistentKeepalive = 30
		----------------------------------------------------------------------------------------------------------------

	6. Start wg on Client
			wg-quick up wg0
		#or for Mac OS
			wg-quick down ~/.config/wireguard/wg0.conf
	
	7. Configure on server allow ip forward in cat /proc/sys/net/ipv4/ip_forward set "1"
		sudo sysctl -w net.ipv4.ip_forward=1
		sudo sysctl -p

	---------------------------------------------------------------------------------------------------------------------
	#Client for Mac - https://medium.com/@headquartershq/setting-up-wireguard-on-a-mac-8a121bfe9d86
	


___qVNC on remoute server__________________________________________________________________________________________
	#install vnc server
		https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-20-04
	#make tunel for vnc viewer on local pc
		ssh -i /Volumes/reserv/save/.ssh/id_rsa -L59000:localhost:5901 root@ip-address -Nf

	#kill VNC
		vncserver -kill :1

	#start VNC on localhost
		vncserver -localhost


___qbase64__________________________________________________________________________________________________________
	#encode to base64 ---> echo password | base64
	#decode from base64 ---> echo 'password' | base64 --decode



___qnetwork____________________________NETWORK______________________________________________________________________
--------------------------------------------------------------------------------------------------------------------
	#Change hostname - https://www.tecmint.com/set-change-hostname-in-centos-7/
	#network (NFS - network file system)
		watch 'netstat -na | grep -v 8080 | grep tcp | sort|grep EST'

	#check all connection status
		netstat -tan | awk '{print $6}' | sort | uniq -c

	#Disable ping icmp (twoside ping)
		https://xakinfo.ru/os/kak-ubrat-opredelenie-tunnelja-dvustoronnij-ping-v-vpn/
		iptables -A INPUT --proto icmp -j DROP

	#Show all open ports
		netstat -nap | grep LISTEN
		or
		netstat -lntup

	#Restart network
		service network restart

	#Show all ip adress on server
		ip addr show

	#Show route table in mac os
		netstat -rn

	#Chenge default route
		route delete default
		route add default 192.168.0.1
	or
		route change default -interface $INTF
		route change 192.168.0.0/16 -interface $INTF

	#Configure Network config (Red Hat)
		#https://www.cyberciti.biz/faq/howto-setting-rhel7-centos-7-static-ip-configuration/
		/etc/sysconfig/network-scripts/ifcfg-eth0
		IPADDR=192.168.1.200
		NETMASK=255.255.255.0
		GATEWAY=192.168.1.1
		DNS1=1.0.0.1
		#or use ---> nmtui

	#DISABLE ipv6 in Red Hat
		sysctl -w net.ipv6.conf.all.disable_ipv6=1
		sysctl -w net.ipv6.conf.default.disable_ipv6=1

	#Disable ipv6 (Ubuntu)
		sudo sh -c 'echo 1 > /proc/sys/net/ipv6/conf/eth0/disable_ipv6'

	#Disable ipv6 (Android)
		echo 0 > /proc/sys/net/ipv6/conf/wlan0/accept_ra
		echo 1 > /proc/sys/net/ipv6/conf/wlan0/disable_ipv6

	#Conect WI-FI via terminal ---> http://rus-linux.net/MyLDP/consol/wifi-from-command-line.html
	
	#Set DNS 		   ---> /etc/resolv.conf

		netstat -nlpt (t - TCP pocket, p - show program using connection, l - Show only listening sockets)

	#REAL TIME network monitor ---> watch lsof -i
	#REAL TIME network monitor ---> watch 'netstat -nap | grep -v 8080 | grep tcp | sort'

	#Show all route        ---> netstat -rn
	#Show all route        ---> route -n
	#Show default geatways ---> ip route show 

	#Show specifications of WI-FI, ethernet -->ethtool -i [wlan0]

	#To show what SERVICE using internet -->lsof -P -i -n | cut -f 1 -d " " | uniq
	#or netstat -nap | grep -v '127.0.0.1' | grep tcp | grep -v nginx | grep -v ':80'

___qtcpdump__________________________________________________________________________________________________________
#Whatch all dns querys with tcpdump
	sudo tcpdump -i en0 port 53

___qwine_____________________________________________________________________________________________________________
#install
	brew cask install wine-stable

___qbrew____________________________________________________________________________________________________________
#brew install from source example
	brew install --build-from-source ffmpeg


___qterminal (qbash) _________________OTHER COMMANDS___________________________________________________________________
-----------------------------------------------------------------------------------------------------------------------
	#To watch / show function or other in bash 
		type [function]
		#Example ---> type youfunction
		#Show all function decleared in bash ---> typeset -f

	#Run breoser vis ssh
		export DISPLAY=:0	
		firefox &

	#Show execute process (trasert mode)
			set -x
		#Turn off
			set +x

	#Get only the file name without path
		basename

		"A && B" Run B if A succeeded
		"A || B" Run B if A failed
		"A &" Run A in background.

	#To know wich command belong to wich packege (watch)
		dpkg -S 'which watch'

	#Return to previous directory/folder ---> cd -

    	#Do 1+1
       		ttt=$((1+1));echo $ttt

    	#Current time/date Plus one hour
       		echo -n $(($(date +'%l')+1));echo -n $(date +' :%m %p')' '; echo -n $(date +'%A %d');

    	#Cut last symbol in line
       		sed 's/.$//'

    	#Check if var not null
		if [[ -z $var ]];then echo 'var is null';fi
		if [[ -n $var ]];then echo 'var is not null';fi

    	#How to check if a string contains a substring in Bash
		string='My long string'
		if [[ $string == *"My long"* ]]; then
  		echo "It's there!"
		fi
    	#or
		string='My long string';if test "$string" = *"My long"*;then echo "It's there!";fi

    	#Check if a File or Directory Exists (https://linuxize.com/post/bash-check-if-file-exists/)
		if [[ -f "$FILE" ]]; then
     			echo "$FILE exist"
 		fi

    	#Replace (http://qaru.site/questions/4981/how-to-use-double-or-single-brackets-parentheses-curly-braces)
		sed -i -e 's|old_text|new_text|g' path_to_file
    	#Replace with echo
		$ var="abcde"; echo ${var/de/12}
		abc12

	#Test redirect
		curl -I http://converterlab.net/

	#qbackground works
	#run script in background ---> nohup command &>/dev/null &

	#watch jobs in background ---> jobs
	#Moving Background Processes to the Foreground ---> fg %number-job
	#Moveng the processes to background ---> CTRL-Z
	#Start pocesses in the foreground ---> bg 
	#Kill background processes ---> watch number (jobs) then ---> kill %number-job 

	#cursor navigation in terminal	
		^A: go to the beginning of line
		^E: go to the end of line
		Alt-B: skip one word backward
		Alt-F: skip one word forward
		Esc-B: skip one word backward (on MacOS)
		Esc-F: skip one word forward (on MacOS)
		^U: delete to the beginning of line
		^K: delete to the end of line
		Alt-D: delete to the end of word
		^J: like Enter
		^H: delete symbol like backspace
		^W: delete word
		^U: delete line
		^\: more powerfull than ^C


	#qlogs all logs here (/etc/rsyslog.conf)
	#log rotation here /etc/logrotate.d/*
		https://www.digitalocean.com/community/tutorials/how-to-manage-logfiles-with-logrotate-on-ubuntu-16-04

	#qtime 	     ---> date
	#change time ---> date MMDDhhmmCCYY.ss 
	(MM - month, hh - hours, mm - minutes, CCYY - year, ss - second)


___qsort_________________________________________________
	sort -k2 -n -t '['

	-k sort by column number
	-n sort by numbers (from 1 to 9)
	-t custom separator

___qcut__________________________________________________
	cut -d ">" -f4
	-d delete separator
	-f the number of columb that shows after

___quniq_________________________________________________
	uniq -c 
	-c count uniq element

___qawk__________________________________________________
	#about qawk - https://www.shellhacks.com/awk-print-column-change-field-separator-linux-bash/
		$1 - take first field
		$(NF) - take the last field
		$(NF-1) - take the last -1 field

	#print column 1 and 2 with " - " separator, and prints column also seperated with space
		awk -F ' ' '{print $1" - "$2}'

	#take only the last field
		awk -F ' ' '{print $NF}' 

	#take the last - 1 field
		awk -F ' ' '{print $(NF-1)}' 

___qln________qsymlink___________________________________
	#Make symlinked ---> (sudo ln -s /symlink  /folder)
	#Show the path for ln symbollind ---> readlink -f symlinkName

	------------------qpermission files and dirs---------------------------------------
	#To change all the directories to 755 (drwxr-xr-x) ---> find /opt/lampp/htdocs -type d -exec chmod 755 {} \;
	#To change all the files to 644 (-rw-r--r--) 	   ---> find /opt/lampp/htdocs -type f -exec chmod 644 {} \;
	------------------------------------------------------------------------------

	#add user
		adduser admin
		passwd admin

	#change default user home folder ---> usermod -d /new/home admin

	#Watch all service
		chkconfig --list
		chkconfig nginx on
		systemctl list-unit-files | grep enabled
	#show is service (mariadb) is active service
		systemctl is-active mariadb

	#show cpu in mac	     ---> sysctl -n machdep.cpu.brand_string

	#RUN command from ROOT ---> su -c command root
	#Watch Last reboot     ---> date -d "$(</proc/uptime awk '{print $1}') seconds ago"
	#error journal 		    ---> journalctl -xe
	#Ram Test memory test linux ---> memtester 1024 1


___qLocale____________________________________________________________________                         
	https://www.shellhacks.com/linux-define-locale-language-settings/

	#list of all locales
		localedef --list-archive

	#Installed locales
		locale -a | grep hi_IN.utf8

	#set language for current session
		LANG=en_US.utf8 or LANG=ru_RU.utf8

	#In Centos add/create to /etc/environment
		LANG=en_US.UTF-8
		LC_ALL=en_US.UTF-8


___qSCREEN____________________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------------
	#run command at every windows (execute from screen with :[command]) ---> at "#" stuff "source ~/Documents/save/1/alias"
        #execute command for window 0 ---> screen -S SessionName -p0 -X stuff "command"$(echo -ne '\015')
		screen -S ses -p [0-4] -X stuff "ls -la"$(echo -ne '\015')
		screen -S ses -X at "#" stuff "c"$(echo -ne '\015')

	#Set password for screen session ---> ^A and type ":password"
	#config file /etc/screenrc
	#kill session 	 --->  screen -X -S [session # you want to kill] quit
	#or 		 ---> ^D then "\"
	#kill all screen ---> pkill screen
	#connect to session ---> screen -x [screen session]
	#New screen session ---> screen -S [new session name]
	#Rename screen session ---> :sessionname [name]
		#or ---> screen -S old_session_name -X sessionname new_session_name
	#Move windows to another number ---> :number 7 (in move current windows to '7' and wndows '7' to current windows)
	#Execute command to every windows ---> :at "#" stuff "ls -l^M"

	#WATCH HELP ^A then ?	
	#To split vertically 		  ---> ^A |.
	#To split horizontally 		  ---> ^A S
	#To unsplit 			  ---> ^A Q
	#Resize				  :resize 20
	#Split				  :split
	#Unsplit current window		  :remove
	#To switch from one to the other  ---> ^A then Tab (or ^I)
	#To close the pane that has focus ---> ^A X
	#Set windows title 		  ---> :title NewWindowsTitle
	#Lock session			  ---> ^A x
	#Kill window 			  ---> ^A k or (Ctrl-a :kill)
	#monitor window for silence	  ---> C-a _
	#redraw window 			  ---> C-a C-l
	#enable logging in the screen session ---> C-a H 
	#change to window by number or name ---> C-a ' <number or title>

	http://www.softpanorama.org/Utilities/Screen/screenrc_examples.shtml
.................................................................................................START......vim ~/.screenrc
#terminfo * F9=\E[20~Q
#bindkey -k F9 next
#
#password ba1f2511fc30423bdbb183fe33f3dd0f
#escape ``
escape ^Kk
#escape Ctrl+\
#escape ^\\

bind = resize =
bind + resize +2
bind - resize -2
bind _ resize max

bindkey -k k1 :"pwd"
bind '"\e[24~":"foobar"'
set password none


#screen -t net 
#screen -t 1 #irssi
#screen -t 2
#screen -t 3
#screen -t 4
altscreen on
term screen-256color
bind ',' prev
bind '.' next
#
#change the hardstatus settings to give an window list at the bottom of the
#screen, with the time and date and with the current window highlighted
hardstatus alwayslastline
#hardstatus string '%{= kG}%-Lw%{= kW}%50> %n%f* %t%{= kG}%+Lw%< %{= kG}%-=%c:%s%{-}'
hardstatus string '%{= kG}[ %{G}%H %{g}][%= %{= kw}%?%-Lw%?%{r}(%{W}%n*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%= %{g}][%{B} %m-%d %{W}%c %{g}]'
#caption always "%{= kw}%-w%{= gW}%n %t%{-}%+w %-= activity - %Y-%m-%d %C:%s"
.......................................................................................................................END.
.................................................................................................................screenrc centos
escape ^Jj
bind = resize =
bind + resize +1
bind - resize -1
bind _ resize max

caption always "%{= kw}%-w%{= gW}%n %t%{-}%+w %-= activity - %Y-%m-%d %C:%s"
...................................................................................................................................
	
 
 	#New terminal 		---> ^A c
	#Next windows 		---> ^A space.
	#Previous windows	---> ^A backspace.
	#Next windows 		---> ^A then [n]. (works for n∈{0,1…9})
	#Switch between terminals using list ---> ^A+a then " (useful when more than 10 terminals)
	#Send ^A to the underlying terminal ^A then a.


___qarh (q7zip,tar.gz)_qtar__qzip______________________________________________________________________________________
-----------------------------------------------------------------------------------------------------------------------
	#Unarhiv 7zip file with password (q7z,qz7)
	7z -pPASSWORD x file.zip

	#pack   ---> tar -zcvf output_file.tar.gz /foder_what_to_encrypt
	       |   | gzip vds1
	#unpack ---> tar zxvf hydra-4.1-src.tar.gz

	unrar e file.rar
___qDD________________________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------------
	https://habr.com/ru/post/117050/

	#Clone hdd from sda to sdb ---> sudo dd if=/dev/sda of=/dev/sdb bs=1M conv=noerror,sync status=progress
		#noerror - ignore errors
		#sync: сообщает утилите dd о необходимости заполнения блоков нулями в случае возникновения ошибок чтения

	#Delete ALL data on HDD ---> dd if=/dev/zero of=/dev/sda bs=4k
				or ---> dd if=/dev/zero of=/dev/Pd3 bs=1M count=999999999999

	#Delete folder forever ---> for filename in /pathtofolder/*; do dd if=/dev/zero of="$filename" bs=1k count=200024; done

___qJournalCtl________________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------------
	Retain only the past two days	---> journalctl --vacuum-time=2d
	Retain only the past 500 MB	---> journalctl --vacuum-size=500M
	https://unix.stackexchange.com/questions/139513/how-to-clear-journalctl#194058


___qCRON________________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------
	crontab -e
	#Clear traffic statistic at first day of every month ---> 0 0 1 * * iptables -Z INPUT
	#Run script every 20 min ---> */20 * * * * /root/run.sh
	#Run script every 2 days in 23:00 ---> 0 23 */2 * * /root/run.sh

	#restart cron centos
		service crond restart

	@reboot /home/user/startup.sh

	open crontab by nano ---> export VISUAL=nano; crontab -e
	open crontab by Vim  ---> export VISUAL=vim; crontab -e

	Job every 6 hours ---> 0 */6 * * * /usr/bin/php /script.php
	drop count of traffic for INPUT to ZERO every first day each month ---> 0 0 1 * * iptables -Z INPUT
	

	SHELL=/bin/bash
	PATH=/sbin:/bin:/usr/sbin:/usr/bin
	MAILTO=root
	# For details see man 4 crontabs

	# Example of job definition:
	# .---------------- minute (0 - 59)
	# |  .------------- hour (0 - 23)
	# |  |  .---------- day of month (1 - 31)
	# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
	# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
	# |  |  |  |  |
	# *  *  *  *  * user-name  command to be executed

___qsyncthing___________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------
	brew install syncthing
	http://127.0.0.1:8384/
	#Action -> Settings -> Connection -> disable "Enable Relaying"
___iTerm________________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------
	Preferences -> Advanced -> search "restor" and turnoff all
	split pane vertical ---> cmd+d
	split pane horisontal ---> Shift+cmd+d
	kill, unsplit pane ---> cmd+w
	list pane ---> cmd+[ and cmd+]
	Bind Key -> Preferences -> Key -> Plus (+) -> Run Coprocess -> Run Commane and Keyboard Shortcuts

___qmpv_________________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------
	brew uninstall ffmpeg
	brew install mpv
	#record video from youtube with mpv ---> mpv --record-file=video.mkv https://www.youtube.com/watch?v=…
	#play video firm youtube with mpv ---> mpv --screenshot-directory=~/Documents/save/1/img/ --autofit=40% --ytdl-format=22 --ontop https://www.youtube.com/watch?v=…
	#install mpv on centos ---> https://snapcraft.io/install/mpv/centos
	#install mpv-video-cutter   

	................................................. mpv.conf..............................................
		screenshot-directory="~/save/1/img"
		screenshot-format=png
		screenshot-template="%F:%P"
		volume=0
		save-position-on-quit
		profile=pseudo-gui
		set F1 vf toggle rotate=1
		set r vf add rotate=90
		set l vf toggle mirror
		RIGHT seek 3
		LEFT seek -3
	........................................................................................................
	#set to ~/.config/mpv/input.conf instead LEFT and RIGHT
		l no-osd seek  1 exact
		h no-osd seek  -1 exact

___Terminal_____________________________________________________________________________________________________
----------------------------------------------------------------------------------------------------------------
	Preferencec -> Window -> Restore text when reopening windows (Turn it off)



	#crontab on server_______________________________________________________________________________
20 4 * * 1 /usr/bin/certbot renew >> /var/log/le-renew.log
25 4 * * 1 /usr/bin/systemctl restart nginx

