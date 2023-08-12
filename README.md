___qsc___________________________Spreadsheet Calculator_____________________________________________________________

        https://github.com/n-t-roff/sc
        https://www.maketecheasier.com/linux-command-line-spreadsheets/
        https://www.linuxjournal.com/article/10699
        http://n-t-roff.github.io/sc.1.html

        git clone https://github.com/n-t-roff/sc.git
        cd sc && ./configure && make && make install

        #help - ?
        #save file      - P
        #edit num cell  - e
        #set format cell - f
        #set color      - ^TC
        #mark cell      - ma or ms or md ...
        #pase           - ca or cs or cd ...

        gB13 — go to cell B13.
        ir, ic — insert row, insert column.
        ma (mb, mc and so on) — “mark” cell as a (or b, or c and so on).
        ca (cb, cc and so on) — copy contents previously marked with ma.
        Ctrl-f, Ctrl-b — page up or down (also pgup, pgdown).
        dr, yr, pr — delete row, yank row, put row.
        dc, yc, pc — delete column, yank, put column.
        dd, yd, pd — delete, yank, put a cell.
        = — enter a numeric value (25 or F13-D14) or formula (@sum(A2:A145)).
        < — insert left-justified text.
        \ — insert centered text.
        > — insert right-justified text.
        x — remove cell.
        W<filename.asc> — write plain-text file.
        P<filename.sc> — write an .sc file.
        G<filename.sc> — read (“get”) an .sc file.
        Zr, Zc — zap (hide) row or column.



___qsyncthing___________________________________________________________________________________________________

	brew install syncthing
	http://127.0.0.1:8384/
	#Action -> Settings -> Connection -> disable "Enable Relaying"
 
___qiTerm_______________________________________________________________________________________________________

	Preferences -> Advanced -> search "restor" and turnoff all
	split pane vertical ---> cmd+d
	split pane horisontal ---> Shift+cmd+d
	kill, unsplit pane ---> cmd+w
	list pane ---> cmd+[ and cmd+]
	Bind Key -> Preferences -> Key -> Plus (+) -> Run Coprocess -> Run Commane and Keyboard Shortcuts

___qmpv_________________________________________________________________________________________________________

	brew uninstall ffmpeg
	brew install mpv
	#record video from youtube with mpv ---> mpv --record-file=video.mkv https://www.youtube.com/watch?v=…
	#play video firm youtube with mpv ---> mpv --screenshot-directory=~/Documents/save/1/img/ --autofit=40% --ytdl-format=22 --ontop https://www.youtube.com/watch?v=…
	#install mpv on centos ---> https://snapcraft.io/install/mpv/centos
	#install mpv-video-cutter   

	................................................. ~/.config/mpv/mpv.conf................................
 
	screenshot-directory="~/Documents/save/img"
	screenshot-format=png
	screenshot-template="%F:%P"
	volume=0
	save-position-on-quit
	profile=pseudo-gui
	........................................................................................................
	#set to ~/.config/mpv/input.conf instead LEFT and RIGHT
		l no-osd seek  1 exact
		h no-osd seek  -1 exact

___Terminal_____________________________________________________________________________________________________

	Preferencec -> Window -> Restore text when reopening windows (Turn it off)

        e — edit a numeric value.
        E — edit a string value.


___qIPTABLES_________________________________________________________________________________________________

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

  ___qDDOS______________________________________________________________________________________________________________

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
					
		___qlogtop_____________________________________________________________________________________________

		#What does logtop shows?
		#line1 - Just number
		#line2 - amount of query from ip
		#line3 - amount of query from ip in one second
		#line4 - ip adress
					
		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $1; fflush();'} | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | cut -d ' ' -f 1 | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $7; fflush();'} | logtop

		--------------------------------------------------------------------------------------------------------

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

___qTEST SERVER__________________________________________________________________________________________________________

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


___qhistory_________________________________________________________________________________________________________________

	#Show history ---> history
	#Search in history ---> history | grep word
	#Reply command number 3 ---> !number (!3)


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
	#Show all date for file ---> exiftool -time:all -a -G0:1 -s file.txt
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


___qWireGuard_______________________________________________________________________________________________________

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
		ssh -i ~/.ssh/id_rsa -L59000:localhost:5901 root@ip-address -Nf

	#kill VNC
		vncserver -kill :1

	#start VNC on localhost
		vncserver -localhost


___qbase64__________________________________________________________________________________________________________

	#encode to base64 ---> echo password | base64
	#decode from base64 ---> echo 'password' | base64 --decode

___qsshfs___________________________________________________________________________________________________________

	#Install sshfs on Centos 7
		yum install epel-release
		yum install fuse-sshfs
	#Mount
		sshfs foobar@remote.server:/home/foobar /mnt

___qnetwork____________________________NETWORK______________________________________________________________________

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

___qwindows make usb with windows setup__________________________________________________

	diskutil eraseDisk MS-DOS "WIN10" MBR /dev/disk2
	diskutil eraseDisk MS-DOS "WIN10" GPT /dev/disk2
	#or
	diskutil eraseDisk ExFAT "WIN10" GPT /dev/disk2

	hdiutil mount ~/Downloads/Win10_1903_V1_English_x64.iso
	cp -rp /Volumes/CCCOMA_X64FRE_EN-US_DV9/* /Volumes/WIN10/	

	#remove hebirnate
		sudo pmset -a sleep 0; sudo pmset -a hibernatemode 0; sudo pmset -a disablesleep 1;
	#enable 
		sudo pmset -b disablesleep 0;sudo pmset -b sleep 5;

___qUbuntu_________________________________________________________________________________________

	#Replace Alt and command (the interrupt command will replaced from Ctrl+c to Shift+Ctrl+c)

	............................................................................................. vim ~/.Xmodmap
	clear control
	clear mod4

	keycode 105 =
	keycode 206 =

	keycode 133 = Control_L NoSymbol Control_L
	keycode 134 = Control_R NoSymbol Control_R
	keycode 37 = Super_L NoSymbol Super_L

	add control = Control_L
	add control = Control_R
	add mod4 = Super_L
	............................................................................................................
	xmodmap ~/.Xmodmap

	#Disable Double Tap (need install synclient) https://askubuntu.com/questions/939065/disable-double-tap-but-keep-one-tap-to-click-enabled-on-touchpad
	synclient MaxTapMove=2

	#run script start.sh at boot
		echo "script body" > ~/start.sh
		cp ~/start.sh /etc/init.d/start.sh
		sudo update-rc.d start.sh defaults

	---------------------------------------------------------
	install Hardware Sensor Indicator
	------------------------------------------------------------------------------------------
	qi3 windows tail manager (HOTKEYS - https://i3wm.org/docs/refcard.html)
	tail manager yabai - https://github.com/koekeishiya/yabai/wiki/Disabling-System-Integrity-Protection
	sudo apt install i3
	/usr/bin/i3-config-wizard

___qterminal (qbash) _________________OTHER COMMANDS___________________________________________________________________

	#To watch / show function or other in bash 
		type [function]
		#Example ---> type youfunction
		#Show all function decleared in bash ---> typeset -f

	#Run browser vis ssh
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
