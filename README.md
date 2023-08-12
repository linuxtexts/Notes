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
